## Access Role / Access Rights Technical Document for CRM Architecture

### 1. **Introduction**
This document defines the role-based access control (RBAC) structure and access rights for the CRM system designed for a real estate brokerage. It ensures that users have appropriate permissions to access the necessary resources, based on their role within the system. This is aligned with the overall architecture plan, including the frontend, backend, database, and integrations.

### 2. **Roles and Permissions Overview**
The CRM system will utilize Role-Based Access Control (RBAC) to restrict access based on user roles. Below is a breakdown of the primary roles and their associated permissions:

- **Admin**
- **Agent**
- **User**

### 3. **Role Definitions**

#### 3.1 **Admin**
Admins are responsible for overseeing all aspects of the CRM system, including managing agents, users, properties, and the entire workflow. They have the highest level of access and can perform administrative tasks.

- **Permissions:**
  - **Manage Users**: Add, edit, and delete agent and user profiles.
  - **Manage Properties**: Add, edit, delete, and approve property listings.
  - **Manage Leads**: Assign, update, and delete leads.
  - **Access Analytics**: View reports and metrics on agents, properties, and campaigns.
  - **Configure System Settings**: Configure email/SMS notifications, integrations with MLS, and third-party services.
  - **API Access**: Full access to all backend APIs.
  - **View and Approve Transactions**: View details of all ongoing transactions and approve deals.
  - **Monitor and Audit Logs**: Access real-time and historical logs for security monitoring and auditing.
  - **Backup and Restore**: Access to perform data backups and restores.

#### 3.2 **Agent**
Agents are real estate professionals responsible for managing leads, properties, and user interactions. Their access is limited to tasks related to lead management and property listings.

- **Permissions:**
  - **Manage Leads**: Create, view, and update leads. Assign leads to other agents.
  - **Manage Properties**: View, update, and add properties they are working on.
  - **Access User Information**: View user details but cannot modify them unless authorized.
  - **View Analytics**: Access reports and dashboards related to their assigned leads and properties.
  - **Communication Tools**: Use tools like Twilio (SMS/Email) for user communication.
  - **Transaction Coordination**: View and manage their assigned transactions, but cannot approve deals.
  - **Notifications**: Receive real-time notifications about new leads, messages, and updates.

#### 3.3 **User**
Users interact with the CRM system mainly through the frontend, viewing property listings, and submitting leads or inquiries.

- **Permissions:**
  - **View Properties**: Search, filter, and view property listings and details.
  - **Submit Inquiries**: Submit inquiries about properties they are interested in.
  - **Manage Profile**: View and edit their personal profile details (e.g., contact information).
  - **Lead Status**: View the status of their inquiries and track responses from agents.
  - **Notifications**: Receive notifications about new properties and updates from agents.

---

### 4. **Access Rights to System Components**

#### 4.1 **Frontend Access Rights**
Frontend users interact with the CRM through WordPress interfaces. Access to various UI components is controlled based on the user’s role.

- **Admin**: Full access to all frontend modules (dashboard, lead management, property listings, etc.).
- **Agent**: Access to agent-specific dashboards, lead management, and property details.
- **User**: Read-only access to property listings and their personal inquiry details.

#### 4.2 **Backend API Access**
Access to backend resources is granted through WordPress RESTful APIs, with specific routes secured by role-based access.

- **Admin**: Full access to all API endpoints, including sensitive user management and transaction approval.
- **Agent**: Limited access to their assigned leads, properties, and transactions.
- **User**: Restricted to APIs related to submitting inquiries and viewing properties.

#### 4.3 **Database Access Rights**
The database uses WordPress’s MySQL/MariaDB for structured data. Access control ensures that users can only access data that pertains to their role.

- **Admin**: Full CRUD (Create, Read, Update, Delete) access to all database tables (Users, Leads, Properties, Transactions, etc.).
- **Agent**: CRUD access to leads and properties they are associated with. Read-only access to transactions and other agents' data.
- **User**: Read-only access to their personal data and inquiry statuses.

#### 4.4 **Third-Party Integration Access**
The CRM integrates with MLS, email/SMS services (Twilio, SendGrid), and cloud storage. Access to these integrations will be limited to specific roles as follows:

- **Admin**: Full control over third-party service integrations, including configuration and troubleshooting.
- **Agent**: Limited to using Twilio and SendGrid for communication with users.
- **User**: No access to third-party integrations.

#### 4.5 **Authentication and Security**
The system uses WordPress authentication and user management to ensure secure access. Permissions are validated based on the role assigned during the authentication process.

- **Admin**: Full access to configure and manage user roles and authentication settings.
- **Agent**: Access to their role-specific features based on authentication.
- **User**: Access is granted only to view properties and manage their profile after successful login.

### 5. **Access Control List (ACL)**
Below is a summarized Access Control List (ACL) matrix that maps each user role to specific actions within the system:

| **Module**                   | **Admin** | **Agent** | **User** |
|------------------------------|-----------|-----------|----------|
| **Dashboard**                 | Full      | View      | N/A      |
| **User Management**           | Full      | N/A       | N/A      |
| **Property Management**       | Full      | CRUD      | View     |
| **Lead Management**           | Full      | CRUD      | Submit   |
| **Transaction Management**    | Full      | View      | N/A      |
| **Reporting & Analytics**     | Full      | View      | N/A      |
| **Email/SMS Communication**   | Full      | Send      | Receive  |
| **MLS Integration**           | Full      | View      | N/A      |
| **System Settings**           | Full      | N/A       | N/A      |
| **Backup and Restore**        | Full      | N/A       | N/A      |
| **Cloud Storage**             | Full      | View      | N/A      |
| **Real-Time Notifications**   | Full      | Receive   | Receive  |

### 6. **Audit Logging and Monitoring**
- **Admins** will have access to comprehensive system logs, including login attempts, data access, and system changes.
- **Agents** and **Users** will have restricted access to logs related to their own actions, e.g., their lead interactions and property views.

---

### 7. **Conclusion**
This RBAC system ensures that users have appropriate access to the CRM system based on their role. It maintains a secure and efficient flow of data and operations, while enabling the right level of access for agents, admins, and users. Access control is enforced throughout the system, from frontend interfaces to backend services and third-party integrations.

