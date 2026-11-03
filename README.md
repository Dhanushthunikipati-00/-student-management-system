# Student Management System

A backend REST API application built using **Java, Spring Boot, Spring Data JPA, and MySQL** to manage student records.

## Technologies Used

- Java 17
- Spring Boot
- Spring Data JPA
- Hibernate
- MySQL 8
- Maven
- IntelliJ IDEA
- Postman

## Features

- Add new student records
- Retrieve all students
- Retrieve a student by ID
- Update student details
- Delete student records
- Input validation using Jakarta Validation
- Global exception handling
- Search students by name
- Search students by course
- Pagination support

## Project Structure

```text
student_management
 └── src
     └── main
         ├── java
         │   └── com.example.student_management
         │       ├── controller
         │       ├── entity
         │       ├── repository
         │       ├── service
         │       └── exception
         └── resources
             └── application.properties
```

## Database Configuration

Create a MySQL database named:

```sql
CREATE DATABASE student_management;
```

Configure your MySQL connection in `application.properties`.

Do not commit your actual database password to GitHub.

## API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/students` | Add student |
| GET | `/api/students` | Get all students |
| GET | `/api/students/{id}` | Get student by ID |
| PUT | `/api/students/{id}` | Update student |
| DELETE | `/api/students/{id}` | Delete student |
| GET | `/api/students/search/name?name=Amit` | Search by name |
| GET | `/api/students/search/course?course=Java` | Search by course |
| GET | `/api/students/pagination?page=0&size=5` | Get paginated students |

## Sample Student JSON

```json
{
  "name": "Amit Kumar",
  "email": "amit@gmail.com",
  "age": 22,
  "course": "Spring Boot"
}
```

## How to Run the Project

1. Clone or download the repository.
2. Open the project in IntelliJ IDEA.
3. Configure MySQL credentials in `application.properties`.
4. Ensure Java 17 is configured.
5. Run `StudentManagementApplication.java`.
6. Test the REST APIs using Postman.

The application runs on:

`http://localhost:8080`

## Learning Outcomes

- Developed RESTful APIs using Spring Boot.
- Implemented CRUD operations using Spring Data JPA.
- Integrated a MySQL relational database.
- Applied input validation and exception handling.
- Implemented search functionality and pagination.
