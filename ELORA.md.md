# Leave Management System

## Complete Project Master Document

### SOA Programming and Microservices \| Set-4 \| Project 12

> **Purpose of this document:** This is the single project handoff
> document for the entire team. A new teammate should be able to read
> this document and understand what the system is, why every component
> exists, how the components communicate, what each person must build,
> what must be tested, what must be shown, and what is mandatory versus
> a team choice.

------------------------------------------------------------------------

# 1. Source of Truth and Requirement Boundary

The official project list identifies **Set-4, Project 12** as:

> **Recruitment & HR: Leave Management System**

The project requirements explicitly ask students to demonstrate:

-   Service contracts and endpoints
-   REST APIs
-   At least three microservices
-   JWT authentication and role-based authorization
-   CORS configuration
-   Eureka service registration
-   Spring Cloud Gateway routing
-   Database per service
-   Unit and integration tests
-   Postman API testing
-   GitHub Actions
-   Jira user stories and sprint board
-   Architecture diagram created using an AI diagram tool

The project list also requires students to use a minimum of five
AI/tools across defined project stages:

  Project stage                      Required tool
  ---------------------------------- ----------------------------
  Requirement understanding          ChatGPT or Gemini Notebook
  User stories and sprint planning   Jira
  SOA and microservices diagram      Lucidchart AI
  Spring Boot coding                 GitHub Copilot
  REST API testing                   Postman Agent Mode
  Security analysis                  Semgrep
  Unit-test generation               GitHub Copilot
  CI/CD                              GitHub Actions

The project list states that every project should contain approximately
four services and provides a common architecture centered on a client,
API Gateway, services, separate databases, Kafka and Notification
Service, with Eureka for service discovery.

**Important boundary:** The source file does **not** mandate an exact
frontend framework, database engine, Java version, IDE, Maven/Gradle
choice, service port numbers, JWT library, Kafka topic names, cloud
provider, or notification delivery mechanism. Those are team decisions
unless your instructor separately specifies them.

The SOA workbook supplied for the course also covers JWT, Eureka, API
Gateway, integration testing and end-to-end microservice validation.
Therefore, this project should be implemented in a way that makes those
concepts visible during the final demonstration.

------------------------------------------------------------------------

# 2. Project Summary

## 2.1 Project Name

**Leave Management System**

## 2.2 Domain

Recruitment & HR

## 2.3 Core Problem

Organizations need a structured way for employees to request leave and
for managers to approve or reject those requests.

A basic monolithic implementation could put registration, employee
information, leave processing and notifications into one application.
That would not demonstrate the required SOA/microservices concepts
strongly enough.

This project instead separates the major business capabilities into
independent services.

## 2.4 Proposed Solution

The system will provide:

1.  User registration and login
2.  JWT-based authentication
3.  Role-based access for employees, managers and administrators
4.  Employee profile and manager mapping
5.  Leave application
6.  Manager approval/rejection
7.  Leave status tracking
8.  Kafka-based event publishing
9.  Notification processing
10. Separate database ownership for each service
11. API Gateway as the client entry point
12. Eureka service discovery
13. REST APIs
14. Unit and integration testing
15. Postman testing
16. CI through GitHub Actions
17. Security analysis using Semgrep
18. Agile tracking through Jira
19. AI-assisted development and architecture documentation

------------------------------------------------------------------------

# 3. Project Goals

The project has two goals.

## 3.1 Business Goal

Provide a simple HR workflow:

``` text
Register
   ↓
Login
   ↓
View Employee Profile
   ↓
Apply Leave
   ↓
Manager Receives Request
   ↓
Approve / Reject
   ↓
Leave Status Updated
   ↓
Employee Receives Notification
```

## 3.2 SOA/Microservices Learning Goal

Demonstrate:

``` text
Client
   ↓
Spring Cloud Gateway
   ↓
Eureka-discovered services
   ↓
Independent databases
   ↓
REST communication
   ↓
Kafka event communication
   ↓
Notification Service
```

------------------------------------------------------------------------

# 4. Scope

## 4.1 In Scope

-   Employee registration
-   Login
-   JWT generation and validation
-   Employee role
-   Manager role
-   Admin role
-   Employee profile
-   Manager relationship
-   Leave application
-   Leave validation
-   Pending leave requests
-   Approval
-   Rejection with reason
-   Leave history
-   Kafka leave events
-   Notification creation
-   Notification retrieval/read status
-   API Gateway
-   Eureka
-   CORS
-   Database-per-service
-   JUnit tests
-   Integration tests
-   Postman testing
-   GitHub Actions
-   Semgrep
-   Jira
-   Lucidchart AI
-   GitHub Copilot
-   ChatGPT/Gemini

## 4.2 Out of Scope Unless Instructor Requests It

Do not add unnecessary complexity such as:

-   Payroll
-   Attendance
-   Biometric integration
-   SMS
-   WhatsApp
-   External HR software
-   Complex leave accounting
-   Cloud deployment
-   Kubernetes
-   Docker
-   Redis
-   Elasticsearch
-   Spring Config Server
-   Distributed tracing
-   Email provider integration

These can be future enhancements. They are not required by the supplied
project specification.

------------------------------------------------------------------------

# 5. Architecture

## 5.1 High-Level Architecture

``` text
                         +----------------------+
                         |       CLIENT         |
                         |   Web Frontend/UI    |
                         +----------+-----------+
                                    |
                                    | HTTP/REST
                                    v
                         +----------------------+
                         | SPRING CLOUD GATEWAY  |
                         | Routing               |
                         | JWT Security           |
                         | CORS                   |
                         +----------+-----------+
                                    |
                    +---------------+----------------+
                    |               |                |
                    v               v                v
             +-----------+   +-----------+    +-----------+
             |   USER    |   | EMPLOYEE  |    |   LEAVE   |
             |  SERVICE  |   |  SERVICE  |    |  SERVICE  |
             +-----+-----+   +-----+-----+    +-----+-----+
                   |               |                 |
                   v               v                 v
             +----------+    +----------+       +----------+
             |  User DB |    |Employee DB|      | Leave DB |
             +----------+    +----------+       +----------+
                                                     |
                                                     | Event
                                                     v
                                             +---------------+
                                             | Apache Kafka  |
                                             +-------+-------+
                                                     |
                                                     | Consume
                                                     v
                                           +-------------------+
                                           | NOTIFICATION      |
                                           | SERVICE           |
                                           +---------+---------+
                                                     |
                                                     v
                                           +-------------------+
                                           | Notification DB   |
                                           +-------------------+

                    +-------------------------------+
                    |      EUREKA SERVER            |
                    |      Service Registry         |
                    +-------------------------------+
```

## 5.2 Component Roles

  Component              Role
  ---------------------- ----------------------------------------------
  Client                 User interface
  API Gateway            Single backend entry point and routing layer
  Eureka Server          Service registry/discovery
  User/Auth Service      Registration, login, users, roles, JWT
  Employee Service       Employee profiles and manager relationships
  Leave Service          Leave business logic and leave state
  Kafka                  Asynchronous event transport
  Notification Service   Consumes events and creates notifications
  User DB                Owned by User Service
  Employee DB            Owned by Employee Service
  Leave DB               Owned by Leave Service
  Notification DB        Owned by Notification Service

------------------------------------------------------------------------

# 6. Why Four Microservices?

The project requires at least three and recommends approximately four.

The recommended four are:

1.  **User/Auth Service**
2.  **Employee Service**
3.  **Leave Service**
4.  **Notification Service**

This is a better split than creating many tiny services simply to
increase the service count.

## 6.1 User/Auth Service

Owns:

-   User account
-   Credentials
-   Role
-   Registration
-   Login
-   JWT generation
-   User lookup

## 6.2 Employee Service

Owns:

