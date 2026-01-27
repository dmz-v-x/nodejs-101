## Node REPL and Running JavaScript with Node.js

### 1. What does “running JavaScript” actually mean?

Running JavaScript means:

- JavaScript code is **read**
- Then **executed line by line**
- By a **JavaScript engine**

A JavaScript engine (like **V8**) does the real work.

In **Node.js**:
- Node takes your JavaScript
- Sends it to **V8**
- V8 executes it

That’s why we say:

> Node.js is a **JavaScript runtime environment**

---

### 2. Why Node.js is called a runtime environment

Node.js =

- **V8 JavaScript Engine**
- extra capabilities (file system, networking, OS access, etc.)

So when you write Node.js code:

- Your code is parsed by V8
- V8 executes it
- Node provides the environment around it

---

### 3. Node REPL

Node provides a special interactive mode called **REPL**.

**REPL** stands for:

- **Read** → read your JavaScript input
- **Eval** → evaluate (execute) it
- **Print** → print the result
- **Loop** → wait for the next input

Very simple meaning:

> A place where you type JavaScript and see results immediately

---

### 4. How to start Node REPL

Open your terminal and type:

    node

You will see:

    >

This means:
- Node is running
- It is waiting for JavaScript input

---

### 5. Using Node REPL

Try typing:

    2 + 3

Press Enter.

Output:

    5

What happened?

- Node read your code
- Evaluated it
- Printed the result
- Went back to waiting

---

### 6. Using variables in REPL

Type:

    let x = 10
    x * 2

Output:

    20

REPL behaves like a live JavaScript playground.

---

### 7. console.log inside REPL

Type:

    console.log("Hello Node")

Output:

    Hello Node
    undefined

Why `undefined`?

Because:
- `console.log()` **prints something**
- But it **returns nothing**
- REPL always shows the return value

This behavior is important and will matter later.

---

### 8. Why Node REPL exists

REPL is useful for:

- Quick experiments
- Testing ideas
- Debugging logic
- Learning JavaScript behavior

👉 REPL is **not used for real projects**

---

### 9. How to exit Node REPL

You can exit REPL using either:

    Ctrl + C  (press twice)

OR:

    .exit

---

### 10. Running a `.js` file

Real Node.js programs are written in files.

---

### 11. Step 1 — Create a JavaScript file

Create a file named:

    hello.js

Inside it, write:

    console.log("Hello from Node.js file");

---

### 12. Step 2 — Run the file using Node

In the same folder, run:

    node hello.js

Output:

    Hello from Node.js file

---

### 13. What actually happened behind the scenes

When you ran:

    node hello.js

Node did this:

1. Read the `hello.js` file  
2. Sent the code to V8  
3. Executed it line by line  
4. Printed the output to the terminal  

That’s all.

---

### 14. REPL vs `.js` file 

| REPL            | `.js` File           |
|-----------------|----------------------|
| Interactive     | One-time execution   |
| Temporary       | Persistent           |
| For learning    | For real programs    |
| Not saved       | Saved in a file      |

---

### 15. Key mental model to remember

- REPL = practice ground
- `.js` file = real program
- Node runs JavaScript using V8
- JavaScript is single-threaded and synchronous by default

