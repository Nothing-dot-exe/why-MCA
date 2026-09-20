# Full-Stack Mock Technical Interview Q&A & Answer Keys

---

## 🎯 Question 1: Explain the JavaScript Event Loop, Call Stack, Microtask Queue, and Macrotask Queue.
* **Category**: Core JavaScript / Node.js Runtime
* **Interviewer Prompt**: *"What is the exact execution output of the following snippet and why?"*

```javascript
console.log('1');
setTimeout(() => console.log('2'), 0);
Promise.resolve().then(() => console.log('3'));
process.nextTick(() => console.log('4'));
console.log('5');
```

### 💡 Answer Key & Explanation
* **Output**:
  ```
  1
  5
  4
  3
  2
  ```
* **Detailed Mechanical Explanation**:
  1. `console.log('1')` and `console.log('5')` execute synchronously on the Call Stack.
  2. `setTimeout(..., 0)` registers a timer in the Node.js Timer phase (Macrotask / Task Queue).
  3. `Promise.resolve().then(...)` queues a callback into the **Microtask Queue**.
  4. `process.nextTick(...)` queues into the **nextTick Queue**, which executes with highest priority before any other microtasks or the next event loop phase.
  5. Priority order: **Call Stack -> nextTick Queue -> Promise Microtask Queue -> Macrotask/Timer Queue**.

---

## 🎯 Question 2: How do you prevent SQL Injection and Cross-Site Scripting (XSS) in a Full-Stack application?
* **Category**: Full-Stack Security

### 💡 Answer Key
1. **Preventing SQL Injection**:
   - **Never concatenate raw user strings into SQL queries**.
   - Use **Parameterized Queries / Prepared Statements**:
     ```typescript
     // INSECURE:
     // db.query(`SELECT * FROM users WHERE email = '${req.body.email}'`);
     
     // SECURE (PostgreSQL parameterized query):
     await pool.query('SELECT * FROM users WHERE email = $1', [req.body.email]);
     ```
   - Parameterized queries send SQL code and user data in separate protocol frames. The database engine treats `$1` strictly as literal string data, rendering SQL syntax manipulation impossible.
2. **Preventing XSS (Cross-Site Scripting)**:
   - **Context-aware HTML Sanitization**: React escapes text by default (`<div>{userInput}</div>` encodes `<` to `&lt;`).
   - Avoid `dangerouslySetInnerHTML`. If raw HTML rendering is mandatory, pass it through `DOMPurify.sanitize()`.
   - Set HTTP Response Header `Content-Security-Policy: default-src 'self'`.
   - Store sensitive authentication tokens in **`HttpOnly; Secure; SameSite=Strict` cookies**, preventing JavaScript (`document.cookie`) from stealing JWTs during an XSS attack.

---

## 🎯 Question 3: How does Database Indexing work internally, and when does an index degrade performance?
* **Category**: Database Engineering (PostgreSQL / MySQL)

### 💡 Answer Key
1. **Internal Mechanism (B+ Tree)**:
   - Relational database indexes typically use a **B+ Tree**.
   - Keys are stored in sorted order. Internal nodes store navigation keys and child pointers; leaf nodes store data pointers (Row IDs) and are linked horizontally as a doubly linked list.
   - Lookups, range queries, and ordering require $O(\log N)$ disk I/O reads rather than an $O(N)$ full table scan.
2. **When Indexing Degrades Performance**:
   - **Write Overhead (INSERT, UPDATE, DELETE)**: Every write must rebalance the B+ Tree and split pages, reducing write throughput.
   - **Low Cardinality Columns**: Indexing a `gender` or `boolean_flag` column is counterproductive because the query optimizer will still prefer a full table scan.
   - **Storage bloat**: Massive indexes can exceed RAM (Buffer Pool size), causing page thrashing between disk and memory.

---

## 🎯 Question 4: How do you handle database migration and schema changes with zero downtime?
* **Category**: Production Engineering & System Reliability

### 💡 Answer Key: The Expand-and-Contract (Parallel Run) Pattern
1. **Step 1 (Expand)**: Add the new column/table as nullable in the database without deleting the old column.
2. **Step 2 (Dual-Write in Code)**: Deploy code that writes to both the old column and the new column, while still reading from the old column.
3. **Step 3 (Backfill)**: Run a background asynchronous script to backfill existing historic rows from the old column into the new column.
4. **Step 4 (Switch Reads)**: Deploy code to read from the new column.
5. **Step 5 (Contract)**: Stop writing to the old column, and drop the old column in a final scheduled migration.