-   Employee profile
-   Department
-   Manager mapping
-   Employee-related information

## 6.3 Leave Service

Owns:

-   Leave request
-   Leave type
-   Dates
-   Reason
-   Status
-   Approval/rejection
-   Leave history
-   Kafka event production

## 6.4 Notification Service

Owns:

-   Notifications
-   Notification read/unread state
-   Kafka event consumption

------------------------------------------------------------------------

# 7. Infrastructure Components Are Not Domain Services

Do not count Gateway, Eureka and Kafka as the four business services.

The architecture contains:

``` text
4 domain/application services
+
1 Gateway
+
1 Eureka Server
+
Kafka infrastructure
+
4 databases
+
1 client
```

This distinction is important during viva.

------------------------------------------------------------------------

# 8. Service Ownership Principle

Every service owns its own data.

``` text
User Service
      |
      +---- User DB

Employee Service
      |
      +---- Employee DB

Leave Service
      |
      +---- Leave DB

Notification Service
      |
      +---- Notification DB
```

Services must not directly query another service's database.

Bad:

``` text
Leave Service
     |
     +---- SELECT * FROM employee_db.employee
```

Correct:

``` text
Leave Service
     |
     +---- Employee Service REST API
```

or use data already supplied through the authenticated request/event
where appropriate.

------------------------------------------------------------------------

# 9. Database Choice

The supplied project specification does not mandate a database engine.

Recommended team choice:

``` text
MySQL
```

Alternative:

``` text
PostgreSQL
```

Choose one and use it consistently.

The important requirement is:

> **Database per service**

Not:

> "All services must use MySQL."

The engine is a team decision.

------------------------------------------------------------------------

# 10. Recommended Database Layout

Use separate database names:

``` text
leave_management_user_db
leave_management_employee_db
leave_management_leave_db
leave_management_notification_db
```

These may run on the same local database server during development while
remaining logically separate.

For a stronger demonstration, show four schemas/databases.

------------------------------------------------------------------------

# 11. User Service

## 11.1 Responsibility

The User Service handles identity.

It should answer:

-   Who is this user?
-   Can this user log in?
-   What role does this user have?
-   Is the account active?
-   What JWT should be issued?

## 11.2 User Entity

Recommended:

``` text
User
--------------------------------
id
username
passwordHash
role
status
createdAt
updatedAt
```

Never store plain-text passwords.

## 11.3 Roles

Recommended:

``` text
EMPLOYEE
MANAGER
ADMIN
```

## 11.4 User APIs

### Register

``` http
POST /auth/register
```

Request:

``` json
{
  "username": "employee01",
  "password": "Password@123",
  "role": "EMPLOYEE"
}
```

Response:

``` json
{
  "message": "User registered successfully"
}
```

### Login

``` http
POST /auth/login
```

Request:

``` json
{
  "username": "employee01",
  "password": "Password@123"
}
```

Response:

``` json
{
  "token": "<JWT>",
  "userId": 101,
  "role": "EMPLOYEE"
}
```

### Get User

``` http
GET /users/{id}
```

------------------------------------------------------------------------

# 12. Password Security

The password must not be stored directly.

Use a secure password-hashing mechanism supported by the chosen Spring
Security implementation.

Database example:

``` text
username: employee01
passwordHash: <hashed value>
```

Never:

``` text
password: Password@123
```

This is also something Semgrep/security review should be used to catch
conceptually.

------------------------------------------------------------------------

# 13. JWT Authentication

## 13.1 Login Flow

``` text
Client
  |
  | username + password
  v
Gateway
  |
  v
User Service
  |
  | validate credentials
  v
JWT generated
  |
  v
Client
```

## 13.2 Subsequent Request

``` http
Authorization: Bearer <JWT>
```

## 13.3 Recommended JWT Claims

Example:

``` json
{
  "sub": "employee01",
  "userId": 101,
  "role": "EMPLOYEE"
}
```

The exact claims are a team implementation choice.

## 13.4 JWT Structure

A JWT consists conceptually of:

``` text
Header.Payload.Signature
```

Be ready to explain all three.

------------------------------------------------------------------------

# 14. Authentication vs Authorization

## Authentication

Answers:

> Who are you?

Example:

``` text
Login → JWT
```

## Authorization

Answers:

> What are you allowed to do?

Example:

``` text
EMPLOYEE → apply leave
MANAGER → approve leave
ADMIN → manage users
```

This distinction is a likely viva question.

------------------------------------------------------------------------

# 15. Role-Based Authorization

Recommended permission matrix:

  Feature                Employee   Manager        Admin
  -------------------- ---------- --------- ------------
  Register                    Yes       Yes   Controlled
  Login                       Yes       Yes          Yes
  View own profile            Yes       Yes          Yes
  Apply leave                 Yes       Yes          Yes
  View own leaves             Yes       Yes          Yes
  View team requests           No       Yes          Yes
  Approve leave                No       Yes          Yes
  Reject leave                 No       Yes          Yes
  Manage employees             No   Limited          Yes
  Manage users                 No        No          Yes

Do not allow a public registration request to freely create an ADMIN
account in a real system. For the demo, seed the admin account or
restrict admin creation.

------------------------------------------------------------------------

# 16. Employee Service

## 16.1 Responsibility

Own employee-specific information.

## 16.2 Employee Entity

``` text
Employee
--------------------------------
id
userId
name
email
department
managerId
joiningDate
status
createdAt
updatedAt
```

## 16.3 Important Relationship

Example:

``` text
Employee 101
   |
   +---- managerId = 205

Manager 205
   |
   +---- role = MANAGER
```

The Employee DB may store `managerId` as an ID.

Do not create a cross-database foreign key.

## 16.4 APIs

``` http
POST /employees
GET /employees/{id}
GET /employees/user/{userId}
PUT /employees/{id}
GET /employees/manager/{managerId}/employees
GET /employees/{id}/manager
```

------------------------------------------------------------------------

# 17. Leave Service

## 17.1 Responsibility

This is the central business service.

It owns:

-   Leave application
-   Validation
-   Status
-   Approval
-   Rejection
-   Leave history
-   Kafka event production

## 17.2 Leave Entity

``` text
LeaveRequest
--------------------------------
id
employeeId
managerId
leaveType
startDate
endDate
reason
status
appliedAt
decisionAt
decisionReason
```

## 17.3 Leave Types

Recommended:

``` text
CASUAL
SICK
EARNED
OTHER
```

These are project design choices, not source-document requirements.

## 17.4 Leave Status

``` text
PENDING
APPROVED
REJECTED
CANCELLED
```

`CANCELLED` is optional if time is limited.

------------------------------------------------------------------------

# 18. Leave Business Rules

At minimum:

1.  Start date cannot be after end date.
2.  Employee ID must be valid.
3.  Manager must be identified.
4.  Leave must start/end on valid dates.
5.  Only PENDING leave can be approved/rejected.
6.  Only the assigned manager or authorized admin can decide.
7.  Employee cannot approve their own request.
8.  Employee cannot access another employee's private leave history.
9.  Rejection should record a reason.
10. Approval/rejection must update the leave status.

Optional rules:

-   Prevent overlapping leave
-   Leave balance
-   Maximum leave duration
-   Weekend handling

Do not implement optional rules unless your team has enough time.

------------------------------------------------------------------------

# 19. Leave APIs

## Apply Leave

``` http
POST /leaves
```

Example:

``` json
{
  "leaveType": "CASUAL",
  "startDate": "2026-09-01",
  "endDate": "2026-09-03",
  "reason": "Personal work"
}
```

The employee identity should preferably come from the authenticated JWT
rather than trusting a client-supplied employeeId.

## My Leaves

``` http
GET /leaves/my
```

## Get Leave

``` http
GET /leaves/{leaveId}
```

## Manager Pending Requests

