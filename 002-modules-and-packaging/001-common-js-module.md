## Common JS Module

### 1. What problem do modules solve?

Imagine writing your entire application in a single file:

    // app.js
    function add(a, b) {
      return a + b;
    }

    function subtract(a, b) {
      return a - b;
    }

    function multiply(a, b) {
      return a * b;
    }

This approach quickly creates problems:

- The file becomes huge  
- Hard to navigate and maintain  
- No separation of responsibilities  
- Code reuse becomes painful  

We need a way to split code into multiple files and share logic cleanly.

That is exactly what modules solve.

---

### 2. What is CommonJS?

CommonJS is Node.js’s original module system.

It defines two key mechanisms:

1. How to export values from a file  
2. How to import values into another file  

These are implemented using:

- `module.exports`
- `require()`

---

### 3. Core mental model (very important)

In CommonJS:

- Each file is its own private scope  
- Nothing is shared automatically  
- You must explicitly export what you want to expose  

Think of every file as a sealed box.

Only exported values leave the box.

---

### 4. Your first CommonJS module (exporting)

Create a file:

    math.js

    function add(a, b) {
      return a + b;
    }

    module.exports = add;

What happened here?

- `add` exists only inside math.js  
- `module.exports = add` exposes it externally  

This file now exports a single function.

---

### 5. Importing with require()

Create another file:

    app.js

    const add = require("./math");

    console.log(add(2, 3));

Run:

    node app.js

Output:

    5

You just used CommonJS modules.

---

### 6. What require("./math") actually does

When Node encounters:

    require("./math")

Node performs these steps:

1. Resolves the file path (`math.js`)  
2. Executes the module once  
3. Captures `module.exports`  
4. Returns that exported value  

So:

    const add = require("./math");

means:

`add` now equals whatever math.js exported.

---

### 7. The hidden Node.js wrapper

Every CommonJS module is internally wrapped by Node:

    function wrapper(module, exports, require, __filename, __dirname) {
      // your code
    }

This explains several important behaviors:

- Variables are file-scoped  
- `module`, `exports`, and `require` are injected  
- Your code is never truly global  

---

### 8. module.exports vs exports (critical distinction)

Inside a module:

- `module.exports` is the real export object  
- `exports` is just a reference to `module.exports`

Initially:

    exports === module.exports  // true

Valid usage:

    exports.add = add;

Equivalent to:

    module.exports.add = add;

Dangerous mistake:

    exports = add;  // ❌ breaks the reference

Why this fails:

You reassigned `exports`, not `module.exports`.

Correct:

    module.exports = add;

Rule of thumb:

Use `module.exports` when exporting a single value.

---

### 9. Exporting multiple values

You are not limited to one export.

    math.js

    function add(a, b) {
      return a + b;
    }

    function subtract(a, b) {
      return a - b;
    }

    module.exports = {
      add,
      subtract
    };

Importing:

    app.js

    const math = require("./math");

    console.log(math.add(5, 2));
    console.log(math.subtract(5, 2));

---

### 10. require() is synchronous

Important design detail:

`require()` is synchronous.

Execution pauses until:

- Module is resolved  
- Module is executed  
- Exports are returned  

This works well for:

- Backend applications  
- CLI tools  
- Node servers  

Less ideal for browsers (one reason ES Modules exist).

---

### 11. Module caching (major gotcha)

Modules are executed only once.

Example:

    console.log("Module loaded");

    module.exports = {};

If required multiple times:

    require("./math");
    require("./math");

Output:

    Module loaded   // only once

Why?

Node caches modules after first execution.

Implications:

- State inside modules persists  
- Can unintentionally create shared state  

---

### 12. Shared state behavior

Example:

    counter.js

    let count = 0;

    module.exports.increment = () => ++count;

Usage:

    const counter1 = require("./counter");
    const counter2 = require("./counter");

    console.log(counter1.increment()); // 1
    console.log(counter2.increment()); // 2

Both share the same module instance.

This surprises many beginners.

---

### 13. Path resolution rules

Common mistakes:

Forgetting `./`

    require("math");   // ❌ looks for package
    require("./math"); // ✅ local file

Resolution order (simplified):

1. Core Node modules  
2. node_modules  
3. Relative paths  

---

### 14. File extensions (useful trick)

Node automatically tries:

- `.js`
- `.json`
- `.node`

So this works:

    require("./math")

Instead of:

    require("./math.js")

---

### 15. Circular dependencies (advanced gotcha)

Example:

File A requires File B  
File B requires File A  

Result:

- Partial exports  
- Undefined values  
- Confusing bugs  

Why?

Modules execute while loading.

Solution:

- Avoid tight coupling  
- Refactor shared logic into a third module  

---

### 16. __filename and __dirname

Provided by the Node wrapper:

    console.log(__filename);
    console.log(__dirname);

Useful for:

- Resolving paths  
- Reading files  
- Loading resources  

---

### 17. Common beginner mistakes

Forgetting relative paths:

    require("utils");   // ❌
    require("./utils"); // ✅

Incorrect exports usage:

    exports = something; // ❌

Expecting variables to leak across files:

Variables are private unless exported.

---

### 18. When CommonJS is used today

Still dominant in:

- Legacy Node projects  
- Many npm packages  
- Tooling ecosystems  

Coexists with ES Modules.

Understanding CommonJS is essential for Node.js mastery.

---

### 19. Final mental model

CommonJS modules follow three simple rules:

1. Every file is isolated  
2. Only exports leave the file  
3. require() loads and returns exports  

If you fully understand:

- module.exports  
- exports reference behavior  
- require() execution & caching  

You understand CommonJS.
