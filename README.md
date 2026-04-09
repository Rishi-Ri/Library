# Library Management (Spring Boot + PostgreSQL)
 
A simple Library Management REST API built with **Java 17** and **Spring Boot**, featuring CRUD operations for **Books** and **Members**, and **Issue/Return** records for tracking borrowed books.  
The API is CORS-enabled and designed with a clean, layered architecture: **Controller → Service → Repository → Model**.
 
> **Tech Stack**
>
> *   Java 17, Spring Boot
> *   Spring Web, Spring Data JPA (Hibernate)
> *   PostgreSQL
> *   Maven
 
***
 
## Features
 
*   **Books**
    *   Create, Read, Update, Delete books
*   **Members**
    *   Create, Read, Update, Delete members
*   **Issue Records**
    *   Issue a book to a member
    *   Return a book
    *   List all issue records
*   **CORS**
    *   Controllers allow all origins (`@CrossOrigin(origins = "*")`) – good for local testing with a frontend
 
***
## Landing Page
 
 src="<img width="1839" height="823" alt="image" src="https://github.com/user-attachments/assets/e5803e89-d3de-4c90-b4cc-a1d118ddfd27" />
" />
 
## Architecture
 
**Package structure (as per your screenshots):**
 
    src/main/java
    └── com.library.libraryManagement
        ├── LibraryManagementApplication.java
        ├── Controller
        │   ├── BooksController.java
        │   ├── IssueRecordController.java
        │   └── MemberController.java
        ├── model
        │   ├── Books.java
        │   ├── IssueRecord.java
        │   └── Member.java
        ├── Repository
        │   ├── BooksRepository.java
        │   ├── IssueRecordRepository.java
        │   └── MemberRepository.java
        └── Service
            ├── BookService.java
            ├── IssueRecordService.java
            └── MemberService.java
 
    src/main/resources
    └── application.properties
 
    pom.xml
 
**Layered flow (ASCII diagram):**
 
    [Client/Frontend/REST Client]
                |
                v
         [Controller Layer]
                |
                v
          [Service Layer]
                |
                v
        [Repository Layer]
                |
                v
         [PostgreSQL Database]
 
*   **Controller**: HTTP endpoint definitions, request/response handling.
*   **Service**: Business logic (e.g., issue/return behavior).
*   **Repository**: Data access via Spring Data JPA.
*   **Model**: JPA entities for Books, Members, and IssueRecord.
 
***
 
## Configuration
 
Your `application.properties`:
 
```properties
spring.application.name=libraryManagement
 
spring.datasource.url=jdbc:postgresql://localhost:5432/mapped
spring.datasource.username=postgres
spring.datasource.password=1234
spring.datasource.driver-class-name=org.postgresql.Driver
 
spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.hibernate.ddl-auto=update
# Options: create, create-drop, validate, update, none
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
```
 
> **Notes**
>
> *   Ensure the database `mapped` exists in PostgreSQL and is accessible with the provided credentials.
> *   `spring.jpa.hibernate.ddl-auto=update` will create/update tables based on your entities.
> *   `show-sql=true` + `format_sql=true` helps you see executed SQL in logs during development.
 
***
 
## How to Run
 
### Prerequisites
 
*   Java 17 (JDK)
*   Maven 3.8+
*   PostgreSQL running locally with database `mapped`
 
### Steps
 
1.  **Create database (if not exists):**
    ```sql
    CREATE DATABASE mapped;
    ```
2.  **Update credentials** in `src/main/resources/application.properties` if needed.
3.  **Build the project:**
    ```bash
    mvn clean install
    ```
4.  **Run the application:**
    ```bash
    mvn spring-boot:run
    ```
    or run the generated JAR:
    ```bash
    java -jar target/libraryManagement-*.jar
    ```
 
By default, the API will be available at:  
`http://localhost:8080`
 
***
 
## REST API Endpoints
 
### Books (`/books`)
 
*   **GET** `/books` — get all books
*   **GET** `/books/{id}` — get a book by ID
*   **POST** `/books` — add a new book
*   **PUT** `/books/{id}` — update a book by ID
*   **DELETE** `/books/delete/{id}` — delete a book by ID
 
**Sample JSON (Books):**
 
```json
{
  "id": 1,
  "title": "Clean Code",
  "author": "Robert C. Martin",
  "isbn": "9780132350884",
  "available": true
}
```
 
**Examples (cURL):**
 
```bash
# Get all books
curl -X GET http://localhost:8080/books
 
# Get book by ID
curl -X GET http://localhost:8080/books/1
 
# Create book
curl -X POST http://localhost:8080/books \
  -H "Content-Type: application/json" \
  -d '{"title":"Clean Code","author":"Robert C. Martin","isbn":"9780132350884","available":true}'
 
# Update book
curl -X PUT http://localhost:8080/books/1 \
  -H "Content-Type: application/json" \
  -d '{"title":"Clean Code (Updated)","author":"Robert C. Martin","isbn":"9780132350884","available":true}'
 
# Delete book
curl -X DELETE http://localhost:8080/books/delete/1
```
 
