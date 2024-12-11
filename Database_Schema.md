# **Database Design and Schema for Real Estate CRM**

---

### **1. Introduction**
This document outlines the database design and schema for the custom CRM system tailored to the needs of a real estate brokerage. The system uses PostgreSQL as the primary relational database for structured data storage.

---

### **2. Database Design**

The CRM system's database will consist of several key tables to store data related to users, leads, properties, activities, and notifications. Below is an overview of the major tables and their relationships.

| Table Name       | Key Columns                                          | Purpose                              |
|------------------|-----------------------------------------------------|--------------------------------------|
| Users            | id, name, email, role, password_hash                | Stores user details and roles.       |
| Leads            | id, name, contact_info, assigned_to, status         | Tracks lead information.             |
| Properties       | id, mls_id, address, price, status, agent_id        | Stores property listings.            |
| Activities       | id, user_id, action, timestamp                      | Logs user actions for auditing.      |
| Notifications    | id, recipient_id, message, status, created_at       | Tracks notifications sent to users.  |

---

### **3. Database Schema**

The following provides detailed schema definitions for each of the key tables:

#### **3.1 Users Table**
Stores information about users, including agents, admins, and clients. The `password_hash` column stores the securely hashed password for authentication purposes.

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE,
    role VARCHAR(50),
    password_hash TEXT
);
```

#### **3.2 Leads Table**
Stores data about potential clients or leads, such as their contact details, assigned agent, and lead status.

```sql
CREATE TABLE leads (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    contact_info TEXT,
    assigned_to INTEGER REFERENCES users(id),
    status VARCHAR(50)
);
```

#### **3.3 Properties Table**
Contains information about properties listed by the brokerage, such as MLS ID, address, price, status, and associated agent.

```sql
CREATE TABLE properties (
    id SERIAL PRIMARY KEY,
    mls_id VARCHAR(50) UNIQUE,
    address TEXT,
    price DECIMAL(10, 2),
    status VARCHAR(50),
    agent_id INTEGER REFERENCES users(id)
);
```

#### **3.4 Activities Table**
Logs every action performed by users, including any system changes, updates to leads or properties, and other key activities. Useful for audit trails.

```sql
CREATE TABLE activities (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    action TEXT,
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### **3.5 Notifications Table**
Tracks notifications sent to users. This could include alerts about new leads, property updates, or administrative messages.

```sql
CREATE TABLE notifications (
    id SERIAL PRIMARY KEY,
    recipient_id INTEGER REFERENCES users(id),
    message TEXT,
    status VARCHAR(50),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

### **4. Database Relationships**

- **Users to Leads**: The `assigned_to` field in the `leads` table references the `id` field in the `users` table, linking leads to the users (agents) assigned to them.
  
- **Users to Properties**: The `agent_id` field in the `properties` table references the `id` field in the `users` table, associating properties with the agents managing them.

- **Users to Activities**: The `user_id` field in the `activities` table links each action to the user who performed it.

- **Users to Notifications**: The `recipient_id` field in the `notifications` table references the `id` field in the `users` table, connecting each notification to the user who receives it.

---

### **5. Indexing and Performance Optimization**

To ensure high performance, the following indexes should be considered:

- **Users Table**: Index on `email` to allow fast user lookups by email.
  
  ```sql
  CREATE INDEX idx_users_email ON users(email);
  ```

- **Leads Table**: Index on `assigned_to` for faster query performance when retrieving leads by agent.
  
  ```sql
  CREATE INDEX idx_leads_assigned_to ON leads(assigned_to);
  ```

- **Properties Table**: Index on `mls_id` for quick lookups of properties based on their MLS ID.
  
  ```sql
  CREATE INDEX idx_properties_mls_id ON properties(mls_id);
  ```

- **Activities Table**: Index on `user_id` and `timestamp` to quickly retrieve activity logs for a specific user or time period.
  
  ```sql
  CREATE INDEX idx_activities_user_id ON activities(user_id);
  CREATE INDEX idx_activities_timestamp ON activities(timestamp);
  ```

- **Notifications Table**: Index on `recipient_id` for fast retrieval of notifications for a specific user.
  
  ```sql
  CREATE INDEX idx_notifications_recipient_id ON notifications(recipient_id);
  ```

---

### **6. Conclusion**

This database design ensures efficient data storage and retrieval for the custom CRM system, supporting key functionalities such as lead tracking, property management, user authentication, and activity auditing. By using normalized relational tables and indexing on frequently queried fields, the database will provide fast access to essential data for the real estate brokerage.