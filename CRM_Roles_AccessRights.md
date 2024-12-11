## Access Role / Access Rights Technical Document for CRM Architecture

### 1. **Introduction**
This document defines the role-based access control (RBAC) structure and access rights for the CRM system designed for a real estate brokerage. It ensures that users have appropriate permissions to access the necessary resources, based on their role within the system. This is aligned with the overall architecture plan, including the frontend, backend, database, and integrations.

### 2. **Roles and Permissions Overview**
The CRM system will utilize Role-Based Access Control (RBAC) to restrict access based on user roles. Below is a breakdown of the primary roles and their associated permissions:

- **Admin**
- **Agent**
- **Client**
- **System (Backend)**

### 3. **Role Definitions**

#### 3.1 **Admin**
Admins are responsible for overseeing all aspects of the CRM system, including managing agents, clients, properties, and the entire workflow. They have the highest level of access and can perform administrative tasks.

- **Permissions:**
  - **Manage Users**: Add, edit, and delete agent and client profiles.
  - **Manage Properties**: Add, edit, delete, and approve property listings.
  - **Manage Leads**: Assign, update, and delete leads.
  - **Access Analytics**: View reports and metrics on agents, properties, and campaigns.
  - **Configure System Settings**: Configure email/SMS notifications, integrations with MLS, and third-party services.
  - **API Access**: Full access to all backend APIs.
  - **View and Approve Transactions**: View details of all ongoing transactions and approve deals.
  - **Monitor and Audit Logs**: Access real-time and historical logs for security monitoring and auditing.
  - **Backup and Restore**: Access to perform data backups and restores.

#### 3.2 **Agent**
Agents are real estate professionals responsible for managing leads, properties, and client interactions. Their access is limited to tasks related to lead management and property listings.

- **Permissions:**
  - **Manage Leads**: Create, view, and update leads. Assign leads to other agents.
  - **Manage Properties**: View, update, and add properties they are working on.
  - **Access Client Information**: View client details but cannot modify them unless authorized.
  - **View Analytics**: Access reports and dashboards related to their assigned leads and properties.
  - **Communication Tools**: Use tools like Twilio (SMS/Email) for client communication.
  - **Transaction Coordination**: View and manage their assigned transactions, but cannot approve deals.
  - **Notifications**: Receive real-time notifications about new leads, messages, and updates.

#### 3.3 **Client**
Clients interact with the CRM system mainly through the frontend, viewing property listings, and submitting leads or inquiries.

- **Permissions:**
  - **View Properties**: Search, filter, and view property listings and details.
  - **Submit Inquiries**: Submit inquiries about properties they are interested in.
  - **Manage Profile**: View and edit their personal profile details (e.g., contact information).
  - **Lead Status**: View the status of their inquiries and track responses from agents.
  - **Notifications**: Receive notifications about new properties and updates from agents.

#### 3.4 **System (Backend)**
The backend role encompasses the core infrastructure and service components that manage the business logic, task queues, integrations, and security operations. This role is not tied to an individual user but to system components that execute operations automatically.

- **Permissions:**
  - **Database Access**: Full access to read, write, and modify the database. This includes both PostgreSQL and MongoDB.
  - **API Integration**: Access to third-party integrations such as MLS and email/SMS services.
  - **Sync Operations**: Control over scheduled sync jobs (e.g., MLS data, email campaigns).
  - **Task Management**: Handle task queues and asynchronous operations (e.g., Bull/Agenda.js).
  - **Authentication Management**: Handle JWT authentication, token generation, and validation.
  - **Security**: Apply and enforce encryption and access control policies across the platform.
  - **Monitoring**: Access to system logs and real-time monitoring tools like Grafana, ELK Stack.

---

### 4. **Access Rights to System Components**

#### 4.1 **Frontend Access Rights**
Frontend users interact with the CRM through React.js or Angular interfaces. Access to various UI components is controlled based on the user’s role.

- **Admin**: Full access to all frontend modules (dashboard, lead management, property listings, etc.).
- **Agent**: Access to agent-specific dashboards, lead management, and property details.
- **Client**: Read-only access to property listings and their personal inquiry details.

#### 4.2 **Backend API Access**
Access to backend resources is granted through RESTful APIs, with specific routes secured by role-based access.

- **Admin**: Full access to all API endpoints, including sensitive user management and transaction approval.
- **Agent**: Limited access to their assigned leads, properties, and transactions.
- **Client**: Restricted to APIs related to submitting inquiries and viewing properties.

#### 4.3 **Database Access Rights**
The database uses PostgreSQL for structured data and MongoDB for unstructured data. Access control ensures that users can only access data that pertains to their role.

- **Admin**: Full CRUD (Create, Read, Update, Delete) access to all database tables (Users, Leads, Properties, Transactions, etc.).
- **Agent**: CRUD access to leads and properties they are associated with. Read-only access to transactions and other agents' data.
- **Client**: Read-only access to their personal data and inquiry statuses.

#### 4.4 **Third-Party Integration Access**
The CRM integrates with MLS, email/SMS services (Twilio, SendGrid), and cloud storage (AWS S3). Access to these integrations will be limited to specific roles as follows:

- **Admin**: Full control over third-party service integrations, including configuration and troubleshooting.
- **Agent**: Limited to using Twilio and SendGrid for communication with clients.
- **Client**: No access to third-party integrations.

#### 4.5 **Authentication and Security**
The system uses JWT-based authentication to ensure secure access. Each role will have different levels of access to the system, and permissions will be validated based on the role assigned during the authentication process.

- **Admin**: Full access to configure and manage user roles and authentication settings.
- **Agent**: Access to their role-specific features based on JWT token validation.
- **Client**: Access is granted only to view properties and manage their profile after successful login.

### 5. **Access Control List (ACL)**
Below is a summarized Access Control List (ACL) matrix that maps each user role to specific actions within the system:

| **Module**                   | **Admin** | **Agent** | **Client** |
|------------------------------|-----------|-----------|------------|
| **Dashboard**                 | Full      | View      | N/A        |
| **User Management**           | Full      | N/A       | N/A        |
| **Property Management**       | Full      | CRUD      | View       |
| **Lead Management**           | Full      | CRUD      | Submit     |
| **Transaction Management**    | Full      | View      | N/A        |
| **Reporting & Analytics**     | Full      | View      | N/A        |
| **Email/SMS Communication**   | Full      | Send      | Receive    |
| **MLS Integration**           | Full      | View      | N/A        |
| **System Settings**           | Full      | N/A       | N/A        |
| **Backup and Restore**        | Full      | N/A       | N/A        |
| **Cloud Storage (S3)**        | Full      | View      | N/A        |
| **Real-Time Notifications**   | Full      | Receive   | Receive    |

### 6. **Audit Logging and Monitoring**
- **Admins** will have access to comprehensive system logs, including login attempts, data access, and system changes.
- **Agents** and **Clients** will have restricted access to logs related to their own actions, e.g., their lead interactions and property views.

---

### 7. **Conclusion**
This RBAC system ensures that users have appropriate access to the CRM system based on their role. It maintains a secure and efficient flow of data and operations, while enabling the right level of access for agents, admins, and clients. Access control is enforced throughout the system, from frontend interfaces to backend services and third-party integrations.
