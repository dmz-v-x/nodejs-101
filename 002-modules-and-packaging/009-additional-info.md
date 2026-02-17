## Variables and Functions Inside Functions, IIFE, and Modules  

## PART 1 — Function Scope (The Foundation)

### 1. What happens when you declare variables inside a function?

Example:

    function outer() {
      let x = 10;
    }

Key rule:

Variables declared inside a function exist **only inside that function**.

Why?

Because every function creates its own **local scope**.

Scope simply means:

"Where can this variable be accessed?"

---

### 2. Visual mental model

Think of it like:

    outer scope {
       x exists here
    }

Outside:

    global scope {
       x does NOT exist
    }

Variables stay inside their function unless explicitly exposed.

---

### 3. Example with an inner function

    function outer() {
      let x = 10;

      function inner() {
        console.log(x);
      }

      inner();
    }

    outer();

Why does this work?

Because of **lexical scope**.

---

## PART 2 — Lexical Scope (Critical Concept)

### 4. Lexical Scope = Scope determined by position

Important rule:

A function can access variables from where it was **defined**, not where it was called.

Example:

    function outer() {
      let x = 10;

      function inner() {
        console.log(x);
      }

      return inner;
    }

    const fn = outer();
    fn();

Why can `inner()` still access `x`?

Because JavaScript remembers its **creation environment**.

---

### 5. This leads to closures

Closure = Function + Preserved Scope

Meaning:

Even after `outer()` finishes execution, `inner()` still remembers `x`.

Closures are one of JavaScript’s most powerful features.

---

## PART 3 — IIFE (Immediately Invoked Function Expression)

### 6. Why IIFEs existed historically

Before modules:

All scripts shared the global scope.

Problems:

- Variable collisions  
- Naming conflicts  
- Hard-to-debug bugs  

Developers needed private scopes.

Solution → IIFE.

---

### 7. What is an IIFE?

Definition:

A function that is:

1. Defined  
2. Immediately executed  

Syntax:

    (function () {
      console.log("Runs immediately");
    })();

---

### 8. Why parentheses are required (Major Gotcha)

This fails:

    function () {}(); // Syntax Error

Correct:

    (function () {})();

Parentheses force JavaScript to treat the function as an **expression**.

---

### 9. Scope isolation using IIFE

    (function () {
      let secret = 123;
    })();

    console.log(secret); // Error

IIFE creates a private scope.

Exactly like a normal function.

---

### 10. Variable shadowing behavior

    let count = 0;

    (function () {
      let count = 100;
      console.log(count);
    })();

    console.log(count);

Two separate variables.

No conflict.

---

## PART 4 — Anonymous Functions

### 11. What is an anonymous function?

Definition:

A function without a name.

Example:

    function () {}

Used commonly in:

    setTimeout(function () {
      console.log("Hello");
    }, 1000);

Why anonymous?

Because we don’t need to reuse it later.

---

### 12. Important distinction

Anonymous function:

    function () {}

Arrow function:

    () => {}

They behave differently, especially with `this`.

---

## PART 5 — Modules (Modern Evolution)

### 13. Conceptual similarity to IIFE

Modules solve the same problem:

Scope isolation.

Mental model:

Module = File with private scope

---

### 14. Example ES Module

    // math.js
    const PI = 3.14;

    export { PI };

Other files cannot access `PI` unless imported.

---

### 15. Key difference from IIFE

IIFE:

- Pattern / trick  

Modules:

- Language feature  
- Native behavior  
- Tooling support  

---

## PART 6 — Node.js Modules (Hidden Wrapper)

### 16. CommonJS internal behavior (Very Important)

In Node.js (CommonJS):

Your file is secretly wrapped.

You write:

    console.log(module);
    console.log(require);

Node internally transforms:

    (function (exports, require, module, __filename, __dirname) {

      console.log(module);
      console.log(require);

    });

---

### 17. Why this wrapper exists

Provides:

- Scope isolation  
- Module system support  
- Special variables  

Without it:

Global chaos.

---

### 18. Major gotcha

Your file is NOT global.

Top-level variables are module-scoped.

---

### 19. Why top-level `this` is `{}`

Because code runs inside a function.

Not global scope.

---

## PART 7 — require() Deep Dive

### 20. What happens when require() is called?

Example:

    const path = require("path");

Node performs several steps.

---

### Step 1 — Cache check

If module already loaded:

- Return cached exports  
- Skip execution  

Performance optimization.

---

### Step 2 — Module resolution

Node determines:

Is this:

- Core module?
- File?
- Package?

---

### Resolution rules simplified

"./something" → Relative file  
"/absolute/path" → Exact file  
"express" → Package lookup

---

### Package lookup behavior

Node walks upward:

./node_modules  
../node_modules  
../../node_modules  

First match wins.

---

### 21. File extension resolution

If extension missing:

Node tries:

- .js  
- .json  
- .node  

---

### 22. Loading behavior by type

.js → Read → Wrap → Execute  
.json → Read → JSON.parse  
.node → Native binary load  
Core modules → Internal binding

---

### 23. Module execution lifecycle

1. Read file  
2. Wrap in function  
3. Execute  
4. Collect exports  
5. Cache result  

---

### 24. Critical gotcha

Modules execute only once.

Due to caching.

---

### 25. Shared state behavior

All imports share the same module instance.

Common beginner confusion.

---

## PART 8 — Closures + Modules

Closures preserve scope.

Modules preserve state via caching.

Together enable:

- Singletons  
- Private state  
- Encapsulation  

---

## PART 9 — Advanced Gotchas

### 26. Circular dependencies

A requires B  
B requires A  

Result:

- Partial exports  
- Undefined values  

---

### 27. Mutating exports (CommonJS)

Allowed.

Because exports are normal objects.

---

### 28. ES Modules difference

Exports are live bindings.

Cannot be reassigned externally.

---

### 29. Sync vs Async loading

CommonJS → Synchronous  
ES Modules → Asynchronous  

Explains interop rules.

---

# FINAL MENTAL MODEL

### Functions

- Create scope  
- Variables stay local  
- Enable closures  

---

### IIFE

- Manual scope isolation  
- Historical module pattern  

---

### Modules

- Native scope isolation  
- Controlled exports/imports  

---

### CommonJS (Node)

- Wrapped in hidden function  
- require() loader  
- Cached execution  

---

### require()

Resolve → Load → Wrap → Execute → Cache


