## Common JS vs ES Modules

### 1. What is the entry point file in Node.js?

In Node.js, the entry point file is simply the first JavaScript file that Node starts executing when you run your application.

There is no fixed entry point enforced by Node.

You decide it.

---

### 2. Choosing the entry point (direct execution)

When running from the terminal:

    node app.js

➡️ `app.js` becomes the entry point.

    node server.js

➡️ `server.js` becomes the entry point.

Whatever file you pass to `node` is the starting file.

---

### 3. Choosing the entry point via package.json

If your project has a `package.json`, you may define:

    {
      "name": "my-app",
      "version": "1.0.0",
      "main": "index.js"
    }

Important clarification:

The `main` field **does NOT define what runs when you execute `node`**.

Instead, it defines:

➡️ The entry point when your package is required/imported by another module.

Example:

    const myApp = require("my-app");

Node loads whatever `main` points to.

---

### 4. Why "main" does not automatically run your app

Running:

    node .

Will only work if Node can resolve a runnable file.

Common expectations:

❌ `"main"` alone is not enough.

You typically also need:

**Option 1 — bin field (CLI usage)**

    {
      "bin": {
        "my-app": "index.js"
      }
    }

**Option 2 — npm start script**

    {
      "scripts": {
        "start": "node index.js"
      }
    }

Scripts define how your application starts.

---

### 5. What is a module in Node.js?

Simple definition:

A module is a file whose code is isolated from other files unless you explicitly export something.

In Node.js:

Every `.js` file is automatically treated as a module.

---

### 6. Why modules exist

Modules solve critical problems:

- Avoid giant files  
- Enable organization  
- Improve reusability  
- Provide privacy  
- Prevent global variable conflicts  

Example separation:

- Server logic  
- Database logic  
- Utility functions  

Each lives in its own file.

---

### 7. Module isolation (very important)

Each module has its own private scope.

Example:

    // a.js
    const secret = 42;

In another file:

    // b.js
    console.log(secret); // ❌ Error

Variables do not leak across modules.

Only exported values are visible.

---

### 8. How Node modules work internally

Node wraps every file like this:

    (function(exports, require, module, __filename, __dirname) {
      // Your code
    })();

Because of this wrapper:

- Variables remain private  
- Special objects are injected  

Available inside every module:

- exports  
- require  
- module  
- __filename  
- __dirname  

---

### 9. Types of modules in Node.js

Node supports two module systems:

| Module System        | Uses                 | File Extensions              | Import Syntax   |
|----------------------|----------------------|------------------------------|-----------------|
| CommonJS (CJS)       | Default in Node      | `.js`                        | require()       |
| ES Modules (ESM)     | Modern standard      | `.mjs` or `"type": "module"` | import/export   |

CommonJS = older Node-native system  
ES Modules = official JavaScript standard

---

### 10. How modules communicate (CommonJS)

**math.js**

    function add(a, b) {
      return a + b;
    }

    module.exports = add;

**main.js**

    const add = require("./math");
    console.log(add(2, 3)); // 5

---

### 11. How modules communicate (ES Modules)

**math.mjs**

    export function add(a, b) {
      return a + b;
    }

**main.mjs**

    import { add } from "./math.mjs";
    console.log(add(2, 3)); // 5

---

### 12. Are modules a JavaScript or Node.js feature?

Originally:

JavaScript had **no built-in module system**.

Different environments created their own:

| Environment | Module System |
|-------------|---------------|
| Node.js     | CommonJS      |
| Browsers    | None → later ES Modules |
| Others      | AMD / UMD     |

---

### 13. Today’s reality

Modules are now part of JavaScript.

Official system:

ES Modules (ES6 / 2015)

Timeline:

| Time              | Module Support |
|-------------------|----------------|
| Before ES6        | CommonJS only  |
| ES6 onward        | ES Modules     |

Node’s CommonJS was Node-specific.

ES Modules are language-standard.

---

### 14. Is require() part of JavaScript?

No.

require() is **NOT a JavaScript language feature**.

It was introduced by Node.js.

That is why:

- Works in Node ✔️  
- Fails in browsers ❌  

---

### 15. JavaScript’s official module system

ES Modules (ESM)

Syntax:

    import { something } from "./file.js";
    export function something() {}

Supported in:

- Browsers ✔️  
- Node.js (ESM mode) ✔️  

---

### 16. Exporting multiple values (CommonJS)

**math.js**

    function add(a, b) {
      return a + b;
    }

    function subtract(a, b) {
      return a - b;
    }

    const PI = 3.14;

    module.exports = {
      add,
      subtract,
      PI
    };

Importing:

    const { add, subtract, PI } = require("./math.js");

Destructuring works because require() returns the exported object.

---

### 17. Exporting multiple values (ES Modules)

Named exports:

    export function add() {}
    export function subtract() {}
    export const PI = 3.14;

OR:

    export { add, subtract, PI };

Importing:

    import { add, subtract, PI } from "./math.js";

Names must match exactly.

---

### 18. Default exports (ES Modules)

A module can have only one default export.

    export default function multiply() {}

Import:

    import multiply from "./math.js";

Default import name is flexible.

---

### 19. Mutability difference (major concept)

| System      | Export Behavior |
|-------------|-----------------|
| CommonJS    | Mutable object |
| ES Modules  | Read-only bindings |

CommonJS exports are normal objects.

ES Module exports are live read-only bindings.

---

### 20. Why require() needs module.exports

Modules protect their internal variables.

Nothing leaks automatically.

To share values:

➡️ You must export explicitly.

---

### 21. CommonJS characteristics

- Uses require()  
- Uses module.exports  
- Synchronous loading  
- Non-strict mode by default  
- Extension optional (`./math`)  

---

### 22. ES Modules characteristics

Enabled via:

    {
      "type": "module"
    }

Defaults:

- Asynchronous loading  
- Strict mode  
- Extension required  
- Uses import/export  

---

### 23. module.exports is just an object

Important internal truth:

    module.exports = {}

You can attach properties:

    module.exports.add = add;
    module.exports.PI = PI;

Equivalent to exporting an object.

---

### 24. Index.js pattern (very common)

Folder structure:

    utils/
      index.js
      math.js
      string.js

**utils/index.js**

    const math = require("./math");
    const string = require("./string");

    module.exports = { math, string };

Usage:

    const utils = require("./utils");

Node automatically resolves:

➡️ `utils/index.js`

Benefits:

- Clean imports  
- Centralized exports  
- Better organization  

---

### 25. Final mental model

Node.js modules obey simple rules:

1. Every file is a module  
2. Modules are isolated  
3. Only exports are shared  
4. require() loads synchronously  
5. import/export follow ESM rules  

Understanding this explains:

Most mysterious Node.js import/export errors.
