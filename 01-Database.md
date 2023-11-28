### 📘 Lesson Overview

This lesson guides you through setting up a PostgreSQL (psql) database via the command line interface (CLI). We'll create a database for storing books and their authors' details.

### 🔧 Prerequisites

- PostgreSQL installed on your system.
- Basic knowledge of SQL and command line operations.

### 🚀 Step 1: Launch psql

Open your terminal or command prompt and type:

```bash
psql -U [username]
```

Replace `[username]` with your PostgreSQL username.

### 🗄️ Step 2: Create Database

Once inside psql, create a new database:

```sql
CREATE DATABASE books_db;
```

Switch to the new database:

```sql
\c books_db
```

### 📚 Step 3: Create Tables

#### Book Table

Create a table for books:

```sql
CREATE TABLE books (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    author_id INT,
    publication_year INT
);
```

#### 🖋️ Author Table

Create a table for authors:

```sql
CREATE TABLE authors (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    dob DATE,
    nationality VARCHAR(100)
);
```

### 🔗 Step 4: Define Relationships

Establish a foreign key relationship between the `books` and `authors` tables:

```sql
ALTER TABLE books
ADD FOREIGN KEY (author_id)
REFERENCES authors (id);
```

### 📖 Step 5: Insert Data

Insert sample data into the authors table:

```sql
INSERT INTO authors (name, dob, nationality)
VALUES ('J.K. Rowling', '1965-07-31', 'British');
```

Add a book entry:

```sql
INSERT INTO books (title, author_id, publication_year)
VALUES ('Harry Potter and the Philosopher''s Stone', 1, 1997);
```

### 🔍 Step 6: Query Data

Retrieve data to verify the setup:

```sql
SELECT * FROM books JOIN authors ON books.author_id = authors.id;
```

### 🛠️ Step 7: Exit psql

To exit psql, type:

```sql
\q
```

### ✨ Conclusion

Congratulations! You've successfully created a PostgreSQL database using CLI, complete with tables for books and authors, and established relationships between them. Experiment with more complex queries and operations to further enhance your database.
