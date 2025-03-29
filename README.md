html code
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Personal Book Library</title>
  <link rel="stylesheet" href="code3.css" />
</head>
<body>

  <header>
    <h1>📚 My Book Library</h1>
  </header>

  <main>
    <!-- Add Book Form -->
    <section class="add-book">
      <h2>Add a New Book</h2>
      <form id="bookForm">
        <input type="text" id="title" placeholder="Book Title" required />
        <input type="text" id="author" placeholder="Author" required />
        <input type="text" id="category" placeholder="Category" required />
        <button type="submit">Add Book</button>
      </form>
    </section>

    <!-- Search Section -->
    <section class="search-books">
      <h2>Search Books</h2>
      <input type="text" id="searchInput" placeholder="Search by Title" onkeyup="searchBooks()" />
    </section>

    <!-- Book List -->
    <section class="book-list">
      <h2>Book Collection</h2>
      <div id="booksContainer"></div>
    </section>
  </main>

  <footer>
    <p>&copy; 2025 My Book Library</p>
  </footer>

  <script src="code3.js"></script>
</body>
</html>

css code
* {
  box-sizing: border-box;
}

body {
  font-family: 'Arial', sans-serif;
  margin: 0;
  padding: 0;
  background-color: #f9f9f9;
  color: #333;
}

header {
  background-color: #4a90e2;
  color: white;
  text-align: center;
  padding: 1.5em 0;
}

main {
  padding: 20px;
  max-width: 800px;
  margin: 0 auto;
}

section {
  background: white;
  padding: 20px;
  margin-bottom: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

h1, h2 {
  margin-bottom: 15px;
}

input, button {
  display: block;
  width: 100%;
  margin-bottom: 10px;
  padding: 12px;
  border-radius: 5px;
  border: 1px solid #ccc;
}

button {
  background-color: #4a90e2;
  color: white;
  font-weight: bold;
  border: none;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #357ab8;
}

.book-card {
  border: 1px solid #ddd;
  padding: 15px;
  margin-bottom: 10px;
  border-radius: 8px;
  background-color: #fafafa;
}

.book-card h3 {
  margin-top: 0;
}

.borrow-btn {
  background-color: #f06292;
  margin-top: 10px;
}

.borrow-btn:hover {
  background-color: #c2185b;
}

.history {
  margin-top: 10px;
}

footer {
  text-align: center;
  padding: 1em 0;
  background-color: #eee;
  color: #666;
  font-size: 14px;
}

javascript code
const bookForm = document.getElementById("bookForm");
const booksContainer = document.getElementById("booksContainer");
const searchInput = document.getElementById("searchInput");

let books = [];

// Add Book Form Submit Event
bookForm.addEventListener("submit", function (e) {
  e.preventDefault();

  const title = document.getElementById("title").value.trim();
  const author = document.getElementById("author").value.trim();
  const category = document.getElementById("category").value.trim();

  if (!title || !author || !category) {
    alert("Please fill in all fields!");
    return;
  }

  const newBook = {
    id: Date.now(),
    title,
    author,
    category,
    borrowed: false,
    history: []
  };

  books.push(newBook);
  bookForm.reset();
  displayBooks();
});

// Display Books
function displayBooks(filteredBooks = books) {
  booksContainer.innerHTML = "";

  if (filteredBooks.length === 0) {
    booksContainer.innerHTML = "<p>No books available.</p>";
    return;
  }

  filteredBooks.forEach(book => {
    const bookCard = document.createElement("div");
    bookCard.className = "book-card";

    bookCard.innerHTML = `
      <h3>${book.title}</h3>
      <p><strong>Author:</strong> ${book.author}</p>
      <p><strong>Category:</strong> ${book.category}</p>
      <p><strong>Status:</strong> ${book.borrowed ? "Borrowed" : "Available"}</p>

      <button class="borrow-btn" onclick="toggleBorrow(${book.id})">
        ${book.borrowed ? "Return Book" : "Borrow Book"}
      </button>

      <div class="history">
        <h4>Borrow History:</h4>
        <ul>
          ${book.history.map(entry => `<li>${entry}</li>`).join("")}
        </ul>
      </div>
    `;

    booksContainer.appendChild(bookCard);
  });
}

// Toggle Borrow Status
function toggleBorrow(bookId) {
  const book = books.find(b => b.id === bookId);

  if (!book) return;

  if (!book.borrowed) {
    const borrower = prompt("Enter borrower's name:");
    if (!borrower) return;
    const date = new Date().toLocaleString();
    book.history.push(`Borrowed by ${borrower} on ${date}`);
    book.borrowed = true;
  } else {
    const date = new Date().toLocaleString();
    book.history.push(`Returned on ${date}`);
    book.borrowed = false;
  }

  displayBooks();
}

// Search Function
function searchBooks() {
  const query = searchInput.value.toLowerCase();

  const filteredBooks = books.filter(book =>
    book.title.toLowerCase().includes(query)
  );

  displayBooks(filteredBooks);
}
