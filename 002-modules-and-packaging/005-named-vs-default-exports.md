## Named vs Default Exports

### 1. First, why do export styles exist?

Exports answer a very simple design question:

How many things does this module expose?

- One main thing → Default export  
- Many things → Named exports  

That is the entire reason both styles exist.

This is not syntax complexity — it is API design.

---

### 2. Named exports (multiple values)

#### Definition

Named exports allow a module to expose multiple values.

Each exported item has a specific name.

---

#### Example — Named exports

    // math.js
    export function add(a, b) {
      return a + b;
    }

    export function subtract(a, b) {
      return a - b;
    }

Importing:

    import { add, subtract } from "./math.js";

    console.log(add(5, 2));
    console.log(subtract(5, 2));

---

### 3. Key rules for named exports (very important)

#### Rule 1 — Names must match exactly

Exports:

    export function add() {}

Import:

    import { add } from "./math.js";  // ✅

    import { Add } from "./math.js";  // ❌ Error

JavaScript is case-sensitive.

---

#### Rule 2 — Curly braces are required

Named imports always use `{}`:

    import { add } from "./math.js";

Without braces:

    import add from "./math.js";  // ❌ Looks for default export

---

#### Rule 3 — Renaming is explicit

You may rename during import:

    import { add as sum } from "./math.js";

    console.log(sum(2, 3));

Important concept:

- Renaming does NOT change the original export  
- It only changes the local variable name  

---

### 4. Default exports (single main value)

#### Definition

A default export represents the primary value provided by a module.

Think of it as:

"This file mainly provides THIS thing."

---

### 5. Example — Default export

    // add.js
    export default function add(a, b) {
      return a + b;
    }

Importing:

    import add from "./add.js";

    console.log(add(2, 3));

---

### 6. Key rules for default exports

#### Rule 1 — No curly braces

Default imports do NOT use `{}`:

    import add from "./add.js";

Using braces:

    import { add } from "./add.js"; // ❌ Error unless named export exists

---

#### Rule 2 — Import name is flexible

This is valid:

    import myAddFunction from "./add.js";

Because:

Default exports do not have a fixed external name.

You choose the local identifier.

---

### 7. Only ONE default export per file

Invalid:

    export default function add() {}
    export default function subtract() {} // ❌ Error

Valid module structure:

- Many named exports ✔️  
- Only one default export ✔️  

---

### 8. Mixing named + default exports

Yes, this is completely valid.

    // math.js
    export default function multiply(a, b) {
      return a * b;
    }

    export function add(a, b) {
      return a + b;
    }

Importing:

    import multiply, { add } from "./math.js";

    console.log(multiply(2, 3));
    console.log(add(2, 3));

Important mental model:

- Default import → no braces  
- Named imports → braces  

---

### 9. Alternative export syntax (useful trick)

Instead of exporting inline:

    function add(a, b) { return a + b; }
    function subtract(a, b) { return a - b; }

    export { add, subtract };

This is equivalent.

Useful when:

- You want clean separation  
- Functions defined first, exports later  

---

### 10. CommonJS comparison (important connection)

CommonJS:

    module.exports = add;

Behaves like:

Default export

---

CommonJS:

    module.exports = { add, subtract };

Behaves like:

Named exports

But remember:

CommonJS exports objects, not true named bindings.

---

### 11. Subtle but important difference

**ES Modules**

Named exports = true named bindings

**CommonJS**

Named-style exports = object properties

Why this matters:

ES Modules support static analysis & tree shaking.

CommonJS does not.

---

### 12. Tree shaking advantage (modern tooling benefit)

Named exports enable:

- Dead code elimination  
- Smaller bundles  
- Better optimizations  

Example:

    import { add } from "./math.js";

Only `add` may be included in final bundle.

With default exports:

Tooling may include entire module.

---

### 13. When to use named exports (best practice)

Prefer named exports when:

- Module exposes multiple utilities  
- You want clarity  
- You want better auto-complete  
- You want safer refactoring  

Examples:

- math.js  
- utils.js  
- helpers.js  

Why?

Explicit APIs scale better.

---

### 14. When to use default exports

Prefer default exports when:

- Module represents one main concept  
- Component files  
- Classes  
- Single-purpose modules  

Examples:

- UserService.js  
- App.js  
- Button.js (React)  

---

### 15. Common beginner mistakes (very common)

#### Mistake 1 — Forgetting curly braces

    import add from "./math.js"; // ❌ if named export

Correct:

    import { add } from "./math.js";

---

#### Mistake 2 — Wrong names

    import { sum } from "./math.js"; // ❌ if not exported

---

#### Mistake 3 — Expecting multiple default exports

Only one allowed.

---

#### Mistake 4 — Confusing CommonJS behavior

CommonJS default-like exports behave differently in interop.

---

### 16. Major gotcha — Default export is NOT "special magic"

This:

    export default function add() {}

Is just syntax sugar for:

    export { add as default }

Understanding this clarifies many edge cases.

---

### 17. Mental shortcut for mastery

If you remember only this:

Named exports → Use `{}` → Names must match  
Default export → No `{}` → Name is flexible  

You avoid 90% of real-world mistakes.

---

### 18. Final takeaway

Export styles are about module design, not syntax preference.

Named exports:

- Explicit  
- Safer  
- Scalable  

Default exports:

- Convenient  
- Clean for single-purpose modules  

Understanding both allows you to read and write modern JavaScript professionally.
