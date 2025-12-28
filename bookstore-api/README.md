### A REST API for a bookstore built in Go. It provides CRUD operations for books using a MySQL database.

Architecture
1. Entry point (cmd/main/main.go)
- Creates a Gorilla Mux router
- Registers routes
- Starts the HTTP server on localhost:9010

2. Database configuration (pkg/config/app.go)
- Uses GORM to connect to MySQL
- Connection string: root:xyz%40%23@tcp(localhost:3306)/parship
- Exposes GetDB() to share the connection

3. Data model (pkg/models/book.go)
- Book struct with:
  - Name, Author, Publication
  - Embedded gorm.Model (adds ID, CreatedAt, UpdatedAt, DeletedAt)
- init() runs on import: connects to DB and auto-migrates the books table
- Methods:
  - CreateBook() — creates a new book
  - GetAllBooks() — returns all books
  - GetBookById() — finds a book by ID
  - DeleteBook() — deletes a book by ID

4. HTTP controllers (pkg/controllers/book-controller.go)
- Handlers that:
  - Parse requests
  - Call model methods
  - Return JSON responses
- Endpoints:
  - GetBooks — list all books
  - GetBookById — get one book
  - CreateBook — create a book
  - UpdateBook — update a book (partial updates)
  - DeleteBook — delete a book

5. Routes (pkg/routes/bookstore-routes.go)
- Defines URL patterns:
  - POST /book/ — create
  - GET /book/{bookId} — get by ID
  - PUT /book/{bookId} — update
  - DELETE /books/{bookId} — delete

6. Utilities (pkg/utils/utils.go)
- ParseBody() — reads JSON from the request body and unmarshals it into a struct

Data flow
- Request → Router matches URL pattern
- Router → Controller handler
- Controller → Parses request (JSON body or URL params)
- Controller → Calls model method
- Model → Executes GORM query
- Model → Returns data
- Controller → Marshals to JSON and writes response

- Technologies
- Gorilla Mux — HTTP router
- GORM — ORM for database operations
- MySQL — database
- Standard library — net/http, encoding/json