``` http
GET /leaves/pending
```

## Approve

``` http
PUT /leaves/{leaveId}/approve
```

## Reject

``` http
PUT /leaves/{leaveId}/reject
```

Example:

``` json
{
  "reason": "Insufficient team coverage"
}
```

## History

``` http
GET /leaves/history
```

Exact endpoint naming is a team API-contract decision. Keep naming
consistent across frontend, Postman and documentation.

------------------------------------------------------------------------

# 20. Notification Service

## 20.1 Responsibility

The Notification Service should not own leave business logic.

It consumes events.

Example:

``` text
Leave Service
     |
     | LEAVE_APPLIED
     v
Kafka
     |
     v
Notification Service
```

## 20.2 Notification Entity

``` text
Notification
--------------------------------
id
userId
eventType
message
isRead
createdAt
```

## 20.3 APIs

``` http
GET /notifications/my
GET /notifications/{id}
PUT /notifications/{id}/read
```

In-app notifications are sufficient for the required project flow.

Email/SMS is optional and should not be represented as mandatory.

------------------------------------------------------------------------

# 21. Apache Kafka

Kafka is used for asynchronous event communication.

## 21.1 Why Kafka?

The Leave Service should not need to wait for Notification Service
processing.

Instead:

``` text
Leave Service
     |
     | publish event
     v
Kafka
     |
     | consume later
     v
Notification Service
```

This decouples the services.

## 21.2 Recommended Topic

Team choice:

``` text
leave-events
```

## 21.3 Event Types

Recommended:

``` text
LEAVE_APPLIED
LEAVE_APPROVED
LEAVE_REJECTED
```

## 21.4 Event Example

``` json
{
  "eventType": "LEAVE_APPLIED",
  "leaveId": 501,
  "employeeId": 101,
  "managerId": 205,
  "status": "PENDING",
  "timestamp": "2026-09-01T10:30:00"
}
```

For approval:

``` json
{
  "eventType": "LEAVE_APPROVED",
  "leaveId": 501,
  "employeeId": 101,
  "managerId": 205,
  "status": "APPROVED",
  "timestamp": "2026-09-01T12:00:00"
}
```

For rejection:

``` json
{
  "eventType": "LEAVE_REJECTED",
  "leaveId": 501,
  "employeeId": 101,
  "managerId": 205,
  "status": "REJECTED",
  "reason": "Insufficient team coverage",
  "timestamp": "2026-09-01T12:00:00"
}
```

------------------------------------------------------------------------

# 22. Kafka Producer and Consumer

## Producer

Leave Service:

``` text
LeaveController
      ↓
LeaveService
      ↓
LeaveRepository
      ↓
KafkaProducer
```

## Consumer

Notification Service:

``` text
Kafka
  ↓
KafkaConsumer
  ↓
NotificationService
  ↓
NotificationRepository
  ↓
Notification DB
```

------------------------------------------------------------------------

# 23. Complete Leave Workflow

This is the most important flow in the entire project.

## Step 1: Registration

``` text
Employee
   ↓
Client
   ↓
Gateway
   ↓
User Service
   ↓
User DB
```

Account created.

------------------------------------------------------------------------

## Step 2: Login

``` text
Employee
   ↓
Client
   ↓
Gateway
   ↓
User Service
   ↓
JWT generated
   ↓
Client
```

Client stores the JWT using the chosen frontend strategy.

------------------------------------------------------------------------

## Step 3: Employee Profile

``` text
Client
   ↓
Gateway
   ↓
Employee Service
   ↓
Employee DB
```

Employee sees:

``` text
Employee ID
Name
Department
Manager
```

------------------------------------------------------------------------

## Step 4: Apply Leave

Employee submits:

``` text
Leave Type
Start Date
End Date
Reason
```

Request:

``` http
POST /leaves
Authorization: Bearer <JWT>
```

Flow:

``` text
Client
  ↓
Gateway
  ↓
JWT validation
  ↓
Leave Service
  ↓
Validate request
  ↓
Find/verify manager
  ↓
Create PENDING leave
  ↓
Leave DB
```

------------------------------------------------------------------------

# 24. Kafka Event After Leave Application

After successful persistence:

``` text
Leave Service
     |
     | LEAVE_APPLIED
     v
Kafka
     |
     v
Notification Service
     |
     v
Notification DB
```

Manager receives:

``` text
New leave request from Employee 101.
```

------------------------------------------------------------------------

# 25. Manager Workflow

Manager logs in.

JWT contains:

``` text
role = MANAGER
```

Manager requests:

``` http
GET /leaves/pending
```

The Leave Service returns only requests assigned to that manager.

Manager chooses:

``` text
APPROVE
```

or:

``` text
REJECT
```

------------------------------------------------------------------------

# 26. Approval Flow

``` text
Manager
   ↓
PUT /leaves/{id}/approve
   ↓
Gateway
   ↓
JWT + MANAGER authorization
   ↓
Leave Service
   ↓
Verify manager owns request
   ↓
status = APPROVED
   ↓
Leave DB
   ↓
LEAVE_APPROVED
   ↓
Kafka
   ↓
Notification Service
   ↓
Notification DB
   ↓
Employee sees notification
```

------------------------------------------------------------------------

# 27. Rejection Flow

``` text
Manager
   ↓
PUT /leaves/{id}/reject
   ↓
Gateway
   ↓
JWT + MANAGER authorization
   ↓
Leave Service
   ↓
Verify request
   ↓
status = REJECTED
   ↓
save reason
   ↓
Leave DB
   ↓
LEAVE_REJECTED
   ↓
Kafka
   ↓
Notification Service
   ↓
Employee notification
```

------------------------------------------------------------------------

# 28. Final State

The employee can see:

``` text
Leave ID: 501
Type: CASUAL
Dates: 01-Sep to 03-Sep
Status: APPROVED
```

or:

``` text
Status: REJECTED
Reason: Insufficient team coverage
```

------------------------------------------------------------------------

# 29. Eureka Service Discovery

Eureka is the service registry.

Instead of depending entirely on hardcoded service locations:

``` text
localhost:8081
localhost:8082
localhost:8083
```

services register themselves.

Example:

``` text
EUREKA SERVER
     |
     +---- USER-SERVICE
     +---- EMPLOYEE-SERVICE
     +---- LEAVE-SERVICE
     +---- NOTIFICATION-SERVICE
     +---- API-GATEWAY
```

## Recommended Local Port

``` text
Eureka = 8761
```

The supplied course material also uses 8761 in its Eureka example. This
is a convenient choice, not a requirement of the Leave Management
project.

------------------------------------------------------------------------

# 30. Eureka Registration

Each service needs Eureka client configuration.

Conceptually:

``` properties
eureka.client.service-url.defaultZone=http://localhost:8761/eureka/
eureka.client.register-with-eureka=true
eureka.client.fetch-registry=true
```

Eureka Server itself should not register with itself.

Conceptually:

``` properties
server.port=8761
eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false
```

------------------------------------------------------------------------

# 31. Spring Cloud Gateway

The client should not directly call every service.

Correct:

``` text
Client
  ↓
Gateway
  ↓
Required Service
```

Not:

``` text
Client → User Service
Client → Employee Service
Client → Leave Service
Client → Notification Service
```

The supplied course material specifically teaches Gateway as the single
client entry point and uses Eureka-backed load-balanced routing.

------------------------------------------------------------------------

# 32. Gateway Route Design

Recommended:

``` text
/auth/**            → USER-SERVICE
/users/**           → USER-SERVICE
/employees/**       → EMPLOYEE-SERVICE
/leaves/**          → LEAVE-SERVICE
/notifications/**   → NOTIFICATION-SERVICE
```

Conceptual route:

``` text
Path=/leaves/**
URI=lb://LEAVE-SERVICE
```

