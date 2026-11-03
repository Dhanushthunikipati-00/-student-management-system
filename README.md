# Student Management System

A RESTful backend service for managing student records, built with
**Java 17, Spring Boot, Spring Data JPA, and MySQL**.

![Java](https://img.shields.io/badge/Java-17-orange?logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.x-6DB33F?logo=springboot&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-8-4479A1?logo=mysql&logoColor=white)
![Maven](https://img.shields.io/badge/Build-Maven-C71A36?logo=apachemaven&logoColor=white)

------------------------------------------------------------------------

## Overview

This project exposes a clean, layered REST API that supports the
complete student record lifecycle: creation, retrieval, update,
deletion, search, and paginated listing. It demonstrates centralized
exception handling and separation of concerns across controller,
service, and repository layers.

## Table of Contents

1.  [Key Features](#key-features)
2.  [Tech Stack](#tech-stack)
3.  [Architecture](#architecture)
4.  [Project Structure](#project-structure)
5.  [Getting Started](#getting-started)
6.  [Configuration](#configuration)
7.  [API Reference](#api-reference)
8.  [Error Handling](#error-handling)
9.  [Testing the API](#testing-the-api)
10. [Troubleshooting](#troubleshooting)
11. [Roadmap](#roadmap)
12. [Contributing](#contributing)

------------------------------------------------------------------------

## Key Features

| Area | Capability |
|---|---|
| **CRUD** | Create, read, update, and delete student records |
| **Search** | Find students by name or by course |
| **Pagination** | Retrieve large datasets page by page |
| **Error Handling** | Consistent JSON error responses via a global exception handler |
| **Persistence** | Relational storage with Spring Data JPA and Hibernate on MySQL |

## Tech Stack

| Layer | Technology |
|---|---|
| Language | Java 17 |
| Framework | Spring Boot |
| Data Access | Spring Data JPA, Hibernate |
| Database | MySQL 8 |
| Build Tool | Maven |
| Tooling | IntelliJ IDEA, Postman |

## Architecture

The service follows a layered architecture, keeping HTTP concerns,
business logic, and data access independent of one another.

``` text
┌─────────────────────────────┐
│   Client (Postman / UI)     │
└──────────────┬──────────────┘
               │ HTTP / JSON
┌──────────────▼──────────────┐
│   Controller Layer          │  Request mapping, validation trigger
├─────────────────────────────┤
│   Service Layer             │  Business logic
├─────────────────────────────┤
│   Repository Layer          │  Spring Data JPA
└──────────────┬──────────────┘
               │ JDBC
┌──────────────▼──────────────┐
│   MySQL 8                   │
└─────────────────────────────┘

   Exception Layer: global handler converts errors into structured JSON responses
```

## Project Structure

```text
student_management
├── pom.xml
└── src
    ├── main
    │   ├── java
    │   │   └── com.example.student_management
    │   │       ├── StudentManagementApplication.java
    │   │       ├── controller    # REST endpoints
    │   │       ├── entity        # JPA entities
    │   │       ├── repository    # Data access interfaces
    │   │       ├── service       # Business logic
    │   │       └── exception     # Custom exceptions and global handler
    │   └── resources
    │       └── application.properties
    └── test
        └── java
            └── com.example.student_management
                └── StudentManagementApplicationTests.java
```

------------------------------------------------------------------------

## Getting Started

### Prerequisites

-   JDK 17+
-   Maven 3.8+
-   MySQL 8
-   Postman or cURL (optional, for testing)

### Installation

**1. Clone the repository**

``` bash
git clone https://github.com/Dhanushthunikipati-00/student_management.git
cd student_management
```

**2. Create the database**

``` sql
CREATE DATABASE student_management;
```

**3. Configure credentials**

Set your MySQL connection details as described in
[Configuration](#configuration).

**4. Build and run**

``` bash
.\mvnw.cmd clean install
.\mvnw.cmd spring-boot:run
```

Alternatively, open the project in IntelliJ IDEA and run
`StudentManagementApplication.java`.

**5. Verify**

The API is available at `http://localhost:8080`.

------------------------------------------------------------------------

## Configuration

Define the datasource in `src/main/resources/application.properties`.
The password is read from an environment variable so that the database
password is not stored in the repository.

``` properties
spring.application.name=student_management

# Datasource
spring.datasource.url=${DB_URL:jdbc:mysql://localhost:3306/student_management}
spring.datasource.username=${DB_USERNAME:root}
spring.datasource.password=${DB_PASSWORD}

# JPA / Hibernate
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

# Server
server.port=8080
```

Set the variables before starting the application:

``` bash
# Linux / macOS
export DB_USERNAME=root
export DB_PASSWORD=your_password

# Windows (PowerShell)
$env:DB_USERNAME="root"
$env:DB_PASSWORD="your_password"
```

> **Security:** Never commit real credentials. Add any local override
> files (for example `application-local.properties`) to `.gitignore`.
> For production, use `ddl-auto=validate` with a migration tool such as
> Flyway instead of `update`.

------------------------------------------------------------------------

## API Reference

**Base URL:** `http://localhost:8080/api/students` **Content type:**
`application/json`

### Student Resource

| Field | Type | Required | Description |
|---|---|---|---|
| `id` | Long | Read-only | Auto-generated unique identifier |
| `name` | String | Yes | Full name |
| `email` | String | Yes | Valid email address |
| `age` | Integer | Yes | Age in years |
| `course` | String | Yes | Enrolled course |

### Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/students` | Create a student |
| `GET` | `/api/students` | List all students |
| `GET` | `/api/students/{id}` | Retrieve a student by ID |
| `PUT` | `/api/students/{id}` | Update a student |
| `DELETE` | `/api/students/{id}` | Delete a student |
| `GET` | `/api/students/search/name?name={name}` | Search by name |
| `GET` | `/api/students/search/course?course={course}` | Search by course |
| `GET` | `/api/students/pagination?page={page}&size={size}` | Paginated listing |

### Create a Student

`POST /api/students`

``` json
{
  "name": "Amit Kumar",
  "email": "amit@gmail.com",
  "age": 22,
  "course": "Spring Boot"
}
```

**Response** `200 OK` (as returned by the current controller
implementation)

``` json
{
  "id": 1,
  "name": "Amit Kumar",
  "email": "amit@gmail.com",
  "age": 22,
  "course": "Spring Boot"
}
```

``` bash
curl -X POST http://localhost:8080/api/students \
  -H "Content-Type: application/json" \
  -d '{"name":"Amit Kumar","email":"amit@gmail.com","age":22,"course":"Spring Boot"}'
```

### List All Students

`GET /api/students`

**Response** `200 OK`: array of student objects.

``` bash
curl http://localhost:8080/api/students
```

### Get a Student by ID

`GET /api/students/{id}`

| Parameter | In | Type | Description |
|---|---|---|---|
| `id` | Path | Long | Student identifier |

**Response** `200 OK`: a single student object, or `404 Not Found` if
the ID does not exist.

``` bash
curl http://localhost:8080/api/students/1
```

### Update a Student

`PUT /api/students/{id}`

``` json
{
  "name": "Amit Kumar",
  "email": "amit.kumar@gmail.com",
  "age": 23,
  "course": "Microservices"
}
```

**Response** `200 OK`: the updated student object.

``` bash
curl -X PUT http://localhost:8080/api/students/1 \
  -H "Content-Type: application/json" \
  -d '{"name":"Amit Kumar","email":"amit.kumar@gmail.com","age":23,"course":"Microservices"}'
```

### Delete a Student

`DELETE /api/students/{id}`

**Response** `200 OK` on success, or `404 Not Found` if the student does
not exist.

``` bash
curl -X DELETE http://localhost:8080/api/students/1
```

### Search by Name

`GET /api/students/search/name?name=Amit`

| Parameter | In | Type | Description |
|---|---|---|---|
| `name` | Query | String | Full or partial student name |

``` bash
curl "http://localhost:8080/api/students/search/name?name=Amit"
```

### Search by Course

`GET /api/students/search/course?course=Java`

| Parameter | In | Type | Description |
|---|---|---|---|
| `course` | Query | String | Course name |

``` bash
curl "http://localhost:8080/api/students/search/course?course=Java"
```

### Paginated Listing

`GET /api/students/pagination?page=0&size=5`

| Parameter | In | Type | Description |
|---|---|---|---|
| `page` | Query | Integer | Zero-based page index |
| `size` | Query | Integer | Records per page |

**Response** `200 OK`

``` json
{
  "content": [
    {
      "id": 1,
      "name": "Amit Kumar",
      "email": "amit@gmail.com",
      "age": 22,
      "course": "Spring Boot"
    }
  ],
  "totalElements": 12,
  "totalPages": 3,
  "number": 0,
  "size": 5,
  "first": true,
  "last": false
}
```

``` bash
curl "http://localhost:8080/api/students/pagination?page=0&size=5"
```

------------------------------------------------------------------------

## Error Handling

The project includes a global exception handler for application errors,
including requests for student IDs that do not exist.

### HTTP Status Codes

| Status | Meaning | Typical Cause |
|---|---|---|
| `200 OK` | Request succeeded | Successful read, update, delete, or the current POST implementation |
| `400 Bad Request` | Invalid request | Malformed JSON or a request body the application cannot process |
| `404 Not Found` | Resource missing | Unknown student ID |
| `500 Internal Server Error` | Unexpected failure | Database or server error |

### Example Error Response

A request for a student ID that does not exist returns `404 Not Found`.

```json
{
  "error": "Student not found with ID: 999"
}
```

------------------------------------------------------------------------

## Testing the API

1.  Open Postman and create a collection named **Student Management
    API**.
2.  Add one request per endpoint, using `http://localhost:8080` as the
    base URL.
3.  For `POST` and `PUT`, set **Body → raw → JSON**.
4.  Send each request and verify the status code and response body.

Exporting the collection into a `postman/` folder in the repository lets
other contributors import and run it immediately.

## Troubleshooting

| Symptom | Likely Cause | Resolution |
|---|---|---|
| `Access denied for user` | Incorrect database credentials | Verify `DB_USERNAME` and `DB_PASSWORD` |
| `Unknown database 'student_management'` | Database not created | Run `CREATE DATABASE student_management;` |
| `Communications link failure` | MySQL is not running | Start the MySQL service |
| `Port 8080 already in use` | Port conflict | Change `server.port` or stop the conflicting process |
| `UnsupportedClassVersionError` | Wrong Java version | Configure JDK 17 in the IDE and set `JAVA_HOME` |

## Roadmap

-   [ ] OpenAPI / Swagger documentation (`springdoc-openapi`)
-   [ ] Authentication and authorization with Spring Security and JWT
-   [ ] DTO layer with MapStruct
-   [ ] Sorting on paginated results
-   [ ] Unit and integration tests (JUnit 5, Mockito, Testcontainers)
-   [ ] Docker and Docker Compose setup
-   [ ] CI pipeline with GitHub Actions

## Contributing

Contributions are welcome.

1.  Fork the repository.
2.  Create a feature branch: `git checkout -b feature/your-feature`
3.  Commit your changes: `git commit -m "Add your feature"`
4.  Push to your branch: `git push origin feature/your-feature`
5.  Open a pull request.

## Author

**Dhanush Thunikipati** GitHub: [@Dhanush
Thunikipati](https://github.com/Dhanushthunikipati-00)
