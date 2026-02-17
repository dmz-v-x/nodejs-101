## Differences & Interop Rules

### 1. First, the big picture

Node.js supports two module systems:

CommonJS (CJS)  
→ Uses `require()` and `module.exports`

ES Modules (ESM)  
→ Uses `import` and `export`

Both systems coexist in modern Node.js.

The natural question becomes:

How do they interact with each other?

Understanding this eliminates many confusing errors.

---

### 2. Core difference (mental model)

**CommonJS (CJS)**

- Node-specific (historically)
- Modules loaded at runtime
- Synchronous loading
- Flexible resolution rules
- More forgiving, sometimes messy

**ES Modules (ESM)**

- Official JavaScript standard
- Statically analyzed before execution
- Asynchronous loading model
- Strict resolution rules
- Predictable behavior

Simple intuition:

CommonJS = older, flexible  
ES Modules = modern, strict

---

### 3. Default behavior in Node.js (critical rule)

Node decides the module system **per file**.

| Condition                    | Module System |
|------------------------------|---------------|
| `.js` + `"type": "commonjs"` | CommonJS      |
| `.js` + `"type": "module"`   | ES Modules    |
| `.cjs`                        | Always CJS    |
| `.mjs`                        | Always ESM    |

Important implications:

- Module type is not global magic  
- File extension + package.json control behavior  

---

### 4. How Node determines the system

**Case 1 — No package.json**

`.js` files default to:

➡️ CommonJS

---

**Case 2 — package.json present**

    {
      "type": "module"
    }

➡️ All `.js` files become ES Modules

---

**Case 3 — Explicit extensions**

`.cjs` → Always CommonJS  
`.mjs` → Always ES Modules

These override package.json.

---

### 5. Can ES Modules import CommonJS?

Yes — mostly.

Example:

**math.cjs**

    module.exports = function add(a, b) {
      return a + b;
    };

**app.js (ES Module)**

    import add from "./math.cjs";

    console.log(add(2, 3));

This works.

Key rule:

CommonJS exports appear as a **default export** in ES Modules.

Mental model:

    module.exports  →  default export

---

### 6. Importing named values from CommonJS (subtle behavior)

Example:

**math.cjs**

    module.exports = {
      add: (a, b) => a + b,
      subtract: (a, b) => a - b
    };

**ES Module**

    import math from "./math.cjs";

    console.log(math.add(2, 3));

Why default import?

Because CommonJS exports a single object.

Node cannot statically analyze property names reliably.

---

### 7. Can CommonJS import ES Modules?

Not directly.

This fails:

**app.cjs**

    const add = require("./math.js"); // ❌ Error

Why?

Because:

- ES Modules load asynchronously  
- require() is synchronous  

Node refuses this mismatch.

---

### 8. Correct way: dynamic import()

CommonJS must use:

    (async () => {
      const module = await import("./math.js");
      console.log(module.add(2, 3));
    })();

Important concept:

- import() works everywhere  
- Returns a promise  

---

### 9. Why this restriction exists

CommonJS:

➡️ Runtime execution  
➡️ Synchronous loader

ES Modules:

➡️ Pre-execution analysis  
➡️ Async loader

Mixing sync + async loaders directly is unsafe.

Hence Node’s enforcement.

---

### 10. Resolution differences that cause confusion

**CommonJS**

- Extension optional  
- Flexible lookup rules  

    require("./math")

---

**ES Modules**

- Extension mandatory  
- Exact file paths required  

    import "./math.js"

Failure to remember this causes many errors.

---

### 11. Interop default export behavior (major gotcha)

CommonJS:

    module.exports = something;

ES Modules:

    import something from "./module.cjs";

But ES Modules:

    export default something;

CommonJS:

    require("./module.js") // gets default wrapped differently

Mismatch here causes confusion.

---

### 12. Named exports vs CommonJS properties

ES Modules have true named exports:

    export function add() {}

CommonJS exports object properties:

    module.exports = { add }

They are not identical concepts.

Interop requires translation by Node.

---

### 13. When to use CommonJS (practical rules)

CommonJS is still useful for:

- Legacy Node.js projects  
- Many config files  
- Tooling ecosystems  

Examples:

- webpack.config.js  
- jest.config.js  
- Older libraries  

Why?

Because some tools still expect CJS.

---

### 14. When to use ES Modules (recommended)

Prefer ES Modules when:

- Starting new projects  
- Writing modern applications  
- Building reusable libraries  
- Working with frontend/backend code  

ES Modules represent the future direction of JavaScript.

---

### 15. Real-world strategy (professional mindset)

Modern Node projects typically:

- Use ES Modules for application code  
- Use CommonJS only when required by tooling  

Hybrid ecosystems are normal.

Blindly forcing one system everywhere is not always practical.

---

### 16. Common beginner mistakes (now prevented)

Mixing require & import in the same file:

    import x from "y";
    const z = require("z"); // ❌ Avoid

Forgetting `"type": "module"`

Forgetting `.js` extensions in ESM

Assuming CJS and ESM behave identically

Misunderstanding default export behavior

---

### 17. Key mental model for interop

**ESM → CJS**

- Use dynamic import()

**CJS → ESM**

- Treated as default export

**Important truth**

Node acts as a translator between systems.

But translation is not perfect symmetry.

---

### 18. Final takeaway

Node.js supports both:

CommonJS = older, flexible  
ES Modules = modern, strict  

Understanding their interaction explains:

Most mysterious module-related errors:

- "Cannot use import statement outside a module"
- "require is not defined"
- "Unexpected token export"

These are not random problems.

They are module system mismatches.
