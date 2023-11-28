### 🎨 Lesson Overview

In this lesson, you'll learn how to build a React front-end application that interacts with a Node.js and Express API for CRUD operations on a PostgreSQL database of books and authors, using the Fetch API instead of Axios.

### 🔧 Prerequisites

- Node.js and npm installed.
- A running Node.js and Express API from a previous lesson.
- Basic understanding of JavaScript, React, and RESTful APIs.

### 🚀 Step 1: Initialize React App

Create a new React application:

```bash
npx create-react-app books-frontend
cd books-frontend
```

### 🌐 Step 2: Setup React Components

Structure your React components for managing books data.

#### App Component (`App.js`)

Setup the main component:

```javascript
import React, { useState, useEffect } from "react";
import BooksList from "./components/BooksList";
import AddBookForm from "./components/AddBookForm";

function App() {
  const [books, setBooks] = useState([]);

  useEffect(() => {
    fetchBooks();
  }, []);

  const fetchBooks = async () => {
    const response = await fetch("http://localhost:3000/books");
    const data = await response.json();
    setBooks(data);
  };

  return (
    <div>
      <h1>Book Library</h1>
      <AddBookForm fetchBooks={fetchBooks} />
      <BooksList books={books} fetchBooks={fetchBooks} />
    </div>
  );
}

export default App;
```

#### BooksList Component (`BooksList.js`)

Create a component for listing books:

```javascript
import React from "react";
import Book from "./Book";

const BooksList = ({ books, fetchBooks }) => {
  return (
    <div>
      {books.map((book) => (
        <Book key={book.id} book={book} fetchBooks={fetchBooks} />
      ))}
    </div>
  );
};

export default BooksList;
```

#### Book Component (`Book.js`)

Create a component for each book:

```javascript
import React from "react";

const Book = ({ book, fetchBooks }) => {
  const handleDelete = async () => {
    await fetch(`http://localhost:3000/books/${book.id}`, { method: "DELETE" });
    fetchBooks();
  };

  return (
    <div>
      <h3>{book.title}</h3>
      {/* Add more book details here */}
      <button onClick={handleDelete}>Delete</button>
    </div>
  );
};

export default Book;
```

#### AddBookForm Component (`AddBookForm.js`)

Create a component to add a new book:

```javascript
import React, { useState } from "react";

const AddBookForm = ({ fetchBooks }) => {
  const [title, setTitle] = useState("");
  // Add more states for other book attributes

  const handleSubmit = async (e) => {
    e.preventDefault();
    await fetch("http://localhost:3000/books", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({ title }), // Include other book attributes
    });
    fetchBooks();
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={title}
        onChange={(e) => setTitle(e.target.value)}
        placeholder="Title"
      />
      {/* Add more input fields for other book attributes */}
      <button type="submit">Add Book</button>
    </form>
  );
};

export default AddBookForm;
```

### 🖌️ Step 3: Styling

Add CSS to enhance the visual appeal of your application.

### 🧪 Step 4: Testing the App

Run your React application and test the interactions with your backend:

```bash
npm start
```

### ✨ Conclusion

You have created a React front-end application capable of performing CRUD operations on a book database, interfacing with your backend via the Fetch API. This setup exemplifies a full-stack JavaScript application, integrating a React frontend with a Node.js backend. You can extend this application with additional features like user authentication, advanced search, or responsive design.
