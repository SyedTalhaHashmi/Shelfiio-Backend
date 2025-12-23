# 📚 Shelfio - Personal Library Management System

A comprehensive Spring Boot REST API for managing your personal book library with reading progress tracking, reviews, and collections.

![Java](https://img.shields.io/badge/Java-17-orange)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.5.6-brightgreen)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Latest-blue)
![License](https://img.shields.io/badge/License-MIT-yellow)

---

## 📑 Table of Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Architecture](#-architecture)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [API Documentation](#-api-documentation)
- [Database Schema](#-database-schema)
- [Configuration](#-configuration)
- [Error Handling](#-error-handling)
---

## ✨ Features

### 📖 Book Management
- **Add books by ISBN** - Automatically fetch book details from external APIs
- **Manual book entry** - Add books without ISBN (personal notes, manuscripts)
- **Reading status tracking** - Not Started, Reading, Finished
- **Reading progress** - Track pages read per book
- **Book queries** - Get all books, latest added, last 3 books, by status, by category
- **Delete books** - Remove books from library with cascade deletion

### ⭐ Review System
- **Multiple reviews per book** - Add unlimited reviews to any book
- **Edit reviews** - Update existing reviews
- **Delete reviews** - Remove reviews
- **Rating system** - 1-5 star ratings with comments
- **Review history** - All reviews included in book responses

### 📚 Collections
- **Create collections** - Organize books into custom collections
- **Manage collections** - Add/remove books, delete collections
- **View collections** - Get all collections with their books

### 📊 Statistics & Progress
- **Total pages read** - Sum of all books' pages read
- **Reading statistics** - Books count, pages read, average pages per book
- **Books count** - Total number of books in library

---

## 🛠 Tech Stack

### Backend
- **Java 17** - Programming language
- **Spring Boot 3.5.6** - Application framework
- **Spring Data JPA** - Data persistence
- **Hibernate** - ORM
- **PostgreSQL** - Database
- **Lombok** - Boilerplate code reduction
- **Jakarta Validation** - Input validation

### Architecture Pattern
- **Layered Architecture** - Controller → Service → Repository
- **RESTful API** - Standard HTTP methods and status codes
- **DTO Pattern** - Data Transfer Objects for API responses
- **Exception Handling** - Global exception handler with consistent error responses

---

## 🏗 Architecture

### Layered Architecture Overview

```
┌─────────────────────────────────────────┐
│          Controller Layer               │
│  (REST endpoints, request validation)   │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│          Service Layer                  │
│  (Business logic, validation)           │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│          Repository Layer               │
│  (Database operations via JPA)          │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│          Database (PostgreSQL)          │
│  (Data persistence)                     │
└─────────────────────────────────────────┘
```

### Component Flow

```
Request → Controller → Service → Repository → Database
                ↓
         Exception Handler (if error)
                ↓
         ApiResponse<T> → JSON Response
```

### Key Components

#### 1. **Controllers** (`controller/`)
- Handle HTTP requests
- Validate input using `@Valid`
- Delegate to service layer
- Return standardized `ApiResponse<T>` wrapper

#### 2. **Services** (`service/` & `service/impl/`)
- Contain business logic
- Validate business rules
- Orchestrate repository calls
- Throw custom exceptions

#### 3. **Repositories** (`repository/`)
- Interface with database
- Extend `JpaRepository`
- Custom queries using JPQL

#### 4. **Entities** (`entity/`)
- JPA entities mapping to database tables
- Relationships: `@ManyToOne`, `@OneToMany`

#### 5. **DTOs** (`dto/`)
- **Request DTOs** - Input validation
- **Response DTOs** - Structured API responses

#### 6. **Mappers** (`mapper/`)
- Convert entities to DTOs
- Utility classes with static methods

#### 7. **Exception Handling** (`exception/`)
- Custom exception classes
- Global exception handler (`@RestControllerAdvice`)
- Consistent error responses

---

## 📁 Project Structure

```
shelfio/
├── src/main/java/com/shelfio/shelfio/
│   ├── config/
│   │   └── CorsConfig.java                 # CORS configuration
│   │
│   ├── controller/
│   │   ├── BookController.java             # Book endpoints
│   │   ├── CollectionController.java       # Collection endpoints
│   │   ├── ReviewController.java           # Review endpoints
│   │   └── UserStatsController.java        # Statistics endpoints
│   │
│   ├── dto/
│   │   ├── request/
│   │   │   ├── AddReviewRequest.java
│   │   │   ├── CreateBookRequest.java
│   │   │   ├── CreateCollectionRequest.java
│   │   │   ├── UpdatePagesReadRequest.java
│   │   │   ├── UpdateReadingStatusRequest.java
│   │   │   └── UpdateReviewRequest.java
│   │   │
│   │   └── response/
│   │       ├── ApiResponse.java            # Standard response wrapper
│   │       ├── BookResponseDto.java
│   │       ├── CollectionResponseDto.java
│   │       ├── ReviewResponseDto.java
│   │       └── TotalPagesReadResponseDto.java
│   │
│   ├── entity/
│   │   ├── Author.java
│   │   ├── Book.java
│   │   ├── Category.java
│   │   ├── Collection.java
│   │   ├── ReadingStatus.java
│   │   ├── Review.java
│   │   └── UserStats.java
│   │
│   ├── exception/
│   │   ├── GlobalExceptionHandler.java     # Centralized exception handling
│   │   ├── ExternalServiceException.java
│   │   ├── InvalidInputException.java
│   │   ├── ResourceAlreadyExistsException.java
│   │   └── ResourceNotFoundException.java
│   │
│   ├── mapper/
│   │   ├── BookMapper.java
│   │   ├── CollectionMapper.java
│   │   └── ReviewMapper.java
│   │
│   ├── repository/
│   │   ├── AuthorRepository.java
│   │   ├── BookRepository.java
│   │   ├── CategoryRepository.java
│   │   ├── CollectionRepository.java
│   │   ├── ReadingStatusRepository.java
│   │   ├── ReviewRepository.java
│   │   └── UserStatsRepository.java
│   │
│   ├── service/
│   │   ├── BookService.java
│   │   ├── CollectionService.java
│   │   ├── ReviewService.java
│   │   ├── UserStatsService.java
│   │   │
│   │   └── impl/
│   │       ├── BookServiceImpl.java
│   │       ├── CollectionServiceImpl.java
│   │       ├── ReviewServiceImpl.java
│   │       └── UserStatsServiceImpl.java
│   │
│   ├── adapter/
│   │   └── BookDataAdapter.java            # External API integration
│   │
│   └── ShelfioApplication.java             # Main application class
│
├── src/main/resources/
│   └── application.properties              # Application configuration
│
├── pom.xml                                 # Maven dependencies
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

- **Java 17** or higher
- **Maven 3.6+**
- **PostgreSQL 12+**
- **IDE** (IntelliJ IDEA, Eclipse, VS Code)

### Installation Steps

#### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/shelfio.git
cd shelfio
```

#### 2. Setup PostgreSQL Database

Create a new database:

```sql
CREATE DATABASE shelfio;
```

Or use a cloud PostgreSQL instance (e.g., Neon, ElephantSQL).

#### 3. Configure Database Connection

Update `src/main/resources/application.properties`:

```properties
# Database Configuration
spring.datasource.url=jdbc:postgresql://localhost:5432/shelfio
spring.datasource.username=your_username
spring.datasource.password=your_password
spring.datasource.driver-class-name=org.postgresql.Driver

# JPA Configuration
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
```

#### 4. Initialize Reading Status Data

After first run, insert initial reading statuses:

```sql
INSERT INTO reading_status (status) VALUES 
    ('Not started'),
    ('Reading'),
    ('Finished');
```

#### 5. Build the Project

```bash
mvn clean install
```

#### 6. Run the Application

```bash
mvn spring-boot:run
```

The application will start on `http://localhost:8080`

#### 7. Test the API

```bash
# Health check
curl http://localhost:8080/actuator/health

# Get all books
curl http://localhost:8080/api/books
```

---

## 📚 API Documentation

### Base URL
```
http://localhost:8080
```

### Standard Response Format

All API responses follow this structure:

```json
{
  "success": true,
  "message": "Optional success message",
  "data": { /* Response data */ },
  "error": null,
  "timestamp": "2025-12-21T10:30:00"
}
```

Error response:

```json
{
  "success": false,
  "message": "Error message",
  "data": null,
  "error": {
    "code": "ERROR_CODE",
    "details": "Detailed error message"
  },
  "timestamp": "2025-12-21T10:30:00"
}
```

---

### 📖 Book Endpoints

#### Add Book by ISBN
```http
POST /api/books/isbn/{isbn}
```

Fetches book details from external API and adds to library.

**Response:**
```json
{
  "success": true,
  "message": "Book added successfully",
  "data": {
    "id": 1,
    "title": "Clean Code",
    "author": "Robert C. Martin",
    "category": "Programming",
    "isbn": "9780132350884",
    "status": "Not started",
    "pages": 464,
    "pagesRead": 0,
    "publisher": "Prentice Hall",
    "coverUrl": "https://...",
    "reviews": []
  }
}
```

#### Add Book Manually
```http
POST /api/books
Content-Type: application/json

{
  "title": "My Book",
  "authorFirstName": "John",
  "authorLastName": "Doe",
  "category": "Fiction",
  "status": "Not started",
  "pages": 300,
  "isbn": "1234567890",
  "publisher": "Publisher Name",
  "coverUrl": "https://..."
}
```

#### Get All Books
```http
GET /api/books
```

Returns all books ordered by most recent first.

#### Get Latest Book
```http
GET /api/books/latest
```

Returns the most recently added book.

#### Get Last 3 Books
```http
GET /api/books/recent
```

#### Get Books by Status
```http
GET /api/books/status/{status}
```

Valid statuses: `Not started`, `Reading`, `Finished`

#### Get Books by Category
```http
GET /api/books/category/{category}
```

#### Get Books Count
```http
GET /api/books/count
```

#### Update Reading Status
```http
PUT /api/books/{bookId}/status
Content-Type: application/json

{
  "status": "Reading"
}
```

#### Update Pages Read
```http
PUT /api/books/{bookId}/pages-read
Content-Type: application/json

{
  "pagesRead": 150
}
```

#### Delete Book
```http
DELETE /api/books/{bookId}
```

Cascades to reviews and collections.

#### Get Total Pages Read
```http
GET /api/books/stats/pages-read
```

Returns sum of all books' `pagesRead` values.

---

### ⭐ Review Endpoints

#### Add Review
```http
POST /api/books/{bookId}/review
Content-Type: application/json

{
  "rating": 5,
  "comment": "Excellent book!"
}
```

#### Get Review by ID
```http
GET /api/reviews/{reviewId}
```

#### Get All Reviews for Book
```http
GET /api/reviews/book/{bookId}
```

#### Update Review
```http
PUT /api/reviews/{reviewId}
Content-Type: application/json

{
  "rating": 4,
  "comment": "Updated review"
}
```

#### Delete Review
```http
DELETE /api/reviews/{reviewId}
```

---

### 📚 Collection Endpoints

#### Create Collection
```http
POST /api/collections
Content-Type: application/json

{
  "name": "Must Read Tech Books"
}
```

#### Get All Collections
```http
GET /api/collections
```

#### Get Collection by ID
```http
GET /api/collections/{id}
```

#### Add Book to Collection
```http
POST /api/collections/{collectionId}/books/{bookId}
```

#### Remove Book from Collection
```http
DELETE /api/collections/{collectionId}/books/{bookId}
```

#### Delete Collection
```http
DELETE /api/collections/{id}
```

---

### 📊 Statistics Endpoints

#### Get Total Pages Read (with breakdown)
```http
GET /api/stats/pages-read
```

**Response:**
```json
{
  "success": true,
  "data": {
    "bookPagesRead": 1425,
    "manualPagesRead": 150,
    "totalPagesRead": 1575
  }
}
```

#### Add Manual Pages
```http
POST /api/stats/pages-read
Content-Type: application/json

{
  "pagesToAdd": 50
}
```

---

## 🗄 Database Schema

### Entity Relationship Diagram

```
┌─────────────┐       ┌─────────────┐       ┌─────────────┐
│   Author    │       │    Book     │       │  Category   │
├─────────────┤       ├─────────────┤       ├─────────────┤
│ author_id   │◄──────┤ book_id     │──────►│ category_id │
│ first_name  │       │ title       │       │ name        │
│ last_name   │       │ isbn        │       └─────────────┘
└─────────────┘       │ pages       │
                      │ pages_read  │
                      │ publisher   │
                      │ bookcover   │       ┌──────────────┐
                      │ author_id   │       │ReadingStatus │
                      │ category_id │       ├──────────────┤
                      │ status_id   │◄──────┤ status_id    │
                      └──────┬──────┘       │ status       │
                             │              └──────────────┘
                    ┌────────┴────────┐
                    │                 │
            ┌───────▼────────┐ ┌─────▼──────┐
            │    Review      │ │ Collection │
            ├────────────────┤ └────────────┘
            │ review_id      │       │
            │ book_id        │       │
            │ rating         │       │
            │ comment        │  ┌────▼──────────────┐
            │ created_at     │  │ book_collection   │
            └────────────────┘  ├───────────────────┤
                                │ collection_id     │
                                │ book_id           │
                                └───────────────────┘
```

### Core Tables

#### books
```sql
CREATE TABLE books (
    book_id BIGSERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    isbn VARCHAR(20),
    pages INTEGER,
    pages_read INTEGER NOT NULL DEFAULT 0,
    publisher VARCHAR(255),
    bookcover VARCHAR(500),
    author_id BIGINT NOT NULL REFERENCES authors(author_id),
    category_id BIGINT NOT NULL REFERENCES categories(category_id),
    reading_status_id BIGINT NOT NULL REFERENCES reading_status(status_id)
);
```

#### reviews
```sql
CREATE TABLE reviews (
    review_id BIGSERIAL PRIMARY KEY,
    book_id BIGINT NOT NULL REFERENCES books(book_id) ON DELETE CASCADE,
    rating INTEGER NOT NULL CHECK (rating >= 1 AND rating <= 5),
    comment TEXT,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

#### authors
```sql
CREATE TABLE authors (
    author_id BIGSERIAL PRIMARY KEY,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL
);
```

#### categories
```sql
CREATE TABLE categories (
    category_id BIGSERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL UNIQUE
);
```

#### reading_status
```sql
CREATE TABLE reading_status (
    status_id BIGSERIAL PRIMARY KEY,
    status VARCHAR(50) NOT NULL UNIQUE
);

-- Initial data
INSERT INTO reading_status (status) VALUES 
    ('Not started'),
    ('Reading'),
    ('Finished');
```

#### collections
```sql
CREATE TABLE collections (
    collection_id BIGSERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL UNIQUE
);

CREATE TABLE book_collection (
    collection_id BIGINT NOT NULL REFERENCES collections(collection_id) ON DELETE CASCADE,
    book_id BIGINT NOT NULL REFERENCES books(book_id) ON DELETE CASCADE,
    PRIMARY KEY (collection_id, book_id)
);
```

#### user_stats
```sql
CREATE TABLE user_stats (
    stats_id BIGINT PRIMARY KEY DEFAULT 1,
    manual_pages_read INTEGER NOT NULL DEFAULT 0,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT chk_stats_id CHECK (stats_id = 1)
);
```

---

## ⚙️ Configuration

### Application Properties

```properties
# Application Name
spring.application.name=shelfio

# Database Configuration
spring.datasource.url=jdbc:postgresql://localhost:5432/shelfio
spring.datasource.username=postgres
spring.datasource.password=password
spring.datasource.driver-class-name=org.postgresql.Driver

# JPA Configuration
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.properties.hibernate.format_sql=true
spring.jpa.open-in-view=false

# Jackson JSON Configuration
spring.jackson.default-property-inclusion=non_null
spring.jackson.serialization.write-dates-as-timestamps=false
spring.jackson.serialization.indent-output=true

# Server Configuration
server.port=8080
server.error.include-message=always
server.error.include-binding-errors=always
server.error.include-stacktrace=never

# Logging
logging.level.root=INFO
logging.level.com.shelfio.shelfio=DEBUG
logging.level.org.hibernate.SQL=DEBUG

# Actuator
management.endpoints.web.exposure.include=health,info,metrics
```

### CORS Configuration

The application includes CORS configuration to allow cross-origin requests:

```java
@Configuration
public class CorsConfig {
    @Bean
    public CorsFilter corsFilter() {
        // Allows all origins, methods, and headers
        // Modify for production to restrict origins
    }
}
```

---

## 🚨 Error Handling

### Custom Exceptions

| Exception | HTTP Status | Use Case |
|-----------|-------------|----------|
| `ResourceNotFoundException` | 404 | Resource not found |
| `ResourceAlreadyExistsException` | 409 | Duplicate resource |
| `InvalidInputException` | 400 | Invalid input data |
| `ExternalServiceException` | 502 | External API failure |

### Global Exception Handler

All exceptions are caught by `GlobalExceptionHandler` and converted to consistent error responses:

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    
    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ApiResponse<Void>> handleResourceNotFound(
            ResourceNotFoundException ex) {
        // Returns 404 with error details
    }
    
    // ... other exception handlers
}
```

### Example Error Response

```json
{
  "success": false,
  "message": "Resource not found",
  "data": null,
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "details": "Book not found with ID: 999"
  },
  "timestamp": "2025-12-21T10:30:00"
}
```

### Prerequisites

- PostgreSQL database (cloud or self-hosted)
- Java 17 runtime
- 512MB+ RAM recommended
