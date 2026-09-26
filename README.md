# Leave Management System

A **Service-Oriented Architecture (SOA) and Microservices-based Leave Management System** for managing employee leave applications, manager approvals, authentication, and notifications.

**Domain:** Recruitment & HR
**Course:** SOA Programming and Microservices
**Project:** Set-4 | Project 12

---

## Overview

The system allows employees to apply for leave and managers to approve or reject requests.

The application uses independent microservices for authentication, employee management, leave processing, and notifications.

### Key Features

* User registration and login
* JWT authentication
* Role-Based Access Control
* Employee profiles and manager mapping
* Leave application and tracking
* Manager approval/rejection
* Kafka-based notifications
* Eureka service discovery
* Spring Cloud Gateway
* Database-per-service
* REST APIs
* Unit and integration testing
* Postman API testing
* GitHub Actions CI
* Semgrep security analysis
* Jira project management

---

## Architecture

```text
                         ┌──────────────┐
                         │    CLIENT    │
                         └──────┬───────┘
                                │
                                ▼
                     ┌────────────────────┐
                     │  SPRING CLOUD      │
                     │      GATEWAY       │
                     └─────────┬──────────┘
                               │
              ┌────────────────┼────────────────┐
              ▼                ▼                ▼
       ┌────────────┐   ┌────────────┐   ┌────────────┐
       │    USER    │   │  EMPLOYEE  │   │    LEAVE   │
       │  SERVICE   │   │  SERVICE   │   │   SERVICE  │
       └─────┬──────┘   └─────┬──────┘   └─────┬──────┘
             │                │                │
             ▼                ▼                ▼
          User DB         Employee DB       Leave DB
                                              │
                                              ▼
                                           Kafka
                                              │
                                              ▼
                                      Notification Service
                                              │
                                              ▼
                                      Notification DB

                 ┌─────────────────────┐
                 │    EUREKA SERVER    │
                 │  Service Registry   │
                 └─────────────────────┘
```

The architecture uses four domain services with separate databases. Gateway, Eureka, and Kafka are infrastructure components.

---

## Microservices

### User/Auth Service

Handles:

* Registration
* Login
* Password hashing
* JWT generation
* User roles

### Employee Service

Handles:

* Employee profiles
* Departments
* Manager mapping
* Employee information

### Leave Service

Handles:

* Leave applications
* Validation
* Leave status
* Approval/rejection
* Leave history
* Kafka event publishing

### Notification Service

Handles:

* Kafka event consumption
* Notification creation
* Read/unread status
* Notification retrieval

---

## Leave Workflow

```text
Employee
   ↓
Register / Login
   ↓
JWT Authentication
   ↓
Apply Leave
   ↓
Leave Service
   ↓
PENDING
   ↓
Kafka Event
   ↓
Manager Notification
   ↓
Manager Approves / Rejects
   ↓
Kafka Event
   ↓
Employee Notification
   ↓
Final Leave Status
```

Leave states:

```text
PENDING → APPROVED
        → REJECTED
```

---

## Authentication & Authorization

The system uses **JWT-based authentication**.

```text
Login
  ↓
User Service
  ↓
JWT Generated
  ↓
Protected API Requests
```

Protected requests use:

```http
Authorization: Bearer <JWT>
```

### Roles

```text
EMPLOYEE
MANAGER
ADMIN
```

| Feature            | Employee | Manager | Admin |
| ------------------ | -------: | ------: | ----: |
| Apply Leave        |      Yes |     Yes |   Yes |
| View Own Leaves    |      Yes |     Yes |   Yes |
| View Team Requests |       No |     Yes |   Yes |
| Approve/Reject     |       No |     Yes |   Yes |
| Manage Users       |       No |      No |   Yes |

---

## REST APIs

### Authentication

```http
POST /auth/register
POST /auth/login
```

### Employees

```http
POST /employees
GET /employees/{id}
GET /employees/user/{userId}
PUT /employees/{id}
```

### Leaves

```http
POST /leaves
GET /leaves/my
GET /leaves/{id}
GET /leaves/pending
GET /leaves/history
PUT /leaves/{id}/approve
PUT /leaves/{id}/reject
```

### Notifications

```http
GET /notifications/my
GET /notifications/{id}
PUT /notifications/{id}/read
```

---

## Kafka Communication

Kafka provides asynchronous communication between Leave and Notification services.

### Events

```text
LEAVE_APPLIED
LEAVE_APPROVED
LEAVE_REJECTED
```

```text
Leave Service
      ↓
    Kafka
      ↓
Notification Service
      ↓
Notification DB
```

This keeps notification processing decoupled from the Leave Service.

---

## Service Discovery & Gateway

**Eureka** acts as the service registry.

```text
EUREKA
 ├── USER-SERVICE
 ├── EMPLOYEE-SERVICE
 ├── LEAVE-SERVICE
 ├── NOTIFICATION-SERVICE
 └── API-GATEWAY
```

Recommended Eureka port:

```text
8761
```

Gateway routes requests to services:

