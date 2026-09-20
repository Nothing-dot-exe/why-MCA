# Master Study Notes: Web Technologies & Application Development
## Course Code: 22MCA24 / MMC205 | Visvesvaraya Technological University (VTU)
### Department of MCA | BKIT College, Bhalki

---

## 📑 Syllabus Architecture & Module Breakdown

* **Module 1**: HTML5 Semantic Elements, Modern CSS3 Layouts (Flexbox, CSS Grid), Responsive Web Design & Media Queries.
* **Module 2**: JavaScript ES6+ Deep-Dive (Closures, Promises, Async/Await, Destructuring, Event Bubbling & DOM API).
* **Module 3**: Front-End Engineering with React.js (Virtual DOM, JSX, Hooks: `useState`, `useEffect`, Component Lifecycle, State vs Props).
* **Module 4**: Backend Architecture with Node.js & Express.js (Event Loop, Middleware, RESTful API Design, Route Controllers).
* **Module 5**: Full-Stack Integration, MongoDB with Mongoose ODM, JWT Token-Based Authentication, Password Hashing (`bcrypt`), Web Security (CORS, XSS, CSRF).

---

# MODULE 1: HTML5 & CSS3 MODERN LAYOUTS

## 1.1 HTML5 Semantic Web Architecture
Semantic elements describe their meaning to both browser and search engine crawlers:
* `<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<aside>`, `<footer>`.

---

## 1.2 CSS3 Flexbox vs. CSS Grid

| Feature | CSS Flexbox | CSS Grid |
| :--- | :--- | :--- |
| **Dimension** | One-dimensional (Row **OR** Column) | Two-dimensional (Rows **AND** Columns simultaneously) |
| **Design Approach** | Content-first (Items dictate layout) | Layout-first (Predefined grid tracks) |
| **Parent Properties** | `display: flex`, `justify-content`, `align-items`, `flex-direction` | `display: grid`, `grid-template-columns`, `grid-template-rows`, `gap` |
| **Best Used For** | Navigation bars, dynamic aligning of elements | Full page templates, card dashboards, complex grids |

### Flexbox Centering Code (The Industry Standard):
```css
.center-container {
    display: flex;
    justify-content: center; /* Horizontally center */
    align-items: center;     /* Vertically center */
    min-height: 100vh;
}
```

---

# MODULE 2: JAVASCRIPT ES6+ & ASYNC PROGRAMMING

## 2.1 Variables, Scope & Hoisting
* `var`: Function-scoped, hoisted and initialized with `undefined`.
* `let` and `const`: Block-scoped (`{ ... }`), hoisted into **Temporal Dead Zone (TDZ)**; cannot be accessed before declaration.

---

## 2.2 Promises and Async / Await
A **Promise** represents the eventual completion (or failure) of an asynchronous operation with three states: *Pending*, *Fulfilled*, or *Rejected*.

```javascript
// Consuming APIs cleanly with modern Async/Await
async function fetchStudentData(studentId) {
    try {
        const response = await fetch(`https://api.bkit.ac.in/students/${studentId}`);
        if (!response.ok) {
            throw new Error(`HTTP Error: Status ${response.status}`);
        }
        const data = await response.json();
        console.log("Student Name:", data.name);
        return data;
    } catch (error) {
        console.error("Failed to fetch data:", error.message);
    }
}
```

---

# MODULE 3: FRONT-END WITH REACT.JS

## 3.1 Virtual DOM Mechanics
1. **Render**: When state changes, React renders a complete new Virtual DOM tree representation in memory.
2. **Diffing**: React compares the new Virtual DOM with the previous snapshot using a heuristic $O(n)$ diffing algorithm.
3. **Reconciliation**: React batches the computed minimal differences and updates **only the modified elements** in the real browser DOM.

---

## 3.2 Core React Hooks
* `useState`: Manages local component state.
* `useEffect`: Handles side effects (data fetching, subscriptions, DOM mutations):
  * `useEffect(fn, [])`: Runs **only once** on component mount.
  * `useEffect(fn, [prop])`: Runs on mount and whenever `prop` changes.

```jsx
import React, { useState, useEffect } from 'react';

function StudentLiveSearch() {
    const [query, setQuery] = useState("");
    const [results, setResults] = useState([]);

    useEffect(() => {
        if (query.trim() === "") return;
        const controller = new AbortController();

        fetch(`/api/search?q=${query}`, { signal: controller.signal })
            .then(res => res.json())
            .then(data => setResults(data))
            .catch(err => { if (err.name !== 'AbortError') console.error(err); });

        return () => controller.abort(); // Cleanup on unmount or query change
    }, [query]);

    return (
        <div>
            <input 
                type="text" 
                value={query} 
                onChange={(e) => setQuery(e.target.value)} 
                placeholder="Search students..." 
            />
            <ul>
                {results.map(st => <li key={st.id}>{st.name} - {st.usn}</li>)}
            </ul>
        </div>
    );
}
```

---

# MODULE 4: BACKEND WITH NODE.JS & EXPRESS

## 4.1 Node.js Event-Driven Non-Blocking I/O
Node.js runs on a single-threaded **Event Loop** powered by Google Chrome's V8 engine and `libuv`:
1. Network I/O, file reading, and timers are offloaded to OS kernel threads or the `libuv` worker pool.
2. When an operation finishes, callback functions are placed into the callback queue.
3. The Event Loop continuously checks if the main call stack is empty and executes waiting callbacks without blocking other incoming requests.

---

## 4.2 RESTful API Design in Express.js
```javascript
const express = require('express');
const app = express();
app.use(express.json()); // Built-in middleware to parse JSON request bodies

let books = [
    { id: 1, title: "Discrete Mathematics", author: "Levin" },
    { id: 2, title: "Pro Git", author: "Chacon" }
];

// GET: Retrieve all books
app.get('/api/books', (req, res) => {
    res.status(200).json(books);
});

// POST: Create a new book
app.post('/api/books', (req, res) => {
    const { title, author } = req.body;
    if (!title || !author) {
        return res.status(400).json({ error: "Title and author are required." });
    }
    const newBook = { id: books.length + 1, title, author };
    books.push(newBook);
    res.status(201).json(newBook);
});

app.listen(3000, () => console.log("Server running on port 3000"));
```

---

# MODULE 5: AUTHENTICATION & SECURITY

## 5.1 JWT (JSON Web Token) Structure
A JWT is a compact, URL-safe means of representing claims between two parties, separated by dots (`.`):
$$\text{JWT} = \text{Base64Url}(\text{Header}) \,.\, \text{Base64Url}(\text{Payload}) \,.\, \text{Signature}$$
1. **Header**: Token type (`JWT`) and hashing algorithm (`HS256`, `RS256`).
2. **Payload**: Claims (User ID, role, expiration time `exp`).
3. **Signature**: Hash calculated using a server-side secret key:
   $$\text{HMACSHA256}(\text{Base64Url}(H) + "." + \text{Base64Url}(P), \; \text{secret})$$

---

## 5.2 Web Security Top 3 Defenses
1. **SQL / NoSQL Injection**: Always use parameterized queries or ORMs/ODMs (`Mongoose`, `Sequelize`); never concatenate raw strings into queries.
2. **Cross-Site Scripting (XSS)**: Sanitize user input, encode output before rendering, and enforce a strict **Content Security Policy (CSP)**.
3. **Cross-Site Request Forgery (CSRF)**: Use anti-CSRF tokens and set cookie attributes to `SameSite=Strict; Secure; HttpOnly`.