`lb://` represents service discovery/load-balanced routing through the
registered service.

------------------------------------------------------------------------

# 33. CORS

CORS is required because the frontend and backend may have different
origins.

Example:

``` text
Frontend
http://localhost:3000

Gateway
http://localhost:8080
```

Browser:

``` text
localhost:3000
       |
       | cross-origin request
       v
localhost:8080
```

Gateway should allow the actual frontend origin.

Do not casually configure unrestricted wildcard access with credentials.

------------------------------------------------------------------------

# 34. Security Placement

Recommended conceptual flow:

``` text
Client
  ↓
Gateway
  ↓
JWT validation
  ↓
Role/access check
  ↓
Microservice
```

The downstream service should still enforce business authorization where
necessary.

For example:

> A manager may approve a leave only if the manager is actually
> responsible for that employee.

A role check alone is not enough.

------------------------------------------------------------------------

# 35. Service-to-Service Communication

Use REST when a service needs an immediate response.

Example:

``` text
Leave Service
     |
     | REST
     v
Employee Service
     |
     v
Manager information
```

Use Kafka when publishing business events.

Example:

``` text
Leave Service
     |
     | event
     v
Kafka
     |
     v
Notification Service
```

This distinction should be explained in the viva.

------------------------------------------------------------------------

# 36. API Contract Standard

Every API should define:

-   HTTP method
-   URL
-   Authentication requirement
-   Role requirement
-   Request body
-   Response body
-   Success status
-   Failure statuses
-   Validation rules

Example:

``` text
API: POST /leaves

Authentication: Required
Role: EMPLOYEE/MANAGER
Request: LeaveRequest
Success: 201 Created
Invalid data: 400 Bad Request
Unauthorized: 401 Unauthorized
Forbidden: 403 Forbidden
```

Use consistent JSON responses.

------------------------------------------------------------------------

# 37. Recommended HTTP Status Codes

``` text
200 OK
201 Created
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
409 Conflict
500 Internal Server Error
```

Do not return `200 OK` for every error.

------------------------------------------------------------------------

# 38. Error Response Format

Recommended:

``` json
{
  "timestamp": "2026-09-01T12:00:00",
  "status": 403,
  "error": "Forbidden",
  "message": "Manager role required",
  "path": "/leaves/501/approve"
}
```

The exact format is a team choice. Consistency matters more than the
exact field names.

------------------------------------------------------------------------

# 39. Recommended Repository Structure

Use one GitHub repository for the whole project unless your instructor
specifically asks for multiple repositories.

``` text
leave-management-system/
│
├── README.md
│
├── eureka-server/
│   ├── pom.xml
│   └── src/
│
├── api-gateway/
│   ├── pom.xml
│   └── src/
│
├── user-service/
│   ├── pom.xml
│   └── src/
│       ├── main/
│       │   ├── java/
│       │   │   └── ...
│       │   └── resources/
│       └── test/
│
├── employee-service/
│   ├── pom.xml
│   └── src/
│
├── leave-service/
│   ├── pom.xml
│   └── src/
│
├── notification-service/
│   ├── pom.xml
│   └── src/
│
├── frontend/
│
├── docs/
│   ├── architecture/
│   ├── api/
│   ├── screenshots/
│   ├── testing/
│   └── ai-evidence/
│
├── postman/
│   └── leave-management.postman_collection.json
│
├── .github/
│   └── workflows/
│       └── ci.yml
│
└── jira/
    └── user-stories.md
```

------------------------------------------------------------------------

# 40. Internal Spring Boot Service Structure

For a typical service:

``` text
src/
├── main/
│   ├── java/
│   │   └── com/example/leaveservice/
│   │       ├── controller/
│   │       ├── service/
│   │       ├── repository/
│   │       ├── entity/
│   │       ├── dto/
│   │       ├── exception/
│   │       ├── security/
│   │       ├── config/
│   │       └── LeaveServiceApplication.java
│   │
│   └── resources/
│       └── application.properties
│
└── test/
    └── java/
```

------------------------------------------------------------------------

# 41. Class Responsibility

## Controller

Receives HTTP requests.

``` text
LeaveController
```

## Service

Contains business logic.

``` text
LeaveService
```

## Repository

Database access.

``` text
LeaveRepository
```

## Entity

Database model.

``` text
LeaveRequest
```

## DTO

API request/response model.

``` text
LeaveRequestDTO
LeaveResponseDTO
```

## Exception

Business/API error handling.

``` text
LeaveNotFoundException
InvalidLeaveException
UnauthorizedLeaveActionException
```

## Config

Infrastructure configuration.

``` text
KafkaConfig
SecurityConfig
CorsConfig
```

------------------------------------------------------------------------

# 42. DTO Principle

Do not expose JPA entities directly as API contracts if you can avoid
it.

Prefer:

``` text
HTTP Request
   ↓
Request DTO
   ↓
Service
   ↓
Entity
   ↓
Database
```

and:

``` text
Database
   ↓
Entity
   ↓
Response DTO
   ↓
HTTP Response
```

This makes the API contract independent of the database structure.

------------------------------------------------------------------------

# 43. Configuration Management

Each service should have its own:

``` text
application.properties
```

Example:

``` properties
spring.application.name=leave-service
server.port=8083
```

Exact ports are a team choice.

Suggested local arrangement:

``` text
Eureka Server          8761
API Gateway            8080
User Service           8081
Employee Service       8082
Leave Service          8083
Notification Service   8084
Frontend               3000
```

These are recommended local-development ports, not project-file
requirements.

------------------------------------------------------------------------

# 44. Frontend Choice

The supplied project file does not mandate a frontend framework.

Choose one that your team already knows.

Reasonable options:

``` text
React
Angular
Vue
Plain HTML/CSS/JavaScript
```

For a college project, React is a reasonable choice if someone on the
team knows it.

Do not spend half the project learning a frontend framework if the
backend requirements are the actual evaluation focus.

------------------------------------------------------------------------

# 45. Frontend Screens

Minimum:

``` text
1. Login
2. Registration
3. Employee Dashboard
4. Employee Profile
5. Apply Leave
6. My Leave History
7. Manager Dashboard
8. Pending Requests
9. Approval/Rejection
10. Notifications
```

## Employee Dashboard

``` text
Welcome, Employee

Pending Leaves
Approved Leaves
Rejected Leaves

[Apply Leave]
[My Leaves]
[Notifications]
[Profile]
```

## Manager Dashboard

``` text
Welcome, Manager

Pending Leave Requests

Employee | Type | Start | End | Reason | Action

[Approve] [Reject]
```

------------------------------------------------------------------------

# 46. Frontend Authentication Flow

``` text
Login
  ↓
POST /auth/login
  ↓
Receive JWT
  ↓
Store token
  ↓
Attach token to protected requests
```

Every protected request:

``` http
Authorization: Bearer <JWT>
```

The frontend should hide or disable actions that the role cannot
perform, but backend authorization remains the real security boundary.

------------------------------------------------------------------------

# 47. Recommended Complete API Inventory

## Authentication

``` text
POST /auth/register
POST /auth/login
```

## Users

``` text
GET /users/{id}
```

## Employees

``` text
POST /employees
GET /employees/{id}
GET /employees/user/{userId}
PUT /employees/{id}
GET /employees/{id}/manager
GET /employees/manager/{managerId}/employees
```

## Leaves

``` text
POST /leaves
GET /leaves/my
GET /leaves/{id}
GET /leaves/pending
GET /leaves/history
PUT /leaves/{id}/approve
PUT /leaves/{id}/reject
```

## Notifications

``` text
GET /notifications/my
GET /notifications/{id}
PUT /notifications/{id}/read
```

This is the recommended baseline, not a mandatory URL list from the
source document.

------------------------------------------------------------------------

