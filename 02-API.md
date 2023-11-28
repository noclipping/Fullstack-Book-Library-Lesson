### 🌐 Lesson Overview

This lesson will guide you through building a RESTful API using Node.js and Express to perform Create, Read, Update, and Delete (CRUD) operations on a PostgreSQL database hosting book and author data.

### 🔧 Prerequisites

- Node.js and npm installed.
- PostgreSQL database running locally with `books_db` setup as per the previous lesson.
- Basic understanding of JavaScript and SQL.

### 🛠️ Step 1: Initialize Node.js Project

Open your terminal, navigate to your project directory, and initialize a Node.js project:

```bash
npm init -y
```

### 📦 Step 2: Install Dependencies

Install Express and pg (PostgreSQL client for Node.js):

```bash
npm install express pg
```

### 🚧 Step 3: Setup Express Server

Create a file `app.js` and set up the Express server:

```javascript
const express = require("express");
const app = express();
app.use(express.json()); // for parsing application/json

app.listen(3000, () => {
  console.log("Server is running on port 3000");
});
```

### 🗄️ Step 4: Database Connection

Set up PostgreSQL connection in `app.js`:

```javascript
const { Pool } = require("pg");

const pool = new Pool({
  user: "your_username",
  host: "localhost",
  database: "books_db",
  password: "your_password",
  port: 5432,
});

// Test DB connection
pool.query("SELECT NOW()", (err, res) => {
  console.log(err, res);
  pool.end();
});
```

Replace `your_username` and `your_password` with your PostgreSQL credentials.

### 📚 Step 5: CRUD API Endpoints

#### Create (POST)

Add a new book:

```javascript
app.post("/books", async (req, res) => {
  const { title, author_id, publication_year } = req.body;
  const result = await pool.query(
    "INSERT INTO books (title, author_id, publication_year) VALUES ($1, $2, $3) RETURNING *",
    [title, author_id, publication_year]
  );
  res.json(result.rows[0]);
});
```

#### Read (GET)

Retrieve all books:

```javascript
app.get("/books", async (req, res) => {
  const result = await pool.query("SELECT * FROM books");
  res.json(result.rows);
});
```

#### Update (PUT)

Update a book's details:

```javascript
app.put("/books/:id", async (req, res) => {
  const { id } = req.params;
  const { title, author_id, publication_year } = req.body;
  const result = await pool.query(
    "UPDATE books SET title = $1, author_id = $2, publication_year = $3 WHERE id = $4 RETURNING *",
    [title, author_id, publication_year, id]
  );
  res.json(result.rows[0]);
});
```

#### Delete (DELETE)

Remove a book:

```javascript
app.delete("/books/:id", async (req, res) => {
  const { id } = req.params;
  await pool.query("DELETE FROM books WHERE id = $1", [id]);
  res.json({ message: "Book deleted" });
});
```

### 🧪 Step 6: Testing the API

You can test your API endpoints using tools like Postman or cURL.

### ✨ Conclusion

You've created a Node.js and Express API for handling CRUD operations on your PostgreSQL `books_db`. You can extend this API to include more complex operations, authentication, and error handling as needed.
