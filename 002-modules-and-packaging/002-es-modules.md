## ES Modules

### 1. Why ES Modules exist (context)

You already learned CommonJS.

CommonJS introduced modules to Node.js, but it had limitations:

- require() is synchronous  
- Not designed for browsers  
- Different syntax from standard JavaScript  

To unify JavaScript across environments, ES Modules (ESM) were introduced.

ES Modules are:

- Standardized  
- Native to JavaScript  
- Supported in browsers  
- Supported in Node.js  

---

### 2. What are ES Modules?

ES Modules are the official JavaScript module system.

They use two keywords:

- export  
- import  

Conceptually identical to CommonJS:

- Files are isolated  
- You explicitly export values  
- You explicitly import values  

Only syntax and loading behavior differ.

---

### 3. Critical Node.js rule

Node.js does NOT treat `.js` files as ES Modules by default.

You must explicitly enable ESM.

Two ways:

Option 1 (industry standard):

    package.json

    {
      "type": "module"
    }

Option 2:

Use `.mjs` file extensions.

Modern projects typically use Option 1.

---

### 4. Enabling ES Modules in Node.js

Add to package.json:

    {
      "type": "module"
    }

This tells Node:

“Treat all .js files as ES Modules.”

Important implication:

This setting affects the entire project.

---

### 5. Your first ES Module (export)

Create:

    math.js

    export function add(a, b) {
      return a + b;
    }

What happened?

- export exposes the function  
- Without export, it remains private  

Same idea as module.exports, different syntax.

---

### 6. Importing with import

Create:

    app.js

    import { add } from "./math.js";

    console.log(add(2, 3));

Run:

    node app.js

Output:

    5

You just used ES Modules.

---

### 7. Extremely important difference: file extensions

In ES Modules:

❌ This fails:

    import { add } from "./math";

✅ This is required:

    import { add } from "./math.js";

Why?

ES Modules follow browser-style resolution rules.

Browsers require exact file paths.

This is one of the most common beginner errors.

---

### 8. Multiple exports

    math.js

    export function add(a, b) {
      return a + b;
    }

    export function subtract(a, b) {
      return a - b;
    }

Importing:

    app.js

    import { add, subtract } from "./math.js";

    console.log(add(5, 2));
    console.log(subtract(5, 2));

---

### 9. Named exports vs default exports (critical concept)

Named export:

    export function add() {}

Default export:

    export default function add() {}

Importing named:

    import { add } from "./math.js";

Importing default:

    import add from "./math.js";

Rules:

- A module can have many named exports  
- A module can have only one default export  

---

### 10. Default export example

    math.js

    export default function add(a, b) {
      return a + b;
    }

Usage:

    import add from "./math.js";

No curly braces required.

---

### 11. Mixing named + default exports

    math.js

    export function subtract(a, b) {
      return a - b;
    }

    export default function add(a, b) {
      return a + b;
    }

Importing:

    import add, { subtract } from "./math.js";

Very common in real-world libraries.

---

### 12. Major difference from CommonJS: loading behavior

| CommonJS        | ES Modules            |
|-----------------|-----------------------|
| require()       | import                |
| synchronous     | asynchronous          |
| runtime loaded  | statically analyzed   |

Key idea:

ES Modules are statically analyzed.

This enables:

- Better optimizations  
- Tree shaking  
- Early error detection  

---

### 13. ES Modules are asynchronous

Imports are resolved before execution.

Implications:

- Better suited for browsers  
- Enables parallel loading  
- Required for modern tooling  

---

### 14. Module scope behavior

Like CommonJS:

- Each file has its own scope  
- Nothing leaks automatically  
- Only exported values are shared  

---

### 15. Module caching still exists

ES Modules are also cached.

- Executed once  
- Reused across imports  

Same shared-state behavior as CommonJS.

---

### 16. Live bindings (subtle but important)

In ES Modules, exports are live bindings.

Example:

    counter.js

    export let count = 0;

    export function increment() {
      count++;
    }

Usage:

    import { count, increment } from "./counter.js";

    console.log(count); // 0
    increment();
    console.log(count); // 1

Unlike CommonJS snapshots, ESM reflects updates.

---

### 17. Path resolution rules

ES Modules require explicit clarity.

Always include:

- Relative prefix (`./`, `../`)  
- File extension  

Correct:

    import x from "./utils.js";

Incorrect:

    import x from "utils";   // looks for package

---

### 18. __dirname / __filename differences

ES Modules do NOT provide:

- __dirname  
- __filename  

Workaround:

    import { fileURLToPath } from "url";
    import { dirname } from "path";

    const __filename = fileURLToPath(import.meta.url);
    const __dirname = dirname(__filename);

This surprises many developers transitioning from CommonJS.

---

### 19. import.meta (ESM-specific feature)

ES Modules introduce:

    import.meta.url

Useful for:

- File paths  
- Resource resolution  
- Module metadata  

---

### 20. Circular dependencies (behavior difference)

ES Modules handle circular dependencies better due to live bindings.

Still risky.

Still best avoided.

---

### 21. Mixing CommonJS and ES Modules (major gotcha)

Incorrect usage:

Inside ESM:

    const x = require("./math.js"); // ❌ Error

Inside CommonJS:

    import x from "./math.js";      // ❌ Error

Node enforces module system boundaries per file.

---

### 22. Interop rules (advanced)

Importing CommonJS into ESM:

    import pkg from "./cjs-module.js";

Default import receives module.exports.

Importing ESM into CommonJS:

Requires dynamic import:

    (async () => {
      const module = await import("./esm-module.js");
    })();

---

### 23. When ES Modules are used today

ES Modules dominate:

- Modern Node.js apps  
- Frontend projects  
- Frameworks  
- Libraries  
- Tooling ecosystems  

They represent the future default.

---

### 24. Common beginner mistakes

Forgetting file extension:

    import x from "./math"; // ❌

Incorrect default import:

    import { add } from "./math.js"; // ❌ if default export

Using require inside ESM:

    require(...) // ❌

---

### 25. Final mental model

ES Modules follow three core rules:

1. Files are isolated  
2. Only exported values are shared  
3. Imports are resolved before execution  

If you understand:

- export vs export default  
- Named vs default imports  
- File extension requirements  
- Live bindings  
- ESM vs CommonJS differences  

You understand ES Modules.