# 48. Postman Collection Structure

``` text
Leave Management System
│
├── 01 Authentication
│   ├── Register Employee
│   ├── Register Manager
│   └── Login
│
├── 02 Employee
│   ├── Create Employee
│   ├── Get Profile
│   └── Get Manager
│
├── 03 Leave
│   ├── Apply Leave
│   ├── My Leaves
│   ├── Pending Leaves
│   ├── Approve Leave
│   ├── Reject Leave
│   └── Leave History
│
└── 04 Notification
    ├── Get Notifications
    └── Mark Notification Read
```

------------------------------------------------------------------------

# 49. Postman Test Sequence

Use this exact sequence during testing:

``` text
1. Register employee
2. Register/seed manager
3. Login employee
4. Copy JWT
5. Create employee profile
6. Apply leave
7. Verify PENDING
8. Verify Kafka event
9. Verify manager notification
10. Login manager
11. View pending request
12. Approve
13. Verify APPROVED
14. Verify Kafka event
15. Verify employee notification
```

Then repeat with rejection.

------------------------------------------------------------------------

# 50. Negative Postman Tests

At minimum test:

``` text
Wrong password
Missing JWT
Invalid JWT
Employee accessing manager API
Employee approving leave
Manager approving unrelated request
Invalid leave dates
Missing required fields
Unknown leave ID
Reject without reason
Approve already approved request
Duplicate username
```

Expected examples:

``` text
Missing token → 401
Invalid token → 401
Wrong role → 403
Unknown ID → 404
Invalid request → 400
State conflict → 409
```

------------------------------------------------------------------------

# 51. Unit Testing

Use JUnit through the Spring Boot testing ecosystem.

Test business logic independently.

Examples:

``` text
LeaveServiceTest

testApplyLeave()
testRejectInvalidDates()
testApprovePendingLeave()
testRejectPendingLeave()
testCannotApproveRejectedLeave()
testCannotApproveWithoutAuthorization()
```

User Service:

``` text
testRegistration()
testDuplicateUsername()
testValidLogin()
testInvalidPassword()
```

Employee Service:

``` text
testCreateEmployee()
testFindEmployee()
testManagerLookup()
```

Notification Service:

``` text
testCreateNotification()
testMarkAsRead()
```

------------------------------------------------------------------------

# 52. Integration Testing

Integration tests should validate that multiple application layers work
together.

Example:

``` text
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

Test:

``` text
POST /leaves
```

and verify that the leave is actually persisted.

Also test the Gateway flow:

``` text
Client
 ↓
Gateway
 ↓
Service
```

The course workbook emphasizes end-to-end microservice testing and
validation of API Gateway + Eureka + JWT behavior.

------------------------------------------------------------------------

# 53. Kafka Testing

Test:

``` text
Leave Service
   ↓
Kafka
   ↓
Notification Service
```

Verify that:

1.  Event is produced.
2.  Consumer receives event.
3.  Correct notification is created.
4.  Correct user receives it.
5.  Approval/rejection events create the correct messages.

------------------------------------------------------------------------

# 54. GitHub Actions

The project explicitly requires GitHub Actions.

Minimum CI pipeline:

``` text
Git Push
   ↓
GitHub Actions
   ↓
Checkout
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

Do not claim cloud deployment unless you actually implement deployment.

------------------------------------------------------------------------

# 55. CI Pipeline Responsibilities

The pipeline should at minimum verify:

``` text
Code compiles
Tests pass
Build succeeds
```

If practical:

``` text
Semgrep scan
```

can also run in CI.

The exact workflow YAML is a team implementation detail.

------------------------------------------------------------------------

# 56. Semgrep

Use Semgrep for security analysis.

The process:

``` text
Source Code
   ↓
Semgrep
   ↓
Security Findings
   ↓
Fix
   ↓
Run Again
```

Look for issues such as:

-   Hardcoded secrets
-   Unsafe patterns
-   Weak security configurations
-   Insecure coding patterns
-   Dangerous credential handling

Do not just install Semgrep and claim completion. Save the scan result.

------------------------------------------------------------------------

# 57. Jira

Jira is required for:

-   User stories
-   Sprint planning
-   Task assignment
-   Progress tracking
-   Sprint board

Recommended columns:

``` text
BACKLOG
TO DO
IN PROGRESS
CODE REVIEW
TESTING
DONE
```

------------------------------------------------------------------------

# 58. Jira Epic Structure

## Epic 1: Authentication

Stories:

``` text
Register User
Login User
Generate JWT
Implement Role Authorization
```

## Epic 2: Employee Management

``` text
Create Employee
View Employee
Assign Manager
```

## Epic 3: Leave Management

``` text
Apply Leave
View Leave
Approve Leave
Reject Leave
Track Leave Status
```

## Epic 4: Notification

``` text
Publish Leave Event
Consume Kafka Event
Create Notification
Read Notification
```

## Epic 5: Infrastructure

``` text
Create Eureka
Configure Gateway
Configure CORS
Configure Databases
Configure Kafka
```

## Epic 6: Testing/DevOps

``` text
JUnit
Integration Testing
Postman
Semgrep
GitHub Actions
```

------------------------------------------------------------------------

# 59. User Stories

## US-01 Registration

> As a user, I want to register an account so that I can access the
> leave management system.

Acceptance criteria:

-   Username is required.
-   Password is required.
-   Duplicate username is rejected.
-   User is persisted.
-   Password is hashed.

## US-02 Login

> As a registered user, I want to log in so that I receive a JWT.

Acceptance criteria:

-   Correct credentials succeed.
-   Wrong credentials fail.
-   JWT is returned after successful login.

## US-03 Employee Profile

> As an employee, I want to view my employee profile and manager.

## US-04 Apply Leave

> As an employee, I want to submit a leave request.

Acceptance criteria:

-   Valid dates required.
-   Reason required.
-   Initial status is PENDING.
-   Leave is stored.
-   Kafka event is produced.

## US-05 Manager Requests

> As a manager, I want to see leave requests assigned to me.

## US-06 Approve

> As a manager, I want to approve a pending leave request.

Acceptance criteria:

-   Only authorized manager can approve.
-   Status changes to APPROVED.
-   Kafka event is produced.

## US-07 Reject

> As a manager, I want to reject a pending leave request with a reason.

## US-08 Notification

> As a user, I want to receive notifications when my leave status
> changes.

## US-09 Testing

> As a developer, I want automated tests so that changes do not break
> existing functionality.

## US-10 CI

> As a developer, I want GitHub Actions to automatically build and test
> the repository.

------------------------------------------------------------------------

# 60. Sprint Plan

## Sprint 0: Project Setup

Deliverables:

``` text
GitHub repository
Jira project
Team roles
Initial architecture
Technology decisions
Database decision
Frontend decision
```

## Sprint 1: Infrastructure

Build:

``` text
Eureka
Gateway
Basic service skeletons
Database connections
CORS
```

Deliverable:

``` text
Client → Gateway → Eureka → Service
```

## Sprint 2: Authentication

Build:

``` text
User Service
Registration
Login
JWT
Roles
Gateway authentication
```

Deliverable:

``` text
Register → Login → JWT → Protected API
```

## Sprint 3: Employee + Leave

Build:

``` text
Employee Service
Employee profile
Manager mapping
Leave Service
Apply leave
Pending requests
Approval
Rejection
```

Deliverable:

``` text
Employee → Apply → Manager → Approve/Reject
```

## Sprint 4: Kafka + Notification

Build:

``` text
Kafka Producer
Kafka Topic
Kafka Consumer
Notification Service
Notification DB
```

Deliverable:

``` text
Leave Event → Kafka → Notification
```

## Sprint 5: Testing + Security

Build:

``` text
JUnit
Integration Tests
Postman Collection
Semgrep
GitHub Actions
```