```text
/auth/**           → USER-SERVICE
/users/**          → USER-SERVICE
/employees/**      → EMPLOYEE-SERVICE
/leaves/**         → LEAVE-SERVICE
/notifications/**  → NOTIFICATION-SERVICE
```

---

## Database Architecture

The project follows **Database-per-Service**.

```text
User Service          → User DB
Employee Service      → Employee DB
Leave Service         → Leave DB
Notification Service  → Notification DB
```

Recommended databases:

```text
leave_management_user_db
leave_management_employee_db
leave_management_leave_db
leave_management_notification_db
```

MySQL is the recommended database choice for this implementation.

---

## Technology Stack

| Category           | Technology                       |
| ------------------ | -------------------------------- |
| Backend            | Java, Spring Boot                |
| APIs               | REST                             |
| Security           | Spring Security, JWT             |
| Gateway            | Spring Cloud Gateway             |
| Discovery          | Spring Cloud Eureka              |
| Database           | MySQL                            |
| Messaging          | Apache Kafka                     |
| Frontend           | React                            |
| Build              | Maven                            |
| Testing            | JUnit, Spring Boot Test, Postman |
| CI/CD              | GitHub Actions                   |
| Security Scan      | Semgrep                          |
| Project Management | Jira                             |
| Architecture       | Lucidchart AI                    |
| AI Development     | ChatGPT/Gemini, GitHub Copilot   |

The project document identifies some of these as recommended team choices rather than fixed requirements.

---

## Project Structure

```text
leave-management-system/
│
├── eureka-server/
├── api-gateway/
├── user-service/
├── employee-service/
├── leave-service/
├── notification-service/
├── frontend/
├── docs/
├── postman/
├── jira/
├── .github/
└── README.md
```

---

## Recommended Ports

```text
Eureka Server          8761
API Gateway            8080
User Service           8081
Employee Service       8082
Leave Service          8083
Notification Service   8084
Frontend               3000
```

---

## Setup

### Prerequisites

```text
Java
Maven
MySQL
Apache Kafka
Git
Node.js
Postman
```

### Clone

```bash
git clone <repository-url>
cd leave-management-system
```

### Create Databases

```text
leave_management_user_db
leave_management_employee_db
leave_management_leave_db
leave_management_notification_db
```

### Start Services

Recommended order:

```text
1. MySQL
2. Kafka
3. Eureka
4. User Service
5. Employee Service
6. Leave Service
7. Notification Service
8. API Gateway
9. Frontend
```

Verify registered services through the Eureka dashboard before testing APIs.

---

## Testing

### Unit Testing

JUnit tests cover:

* Registration and login
* Employee operations
* Leave validation
* Leave approval/rejection
* Notification operations

### Integration Testing

```text
HTTP Request
     ↓
Controller
     ↓
Service
     ↓
Repository
     ↓
Database
```

### Postman

The main testing flow is:

```text
Register
→ Login
→ Get JWT
→ Apply Leave
→ Verify PENDING
→ Manager Login
→ Approve/Reject
→ Verify Status
→ Verify Notification
```

Negative tests include:

```text
Invalid JWT
Missing JWT
Wrong role
Invalid dates
Missing fields
Unknown leave
Duplicate username
Unauthorized approval
```

---

## GitHub Actions

The CI pipeline performs:

```text
Git Push
   ↓
GitHub Actions
   ↓
Build
   ↓
Unit Tests
   ↓
Integration Tests
   ↓
Security Check
   ↓
Pass / Fail
```

---

## Security

Semgrep is used to identify security issues such as:

* Hardcoded secrets
* Unsafe coding patterns
* Weak security configurations
* Insecure credential handling

Never commit:

```text
DB passwords
JWT secrets
API keys
Kafka credentials
```

Use environment variables instead.

---

## Jira & AI Tools

### Jira

Used for:

* User stories
* Sprint planning
* Task assignment
* Progress tracking

### AI Tools

```text
ChatGPT / Gemini      → Requirements & documentation
Jira                  → Agile planning
Lucidchart AI         → Architecture
GitHub Copilot        → Development & tests
Postman Agent Mode    → API testing
Semgrep               → Security analysis
GitHub Actions        → CI
```

These tools correspond to the required project workflow defined in the project specification.

---

## Demo Flow

```text
1. Show Architecture
2. Show Eureka
3. Register Employee
4. Login and obtain JWT
5. Apply Leave
6. Show PENDING status
7. Show Kafka event
8. Show Manager Notification
9. Login as Manager
10. Approve/Reject Leave
11. Show updated status
12. Show Employee Notification
13. Demonstrate unauthorized access
14. Show Postman tests
15. Show JUnit tests
16. Show GitHub Actions
17. Show Semgrep
18. Show Jira Board
```

---

## Future Enhancements

* Leave balance management
* Email notifications
* SMS notifications
* Attendance integration
* Payroll integration
* Docker deployment
* Kubernetes
* Redis
* Distributed tracing
* Cloud deployment

---

## Academic Information

**Project:** Recruitment & HR — Leave Management System
**Set:** 4
**Project Number:** 12
**Subject:** SOA Programming and Microservices
