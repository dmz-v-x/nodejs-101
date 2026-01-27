## Global Objects in JavaScript — window vs global vs globalThis vs this

### 1. JavaScript runs in different environments

JavaScript is **one language**, but it can run in **different environments**:

- Browser (Chrome, Firefox, Edge)
- Node.js (server-side JavaScript)
- Others (Web Workers, Deno, etc.)

Each environment provides its own **global object**  
(the root object where global variables and APIs live).

---

### 2. Global objects by environment (big picture)

- **Browser** → global object is `window`
- **Node.js** → global object is `global`
- **Modern JavaScript (standard)** → `globalThis`
- **`this`** → not an environment object, but a **context-dependent keyword**

This distinction is extremely important.

---

### 3. Quick classification: browser vs Node.js

| Name         | Browser?                     | Node.js?             | What is it?                                |
|--------------|------------------------------|----------------------|--------------------------------------------|
| `window`     | ✅ Yes (main browser context) | ❌ No                | Browser global object (Window)              |
| `global`     | ❌ No                        | ✅ Yes               | Node.js global object                       |
| `globalThis` | ✅ Yes (modern browsers)     | ✅ Yes (Node 12+)     | Standard cross-platform global object name  |
| `this`       | ✅ Yes                       | ✅ Yes               | Context-dependent keyword                  |

Browser-only: `window`  
Node-only: `global`  
Both: `globalThis`, `this`

---

### 4. window (browser feature)

#### What is `window`?

In a browser (normal `<script>` — not a module):

- `window` is the **global object**
- It also represents the browser tab / window environment

So these are true in a normal browser script:

    console.log(window === this);        // true
    console.log(window === globalThis);  // true

---

#### What lives on `window`?

**Global variables (var only, non-module):**

    var x = 10;
    console.log(window.x); // 10

**Browser APIs:**

    window.document
    window.alert
    window.setTimeout
    window.localStorage

**Custom properties:**

    window.myAppName = "Cool App";
    console.log(myAppName); // "Cool App"

---

#### When to use `window`?

- Accessing browser APIs:

    window.alert("Hi");   // same as alert("Hi")

- As a deliberate namespace:

    window.myUtils = {
      sum(a, b) {
        return a + b;
      }
    };

---

#### When NOT to use `window`?

- In Node.js (it does not exist)
- In browser modules (`<script type="module">`)
- For random global storage (causes global scope pollution)

---

### 5. global (Node.js feature)

#### What is `global`?

In Node.js:

- `global` is the **global object**
- Similar in concept to `window` in browsers

Example:

    global.foo = 42;
    console.log(global.foo); // 42

---

#### Important difference from browsers

In Node.js, **top-level variables are NOT automatically added to global**.

    var x = 10;

    console.log(x);        // 10
    console.log(global.x); // undefined

Why?

Because:
- Each file in Node.js is its **own module**
- It does not run in the true global scope

---

#### When to use `global`?

- Rare, intentional cases:

    global.config = { dbUrl: "..." };

- Debug flags (occasionally):

    global.debugMode = true;

---

#### When NOT to use `global`?

- In real applications
- Prefer:
  - `module.exports`
  - `require` / `import`
  - Dependency injection

---

### 6. globalThis (standard, works everywhere)

#### Why was `globalThis` introduced?

Because earlier:

- Browser → `window`
- Node → `global`
- Worker → `self`

This made universal code difficult.

So JavaScript standardized:

👉 **`globalThis`**

---

#### What is `globalThis`?

- A **single, environment-independent name**
- Always points to the global object

Works in:
- Browser
- Node.js
- Web Workers
- Deno (and others)

---

#### Examples

Browser:

    console.log(globalThis === window); // true

Node.js:

    console.log(globalThis === global); // true

Attach something:

    globalThis.appName = "My Awesome App";
    console.log(globalThis.appName);

Works everywhere.

---

#### When to use `globalThis`?

- Writing cross-platform libraries
- Code that runs in both browser and Node
- When you explicitly need the global object

---

#### When NOT to use it?

- If targeting only one environment
- When environment-specific APIs are clearer
- In very old environments (rare now)

---

### 7. this (special keyword — NOT a global object)

`this` is **not** a global object.

It is a **keyword** whose value depends on:
- How the function is called
- Where the code runs
- Whether strict mode is enabled

---

### 8. this at the top level

#### Browser (non-module `<script>`)

    console.log(this === window); // true

---

#### Node.js (CommonJS module)

    console.log(this); // {}

Here:
- `this === module.exports`
- NOT the global object

---

#### ES Modules (browser or Node)

    console.log(this); // undefined

By specification:
- Top-level `this` is `undefined` in ES modules

---

### 9. this inside functions

#### Non-strict mode

    function show() {
      console.log(this);
    }

    show();

- Browser → `window`
- Node (older, non-strict) → `global`

---

#### Strict mode

    "use strict";
    function show() {
      console.log(this);
    }

    show(); // undefined

---

#### As an object method

    const person = {
      name: "Himanshu",
      greet() {
        console.log(this.name);
      }
    };

    person.greet(); // "Himanshu"

Here:
- `this` is the object before the dot

---

#### call / apply / bind

    function greet() {
      console.log(this.name);
    }

    const user = { name: "Himanshu" };

    greet.call(user);
    greet.apply(user);

    const bound = greet.bind(user);
    bound();

---

### 10. this in arrow functions

Arrow functions:
- Do NOT have their own `this`
- Capture `this` from the surrounding scope

      const obj = {
        name: "Himanshu",
        regular() {
          const arrow = () => {
            console.log(this.name);
          };
          arrow();
        }
      };
  
      obj.regular(); // "Himanshu"

---

### 11. this in constructors and classes

#### Constructor function

    function Person(name) {
      this.name = name;
    }

    const p = new Person("Himanshu");
    console.log(p.name);

---

#### Class syntax

    class Person {
      constructor(name) {
        this.name = name;
      }
      greet() {
        console.log("Hi, I'm " + this.name);
      }
    }

    const p2 = new Person("Himanshu");
    p2.greet();

---

### 12. Why `this` behaves differently in browser vs VS Code

#### In the browser

Your script runs in the **global scope**:

    console.log(this); // window

Because:
- Browser global object = `window`
- Top-level `this === window`

---

#### In VS Code

Node.js wraps every file like this:

    (function (exports, require, module, __filename, __dirname) {
      // your file code
    })();

So at top level:

    this === module.exports

And since `module.exports` starts as `{}`:

    console.log(this); // {}

---

### 13. Final summary 

- `window` → browser global object
- `global` → Node.js global object
- `globalThis` → standard global object everywhere
- `this` → context-dependent keyword
- Node.js does NOT run files in the true global scope
- Browser scripts (non-module) do

Understanding this clears **one of the biggest JavaScript confusions** for good.