## Sprint 6: Finalization

Complete:

``` text
Frontend
Documentation
Architecture diagram
Jira evidence
Testing evidence
Demo
Viva preparation
```

Adjust sprint count to your actual academic schedule.

------------------------------------------------------------------------

# 61. Team Division for Four Members

## Member 1: Authentication + Gateway

Own:

``` text
User Service
JWT
Spring Security
Roles
API Gateway
CORS
```

## Member 2: Employee Service

Own:

``` text
Employee Service
Employee DB
Manager mapping
Employee APIs
```

## Member 3: Leave Service

Own:

``` text
Leave Service
Leave DB
Leave business rules
Approval/rejection
Kafka Producer
```

## Member 4: Notification + Testing

Own:

``` text
Notification Service
Notification DB
Kafka Consumer
Notification APIs
Testing coordination
```

## Shared Work

Everyone must contribute to:

``` text
Jira
GitHub
Postman
Testing
Documentation
Demo
Viva
```

No one should disappear until the final presentation and claim "I did
the database."

------------------------------------------------------------------------

# 62. Git Branch Strategy

Recommended:

``` text
main
develop

feature/user-service
feature/employee-service
feature/leave-service
feature/notification-service
feature/gateway
feature/eureka
feature/testing
feature/frontend
```

Workflow:

``` text
Create branch
   ↓
Implement
   ↓
Commit
   ↓
Push
   ↓
Pull Request
   ↓
Review
   ↓
Merge
```

Use meaningful commit messages:

``` text
feat: add leave application API
feat: add JWT login
feat: configure Eureka client
feat: publish leave events
test: add leave approval tests
fix: validate leave dates
```

------------------------------------------------------------------------

# 63. AI Tool Usage

The project specifically requires AI/tool usage across project stages.

## ChatGPT or Gemini Notebook

Use for:

``` text
Requirement analysis
SOA understanding
Microservice boundary discussion
API planning
Debugging
Documentation
Viva preparation
```

Keep screenshots/evidence of meaningful use.

Do not blindly paste generated code.

------------------------------------------------------------------------

# 64. Lucidchart AI

Use it for the architecture diagram.

Prompt concept:

``` text
Create a microservices architecture for a Leave Management System.

Client communicates only with Spring Cloud Gateway.

Gateway routes to User/Auth Service, Employee Service,
Leave Service and Notification Service.

Use Eureka Service Registry for service discovery.

Each microservice has its own separate database.

Leave Service publishes leave events to Apache Kafka.

Notification Service consumes Kafka events and stores notifications.

Show JWT authentication and CORS at the gateway/security layer.

Show REST communication and Kafka asynchronous event flow.
```

Then manually inspect the generated diagram.

AI diagrams are not automatically correct.

------------------------------------------------------------------------

# 65. GitHub Copilot

Use Copilot for:

``` text
Spring Boot boilerplate
Controllers
Services
Repositories
DTOs
Exception handling
Kafka producer
Kafka consumer
JUnit test skeletons
```

The developer remains responsible for:

``` text
Architecture
Security
Validation
Correctness
Testing
Code review
```

------------------------------------------------------------------------

# 66. Postman Agent Mode

Use it for:

``` text
REST API testing
Request generation
Response analysis
API workflow testing
Negative cases
```

Keep the final Postman collection in the repository.

------------------------------------------------------------------------

# 67. Semgrep

Use it for:

``` text
Security scanning
Code pattern analysis
Vulnerability detection
```

Save:

``` text
Semgrep report
```

under:

``` text
docs/testing/
```

------------------------------------------------------------------------

# 68. GitHub Actions

Use it for:

``` text
Automated build
Automated tests
CI validation
```

Save workflow:

``` text
.github/workflows/ci.yml
```

------------------------------------------------------------------------

# 69. ChatGPT/Gemini Evidence

Create:

``` text
docs/ai-evidence/
```

Recommended:

``` text
requirements-analysis.png
architecture-prompt.png
copilot-example.png
postman-agent-example.png
semgrep-result.png
```

Only include evidence that reflects actual work performed.

------------------------------------------------------------------------

# 70. Documentation Structure

The repository should contain:

``` text
docs/
├── architecture/
│   ├── architecture.png
│   ├── architecture.md
│   └── sequence-diagrams.md
│
├── api/
│   ├── api-contracts.md
│   └── postman.md
│
├── database/
│   ├── schema.md
│   └── er-diagrams.md
│
├── testing/
│   ├── unit-tests.md
│   ├── integration-tests.md
│   └── semgrep-report.md
│
├── ai-evidence/
│
└── jira/
    └── sprint-evidence.md
```

------------------------------------------------------------------------

# 71. Architecture Diagram Requirements

The final Lucidchart diagram should show:

``` text
Client
   ↓
Spring Cloud Gateway
   ↓
Eureka
   ↓
User Service
Employee Service
Leave Service
Notification Service

Each service → own database

Leave Service → Kafka → Notification Service
```

Also label:

``` text
JWT
CORS
REST
Kafka
Service Discovery
Database per Service
```

------------------------------------------------------------------------

# 72. Sequence Diagram: Leave Application

``` text
Employee        Client       Gateway      Leave Service      Leave DB       Kafka       Notification
   |               |            |              |               |             |              |
   | Apply Leave   |            |              |               |             |              |
   |-------------->|            |              |               |             |              |
   |               | POST /leaves               |               |             |              |
   |               |----------->|              |               |             |              |
   |               |            | JWT check    |               |             |              |
   |               |            |------------->|               |             |              |
   |               |            |              | Save PENDING  |             |              |
   |               |            |              |-------------->|             |              |
   |               |            |              |<--------------|             |              |
   |               |            |              | Publish event |             |              |
   |               |            |              |---------------------------->|              |
   |               |            |              |               |             | Consume       |
   |               |            |              |               |             |-------------> |
   |               |            |              |               |             |              |
   |               |<-----------|<-------------|               |             |              |
```

------------------------------------------------------------------------

# 73. Sequence Diagram: Approval

``` text
Manager       Client       Gateway      Leave Service       Leave DB       Kafka       Notification
   |             |            |              |               |             |              |
   | Approve     |            |              |               |             |              |
   |------------>|            |              |               |             |              |
   |             |----------->|              |               |             |              |
   |             |            | JWT + ROLE   |               |             |              |
   |             |            |------------->|               |             |              |
   |             |            |              | verify manager|             |              |
   |             |            |              | update status |             |              |
   |             |            |              |-------------->|             |              |
   |             |            |              | publish event |             |              |
   |             |            |              |---------------------------->|              |
   |             |            |              |               |             |------------->|
   |             |            |              |               |             |              |
   |             |<-----------|<-------------|               |             |              |
```

------------------------------------------------------------------------

# 74. State Machine

``` text
              +----------+
              |  PENDING |
              +----+-----+
                   |
          +--------+--------+
          |                 |
          v                 v
    +-----------+     +-----------+
    | APPROVED  |     | REJECTED  |
    +-----------+     +-----------+
```

Optional:

``` text
PENDING → CANCELLED
```

Do not allow:

``` text
APPROVED → REJECTED
REJECTED → APPROVED
```

unless your business rules explicitly support it.

------------------------------------------------------------------------

# 75. Database Relationships

## User DB

``` text
User
 |
 +-- id
 +-- username
 +-- role
```

## Employee DB

``` text
Employee
 |
 +-- userId
 +-- managerId
```

## Leave DB

``` text
LeaveRequest
 |
 +-- employeeId
 +-- managerId
```

## Notification DB

``` text
Notification
 |
 +-- userId
```

These IDs are logical references.

Do not create cross-service database foreign keys.

------------------------------------------------------------------------

# 76. Startup Order

For local development:

