## Node.js Fundamentals — Runtime vs Browser

### 1. Runtime vs Browser — the most important mental model

This is **THE most important concept** in Node.js.

Before understanding Node.js, we must understand **JavaScript itself**.

JavaScript is **just a language**.

Just like:
- English is a language  
- Hindi is a language  

A **language cannot run by itself**.

A language always needs a **place to run**, called an **environment** or **runtime**.

So the first big idea is:

> JavaScript is the language.  
> Something else runs it.

---

### 2. Where can JavaScript run?

JavaScript can run in **two main environments**:

1. **Browser** (Chrome, Firefox, Edge, etc.)
2. **Node.js** (outside the browser)

Same language.  
Different environments.

Let’s understand each one step by step.

---

### 3. JavaScript in the Browser

When you open a website:

- Your browser (like Chrome) has a **JavaScript engine**
- Chrome uses **V8**
- Firefox uses **SpiderMonkey**

That engine:
- Reads JavaScript
- Executes it line by line

In the browser, JavaScript can:
- Read HTML
- Manipulate the DOM
- Handle user clicks
- Use browser APIs

Examples of browser APIs:
- `window`
- `document`
- `alert`
- `fetch`

---

### 4. Example: Browser-only JavaScript

This code works **only in the browser**:

    document.querySelector("button").addEventListener("click", () => {
      alert("Button clicked!");
    });

Why?

Because:
- `document` exists only in browsers
- `window` exists only in browsers

👉 **Node.js does NOT have `document` or `window`**

---

### 5. Now comes Node.js

So what is Node.js?

👉 **Node.js is a runtime that lets JavaScript run outside the browser**

In very simple words:

> Node.js lets JavaScript run on your computer  
> just like a normal program

This means:
- No browser needed
- No HTML required
- JavaScript can run directly on your machine or server

---

### 6. What Node.js gives JavaScript

Node.js gives JavaScript **superpowers** that browsers do not allow.

With Node.js, JavaScript can:

- Read files from disk
- Write files to disk
- Create servers
- Talk to databases
- Access OS features
- Run scripts and tools

These things are **restricted in browsers for security reasons**.

---

### 7. Example: Node-only JavaScript

This code works **only in Node.js**:

    const fs = require("fs");

    fs.writeFileSync("hello.txt", "Hello from Node.js");

Why?

Because:
- Browsers cannot access your file system
- Node.js can

👉 This will **fail in the browser**
👉 This works perfectly in **Node.js**

---

### 8. Key difference: Browser vs Node.js

| Feature             | Browser JavaScript | Node.js                  |
|---------------------|-------------------|--------------------------|
| Runs where?         | Inside browser    | On your machine/server   |
| DOM access          | ✅ Yes            | ❌ No                    |
| File system access  | ❌ No             | ✅ Yes                   |
| Backend servers     | ❌ No             | ✅ Yes                   |
| window / document   | ✅ Yes            | ❌ No                    |

---

### 9. The most important sentence (remember forever)

> JavaScript is the language  
> Browser and Node.js are environments  

Same language.  
Different worlds.

---

### 10. Why Node.js exists

Before Node.js:
- JavaScript = frontend only
- Backend was written in:
  - Java
  - PHP
  - Python

After Node.js:
- JavaScript = frontend + backend
- One language everywhere
- Same developer mindset
- Same tooling

👉 This is why Node.js **changed the industry**

---

### 11. What exactly is Node.js?

Node.js is:

- A **JavaScript runtime**
- Built on **Chrome’s V8 JavaScript engine**
- Cross-platform:
  - Windows
  - macOS
  - Linux
- Open-source
- Maintained by the **OpenJS Foundation**

Node.js allows JavaScript to:
- Run outside the browser
- Use system-level features
- Build servers and tools

---

### 12. Node.js architecture

Node.js uses:
- **Event-driven architecture**
- **Asynchronous, non-blocking I/O**

This means:
- It does not wait for one task to finish before starting another
- It can handle many requests efficiently

---

### 13. History of Node.js — where it came from

To run JavaScript, you always need a **JavaScript engine**.

Wherever JavaScript runs, a JS engine exists.

Examples:
- Chrome → V8
- Firefox → SpiderMonkey

---

### 14. Early days of Node.js

- Node.js was created by **Ryan Dahl** in **2009**
- Initially used **SpiderMonkey** (Firefox engine)
- Later switched to **V8** for better performance
- Original name of Node.js was **web.js**

---

### 15. Why Node.js was created

Before Node.js:
- Apache HTTP Server was commonly used
- Apache uses a **blocking architecture**

Problem:
- Each request blocks a thread
- More users = more threads = heavy system load

Ryan Dahl wanted:
- A **non-blocking server**
- High concurrency with fewer threads

That idea became **Node.js**

---

### 16. Non-blocking I/O advantage

Non-blocking architecture allows:
- Handling multiple requests simultaneously
- Using fewer system resources
- Better scalability

This is one of Node.js’s **biggest strengths**

---

### 17. Important milestones in Node.js history

- **2010** → NPM was introduced  
  - NPM = Node Package Manager  
  - Central registry for JavaScript packages  

- **2011** → Windows support added  
  - Initially Node.js supported only macOS and Linux  

- **2012** → Ryan Dahl left the project  
  - Isaac Z. Schlueter (creator of npm) took over  

- **2014** → io.js fork created by Fedor Indutny  

- **2015** → Node.js and io.js merged  
  - Node.js Foundation formed  

- **2019** → JS Foundation + Node.js Foundation merged  
  - New foundation: **OpenJS Foundation**

---

### 18. Final takeaway

- JavaScript is just a language
- Node.js is an environment that runs JavaScript
- Browser and Node.js serve different purposes
- Node.js made JavaScript a **full-stack language**

This mental model is the foundation of everything you’ll learn next in Node.js.
