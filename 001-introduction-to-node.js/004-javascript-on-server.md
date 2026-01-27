## Node.js — JavaScript on the Server


### 1. Node.js and its core philosophy

Node.js came with a **very important philosophy**:

👉 **JavaScript should not be limited to the browser**  
👉 **JavaScript should be able to run outside the browser, primarily on servers**

Before Node.js:
- JavaScript lived only inside browsers
- Backend servers were written in other languages

Node.js changed this completely.

---

### 2. What is a server?

A **server** is simply:

- A remote computer
- Connected to the internet
- Running continuously
- Waiting for requests

This remote computer:
- Stores your website or application
- Responds when users access it

So when we say *“JavaScript on the server”*, we literally mean:

> JavaScript running on another computer somewhere on the internet.

---

### 3. How a browser talks to a server

When you type a website URL in your browser:

    https://example.com

Here’s what happens conceptually:

1. You type a **domain name**
2. That domain name maps to an **IP address**  
3. That IP address points to a **server (machine)**  
4. Your browser sends a request to that server  
5. The server processes the request  
6. The server sends a response back  

So every browser request is:

> Your computer → Server computer → Response back

---

### 4. JavaScript before and after Node.js

**Before Node.js**:
- JavaScript = browser-only language
- Used mainly for:
  - Click handling
  - DOM manipulation
  - UI logic

**After Node.js**:
- JavaScript runs on:
  - Browser (frontend)
  - Server (backend)

👉 This made **full stack JavaScript** possible.

---

### 5. Full stack applications with JavaScript

With Node.js:

- Frontend → JavaScript
- Backend → JavaScript
- Same language everywhere
- Same developer mindset
- Same tooling

This is why Node.js became extremely popular.

---

### 6. What Node.js is actually written in

Important technical fact:

- **Node.js is written in C++**
- **V8 JavaScript Engine is also written in C++**

Node.js is not written in JavaScript.

Instead:
- Node.js is a **C++ application**
- It embeds the **V8 JavaScript engine**

---

### 7. What is V8 and why it matters

- V8 is a **JavaScript engine**
- Developed by Google
- Used in:
  - Chrome browser
  - Node.js

V8:
- Understands JavaScript
- Converts JavaScript into machine code
- Executes it very fast

---

### 8. V8 can be embedded

A very important concept:

👉 **V8 can be embedded into any C++ application**

That’s exactly what Node.js does.

- Node.js = C++ program
- V8 = embedded inside Node.js
- JavaScript runs through V8

---

### 9. JavaScript standards (ECMAScript)

JavaScript follows **ECMAScript standards**.

- ECMAScript defines:
  - Syntax
  - Language rules
  - Core behavior

JavaScript engines (like V8):
- Implement these standards
- Ensure JavaScript behaves consistently everywhere

---

### 10. What is a JavaScript runtime?

A **JavaScript runtime** is:

- A JavaScript engine (like V8)
- + additional APIs and capabilities

For Node.js, this means:

- V8 engine
- File system APIs
- Networking APIs
- OS-level access
- Timers, buffers, streams, etc.

So:

> JavaScript Runtime = JS Engine + Extra Powers

---

### 11. How high-level code gets executed

Computers **do not understand JavaScript or C++ directly**.

Computers understand only:

- Binary
- Machine code

So the flow is:

High-level language  
(C++, JavaScript)  
→ Machine code  
→ Binary  
→ Executed by computer

---

### 12. JavaScript execution flow (step by step)

When you write JavaScript:

1. You write JavaScript code
2. Node.js sends it to the JavaScript engine (V8)
3. V8 converts JavaScript into machine code
4. Machine code is converted into binary  
5. Computer executes the binary instructions  

So in simple words:

> You write JavaScript  
> V8 translates it  
> Computer executes it

---

### 13. Final mental model

- Node.js is a **C++ application**
- V8 is a **C++ JavaScript engine**
- JavaScript follows **ECMAScript**
- Node.js embeds V8
- V8 converts JavaScript into machine code
- Machine code runs on the server

This is why Node.js enables:

👉 **JavaScript on the server**  
👉 **Full stack JavaScript development**