``` text
1. Database servers
2. Kafka
3. Eureka Server
4. User Service
5. Employee Service
6. Leave Service
7. Notification Service
8. API Gateway
9. Frontend
```

After services register, verify Eureka.

------------------------------------------------------------------------

# 77. Startup Verification

Before testing APIs:

``` text
Open Eureka Dashboard
       ↓
Verify services are registered
```

Expected:

``` text
USER-SERVICE
EMPLOYEE-SERVICE
LEAVE-SERVICE
NOTIFICATION-SERVICE
API-GATEWAY
```

Then test Gateway routes.

------------------------------------------------------------------------

# 78. Local Development Checklist

``` text
[ ] Java installed
[ ] Maven available or Maven Wrapper configured
[ ] IDE installed
[ ] Database installed
[ ] Kafka available
[ ] Git installed
[ ] GitHub access configured
[ ] Node/frontend tooling installed if using a frontend framework
[ ] Postman available
[ ] Semgrep available
[ ] Jira project created
[ ] Lucidchart access available
[ ] GitHub Copilot available
[ ] ChatGPT/Gemini available
```

Exact Java/Node/IDE versions are team choices.

------------------------------------------------------------------------

# 79. Recommended Technology Decision

If the team wants a concrete baseline:

``` text
Backend:
Java
Spring Boot
Spring Cloud Gateway
Spring Cloud Eureka
Spring Security
JWT
Spring Data JPA
REST

Database:
MySQL

Messaging:
Apache Kafka

Frontend:
React

Build:
Maven

Testing:
JUnit
Spring Boot Test
Postman

Version Control:
Git + GitHub

CI:
GitHub Actions

Security:
Semgrep

Planning:
Jira

Architecture:
Lucidchart AI

AI Development:
ChatGPT/Gemini
GitHub Copilot
Postman Agent Mode
```

Again, only the technologies explicitly required by the project should
be described as mandatory. The database, React, Maven and exact versions
are this team's recommended choices.

------------------------------------------------------------------------

# 80. Dependency Categories

Each Spring Boot service will generally need some subset of:

``` text
Spring Web
Spring Data JPA
Database Driver
Spring Security
JWT implementation
Eureka Client
Validation
Lombok (optional)
Kafka
Testing
```

Gateway needs:

``` text
Spring Cloud Gateway
Eureka Client
Security/JWT support
```

Eureka Server needs:

``` text
Eureka Server
```

Do not copy dependencies blindly between services.

------------------------------------------------------------------------

# 81. Java Version and Spring Boot Version

The supplied project list does not specify a mandatory Java or Spring
Boot version.

The course's separate Eureka material shows examples using Java 17/21
and Spring Boot 3.5.x/4.1.0 in different examples.

Therefore:

> Select one compatible Java + Spring Boot + Spring Cloud release
> combination and use it consistently across the project.

Do not mix incompatible Spring Boot/Spring Cloud versions.

Record the final chosen versions in:

``` text
README.md
```

and:

``` text
docs/architecture/technology-stack.md
```

------------------------------------------------------------------------

# 82. Configuration Secrets

Do not commit:

``` text
Database passwords
JWT secrets
Kafka credentials
API keys
```

For local development, use environment variables or a local
configuration mechanism.

Example:

``` text
DB_USERNAME
DB_PASSWORD
JWT_SECRET
```

Do not put real secrets in GitHub.

------------------------------------------------------------------------

# 83. README Requirements

The root README should contain:

``` text
1. Project title
2. Problem statement
3. Objectives
4. Architecture
5. Services
6. Technology stack
7. Database design
8. API list
9. Kafka flow
10. Authentication
11. Authorization
12. Setup instructions
13. Run instructions
14. Testing
15. Postman
16. GitHub Actions
17. Semgrep
18. Jira
19. AI tools used
20. Team members
```

------------------------------------------------------------------------

# 84. Environment Setup Documentation

Document:

``` text
Java version
Maven version
Database setup
Kafka setup
Frontend setup
Environment variables
Ports
Startup order
```

A teammate should not need to ask:

> "Which port does Leave Service run on?"

It should be written down.

------------------------------------------------------------------------

# 85. Final Demo Order

Do not randomly click around the application.

Use a scripted sequence.

## Part 1: Architecture

Show:

``` text
Lucidchart diagram
```

Explain every component.

## Part 2: Eureka

Show:

``` text
Eureka dashboard
```

Show registered services.

## Part 3: Login

Register/login as employee.

Show JWT.

## Part 4: Employee

Show profile and manager.

## Part 5: Leave

Apply leave.

Show PENDING.

## Part 6: Kafka

Show `LEAVE_APPLIED`.

## Part 7: Notification

Show manager notification.

## Part 8: Manager

Login as manager.

Show pending request.

## Part 9: Approval

Approve.

## Part 10: Kafka

Show `LEAVE_APPROVED`.

## Part 11: Employee

Show final notification and status.

## Part 12: Security

Try employee → manager endpoint.

Show:

``` text
403 Forbidden
```

## Part 13: Testing

Show:

``` text
JUnit
Integration Tests
Postman
```

## Part 14: DevOps

Show:

``` text
GitHub Actions
Semgrep
```

## Part 15: Agile

Show:

``` text
Jira sprint board
```

------------------------------------------------------------------------

# 86. What Must Be Demonstrated

Use this as the final evaluation checklist.

## Architecture

-   [ ] Client
-   [ ] Gateway
-   [ ] Eureka
-   [ ] Four services
-   [ ] Four databases
-   [ ] Kafka
-   [ ] Notification Service

## APIs

-   [ ] Service contracts
-   [ ] REST endpoints
-   [ ] Request/response examples
-   [ ] Error responses

## Security

-   [ ] Registration
-   [ ] Login
-   [ ] JWT
-   [ ] Roles
-   [ ] Authorization
-   [ ] CORS
-   [ ] Unauthorized request
-   [ ] Forbidden request

## Business

-   [ ] Apply leave
-   [ ] Pending status
-   [ ] Manager approval
-   [ ] Manager rejection
-   [ ] Final status
-   [ ] Notification

## Distributed System

-   [ ] Eureka registration
-   [ ] Gateway routing
-   [ ] REST service communication
-   [ ] Kafka producer
-   [ ] Kafka consumer
-   [ ] Database-per-service

## Testing

-   [ ] Unit tests
-   [ ] Integration tests
-   [ ] Postman
-   [ ] Negative tests
-   [ ] Kafka event verification

## DevOps/Security

-   [ ] GitHub Actions
-   [ ] Semgrep

## Agile

-   [ ] Jira backlog
-   [ ] User stories
-   [ ] Sprint
-   [ ] Assigned tasks
-   [ ] Completed board

## AI

-   [ ] ChatGPT/Gemini
-   [ ] Lucidchart AI
-   [ ] GitHub Copilot
-   [ ] Postman Agent Mode
-   [ ] Semgrep
-   [ ] GitHub Actions

------------------------------------------------------------------------

# 87. Viva Questions

## Architecture

### Why microservices?

To separate business capabilities into independently developed and
maintained services and to demonstrate service-oriented architecture.

### Why four services?

The project asks for at least three and approximately four. Four
provides a clean split between identity, employee data, leave business
logic and notifications.

### Why Gateway?

It provides a single client entry point and centralized
routing/cross-cutting handling.

### Why Eureka?

It provides service registration and discovery.

### Why database per service?

Each service owns its data, reducing coupling and maintaining service
autonomy.

------------------------------------------------------------------------

# 88. Security Viva Questions

### What is JWT?

A signed token used to represent authenticated identity/claims between
client and services.

### What are JWT components?

Header, payload and signature.

### Authentication vs authorization?

Authentication verifies identity. Authorization determines permitted
actions.

### What is RBAC?

Role-Based Access Control. Permissions are determined by roles.