***
### Member Page
<img width="945" height="426" alt="{7AC0DC6D-617E-46BD-B2FD-3966BF410979}" src="<img width="1839" height="824" alt="image" src="https://github.com/user-attachments/assets/c5c85217-ff8d-4672-ab70-a254362d83d3" />
" />
 
 
***
### Members (`/member`)
 
*   **GET** `/member` — get all members
*   **GET** `/member/{member_Id}` — get member by ID
*   **POST** `/member` — add a new member
*   **PUT** `/member/{member_Id}` — update a member by ID
*   **DELETE** `/member/delete/{member_Id}` — delete a member by ID
 
**Sample JSON (Member):**
 
```json
{
  "member_Id": 101,
  "name": "Alice Johnson",
  "email": "alice@example.com",
  "active": true
}
```
 
**Examples (cURL):**
 
```bash
# Get all members
curl -X GET http://localhost:8080/member
 
# Get member by ID
curl -X GET http://localhost:8080/member/101
 
# Create member
curl -X POST http://localhost:8080/member \
  -H "Content-Type: application/json" \
  -d '{"name":"Alice Johnson","email":"alice@example.com","active":true}'
 
# Update member
curl -X PUT http://localhost:8080/member/101 \
  -H "Content-Type: application/json" \
  -d '{"name":"Alice J.","email":"alice@example.com","active":true}'
 
# Delete member
curl -X DELETE http://localhost:8080/member/delete/101
```
 
***
 
### Record Page
<img width="943" height="424" alt="{F7DAF67F-CB81-42EB-9B9E-12DA25BB93F0}" src="https://github.com/user-attachments/assets/a9bba6da-299b-4c9e-8e0a-ce7f6714a108" />
 
***
### Issue Records (`/record`)
 
*   **POST** `/record/issued/{id}/{member_ID}` — issue the book `{id}` to member `{member_ID}`
    *   Returns `201 Created` with `IssueRecord`
*   **POST** `/record/ret/{id}` — return the issued record `{id}` (marks returned)
*   **GET** `/record` — list all issue records
 
**Sample JSON (IssueRecord):**
 
```json
{
  "id": 5001,
  "bookId": 1,
  "memberId": 101,
  "issuedDate": "2025-12-01T10:00:00",
  "returnedDate": null,
  "status": "ISSUED"
}
```
 
**Examples (cURL):**
 
```bash
# Issue a book to a member
curl -X POST http://localhost:8080/record/issued/1/101
 
# Return an issued record
curl -X POST http://localhost:8080/record/ret/5001
 
# Get all issue records
curl -X GET http://localhost:8080/record
```
 
***
 
 
## Error Handling (Typical)
 
*   **404 Not Found** — when resources don’t exist (e.g., invalid book/member ID)
*   **400 Bad Request** — invalid payloads
*   **409 Conflict** — when issuing an already issued/unavailable book (depends on your service logic)
*   **500 Internal Server Error** — unexpected server-side errors
 
> Consider adding a global exception handler via `@RestControllerAdvice` for consistent error responses.
 
***
 
## CORS
 
All three controllers include:
 
```java
@CrossOrigin(origins = "*")
```
 
This allows requests from any origin—handy during local development with a separate frontend.  
For production, restrict origins to your domain.
 
***
 
## Build & Test
 
```bash
# build
mvn clean package
 
# run unit tests (if present)
mvn test
```
 
***
 
## Swagger (Optional)
 
If you add Swagger/OpenAPI, you can document and test endpoints interactively.  
Typical setup (dependencies + config) will expose a UI at:
 
*   `http://localhost:8080/swagger-ui/index.html`
*   `http://localhost:8080/v3/api-docs`
 
***
 
## Future Enhancements
 
*   Validation with `javax.validation` annotations on DTOs/entities
*   Pagination for listing endpoints
*   Role-based auth (Spring Security + JWT)
*   Business rules (e.g., max books per member)
*   Proper DTOs and manual mapping (as per your preference)
*   Postman collection export for quick testing
 
***
 
## License
 
This repository is for educational/demo purposes. Add a license (e.g., MIT) if you plan to share or reuse.
 
***
 
## Maintainers
 
*   **Abhishek Kumar** — Software Engineer (Noida)
 
***
 
### Quick Tips
 
*   If you see `Request failed with status code 404`, verify:
    *   The endpoint path (`/books`, `/member`, `/record`) and HTTP method
    *   The server is running on the correct port
    *   Path variables are valid numeric IDs
*   Keep your PostgreSQL service running and ensure credentials in `application.properties` are correct.
 
***
 
If you’d like, I can also generate a **Postman collection JSON** or a **Dockerfile + docker-compose.yml** for PostgreSQL to make setup one command.