### Why can't an employee approve a leave?

The authorization rules allow approval only for authorized manager/admin
roles, followed by a business check that the manager owns the request.

### Is hiding the approve button enough?

No. The backend must reject unauthorized requests.

------------------------------------------------------------------------

# 89. Kafka Viva Questions

### Why Kafka?

For asynchronous event-driven communication.

### Why not REST for notification?

REST would create tighter runtime coupling. Kafka allows the leave
operation and notification processing to be decoupled.

### What happens when a leave is applied?

Leave Service saves it and publishes `LEAVE_APPLIED`.

### What consumes the event?

Notification Service.

### What happens after approval?

`LEAVE_APPROVED` is published and consumed by Notification Service.

------------------------------------------------------------------------

# 90. Testing Viva Questions

### Unit vs integration test?

Unit tests isolate a component. Integration tests verify interaction
between multiple application components.

### What is API testing?

Testing HTTP endpoints using requests, responses, status codes,
authentication and business rules.

### Why negative tests?

Because distributed systems and security controls must prove that
invalid/unauthorized operations fail correctly.

### Why test Gateway?

Because Gateway routing/security is part of the system architecture.

------------------------------------------------------------------------

# 91. Eureka Viva Questions

### What is service discovery?

Finding service instances dynamically through a registry.

### What is Eureka?

A service registry used for registration and discovery.

### What happens when a service starts?

It registers itself with Eureka.

### What happens when a service stops?

Its instance eventually becomes unavailable to discovery depending on
registry heartbeat/eviction behavior.

### Why not hardcode ports?

Hardcoded locations create tighter coupling and make scaling/moving
instances harder.

------------------------------------------------------------------------

# 92. Failure Scenarios to Understand

## Notification Service Down

Conceptually:

``` text
Leave Service
   ↓
Kafka
   ↓
Notification Service unavailable
```

The event can remain available for later consumption depending on Kafka
retention/consumer configuration.

## Employee Service Down

Leave operations that require employee/manager information may fail or
be restricted.

## Leave Service Down

Leave operations cannot be processed.

## Gateway Down

External clients cannot reach backend services through the normal entry
point.

## Eureka Down

Existing direct connectivity may differ from discovery behavior
depending on cached registry state, but new service discovery can be
affected.

Do not claim "the entire system always continues working" under every
failure. Distributed systems have failure modes.

------------------------------------------------------------------------

# 93. Optional Enhancements

Only implement after the mandatory requirements work.

Possible enhancements:

``` text
Leave balance
Overlapping leave detection
Admin dashboard
Audit history
Email notification
Leave cancellation
Pagination
Filtering
Search
Dashboard statistics
Docker
Centralized logging
```

These are optional.

------------------------------------------------------------------------

# 94. What Not to Do

Do not:

``` text
[ ] Build only CRUD and ignore architecture
[ ] Use one database for all services
[ ] Let frontend call every service directly
[ ] Store plain passwords
[ ] Put JWT secret in GitHub
[ ] Let employee call approval API successfully
[ ] Hardcode manager ID from frontend without validation
[ ] Claim Kafka exists without demonstrating an event
[ ] Claim Eureka exists without registering services
[ ] Claim tests exist without running them
[ ] Claim AI tools were used without evidence
[ ] Put all business logic in controllers
[ ] Return HTTP 200 for every error
[ ] Add unnecessary technologies just to make the diagram bigger
```

------------------------------------------------------------------------

# 95. Definition of Done

The project is complete only when all of these are true:

``` text
[ ] Four services implemented
[ ] Eureka running
[ ] Services registered
[ ] Gateway running
[ ] Gateway routing working
[ ] CORS working
[ ] JWT authentication working
[ ] Roles working
[ ] Role authorization working
[ ] User DB working
[ ] Employee DB working
[ ] Leave DB working
[ ] Notification DB working
[ ] Employee registration working
[ ] Employee login working
[ ] Employee profile working
[ ] Manager relationship working
[ ] Leave application working
[ ] Pending status working
[ ] Manager approval working
[ ] Manager rejection working
[ ] Kafka producer working
[ ] Kafka consumer working
[ ] Notification creation working
[ ] Final status visible
[ ] JUnit tests passing
[ ] Integration tests passing
[ ] Postman collection passing
[ ] Negative tests passing
[ ] GitHub Actions passing
[ ] Semgrep scan completed
[ ] Jira board maintained
[ ] Lucidchart AI diagram completed
[ ] AI usage evidence collected
[ ] README completed
[ ] Final demo rehearsed
[ ] Viva questions prepared
```

------------------------------------------------------------------------

# 96. One-Sentence Architecture Explanation

Every team member should memorize this:

> The Leave Management System uses a client-to-Spring Cloud Gateway
> architecture, where Eureka provides service discovery for four
> independent services---User/Auth, Employee, Leave and
> Notification---each owning its own database, while Leave Service
> publishes business events through Apache Kafka and Notification
> Service consumes those events to generate notifications.

------------------------------------------------------------------------

# 97. One-Sentence Business Workflow

> An employee registers and logs in to receive a JWT, applies for leave
> through the Gateway, the Leave Service stores it as PENDING and
> publishes an event to Kafka, the manager receives a notification and
> approves or rejects the request, and the resulting status event is
> consumed by Notification Service so the employee receives the final
> status.

------------------------------------------------------------------------

# 98. Final Team Execution Order

Do the project in this order.

``` text
PHASE 1
Understand requirements
        ↓
PHASE 2
Freeze architecture
        ↓
PHASE 3
Create Jira + GitHub
        ↓
PHASE 4
Create Eureka
        ↓
PHASE 5
Create Gateway
        ↓
PHASE 6
Create four services
        ↓
PHASE 7
Create four databases
        ↓
PHASE 8
Implement authentication
        ↓
PHASE 9
Implement authorization
        ↓
PHASE 10
Implement employee management
        ↓
PHASE 11
Implement leave workflow
        ↓
PHASE 12
Implement Kafka
        ↓
PHASE 13
Implement Notification Service
        ↓
PHASE 14
Connect frontend
        ↓
PHASE 15
Unit tests
        ↓
PHASE 16
Integration tests
        ↓
PHASE 17
Postman
        ↓
PHASE 18
Semgrep
        ↓
PHASE 19
GitHub Actions
        ↓
PHASE 20
Lucidchart architecture
        ↓
PHASE 21
Documentation
        ↓
PHASE 22
Final demo rehearsal
        ↓
PHASE 23
Viva preparation
```

------------------------------------------------------------------------

# 99. Final Project Deliverables

The final GitHub repository should contain:

``` text
Source code
Four microservices
Eureka server
API Gateway
Frontend
Database scripts/schema documentation
Kafka configuration
Tests
Postman collection
GitHub Actions workflow
Semgrep evidence/report
Jira evidence
Lucidchart architecture diagram
AI usage evidence
README
API documentation
Demo screenshots
```

------------------------------------------------------------------------

# 100. Final Evaluation Strategy

If time becomes limited, prioritize in this order:

``` text
1. Four working services
2. Gateway
3. Eureka
4. JWT + roles
5. Database per service
6. Leave workflow
7. Kafka
8. Notification Service
9. Unit/integration tests
10. Postman
11. GitHub Actions
12. Jira
13. Semgrep
14. AI evidence
15. Frontend polish
16. Optional features
```

Do not sacrifice the required architecture to build a prettier frontend.

The strongest project is not the one with the most screens. It is the
one where the evaluator can clearly see:

``` text
Client
   ↓
Gateway
   ↓
Eureka
   ↓
Independent Services
   ↓
Independent Databases
   ↓
Kafka Event
   ↓
Notification Service
   ↓
Final Business Outcome
```

That is the architecture and workflow the entire team should build and
demonstrate.
