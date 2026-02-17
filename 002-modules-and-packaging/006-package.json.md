## `package.json`

### 1. Why entry points matter

When working with modules, you often write imports like:

    import { add } from "./math.js";

Here, the file path is explicit.

But in real-world applications, you frequently write:

    import express from "express";

Question:

How does Node know which file inside the `express` package to load?

The answer lies in `package.json`.

---

### 2. Big picture mental model

When you import a package by name:

    import something from "some-package";

Node follows a predictable process:

1. Finds the package folder inside `node_modules`  
2. Reads the package’s `package.json`  
3. Determines the entry point file  
4. Loads that file  

This decision is controlled entirely by specific fields.

---

### 3. The classic mechanism: `main`

#### What is `main`?

    {
      "main": "index.js"
    }

Meaning:

“When someone imports this package, start execution here.”

---

#### Example package structure

    my-lib/
    ├── package.json
    ├── index.js
    └── utils.js

    {
      "name": "my-lib",
      "main": "index.js"
    }

Now:

    const lib = require("my-lib");

Node loads:

    my-lib/index.js

---

#### Important notes about `main`

- Oldest entry point mechanism  
- Designed for CommonJS  
- Only one entry file  
- Still widely supported  

This explains why many packages expose `index.js`.

---

### 4. What happens if `main` is missing?

If `main` is not defined, Node falls back to:

    index.js

Inside the package root.

This implicit behavior became a long-standing convention.

---

### 5. The `type` field (module system switch)

#### What is `type`?

    {
      "type": "module"
    }

Meaning:

“Treat `.js` files in this package as ES Modules.”

If omitted:

Default behavior:

➡️ CommonJS

---

#### Critical clarification

The `type` field:

- Does NOT choose the entry point  
- Only affects how files are interpreted  

It controls syntax rules:

- Whether `import/export` works  
- Whether `require()` works  

---

### 6. Why `type` matters

It influences:

- How Node interprets `main`  
- How consumers must import the package  
- Which syntax is valid  

Example confusion:

Incorrect assumption:

“Setting `type` defines the entry file.”

Correct understanding:

`type` defines the module system.

---

### 7. The modern mechanism: `exports`

#### What is `exports`?

The `exports` field explicitly defines what the package exposes.

Example:

    {
      "exports": "./index.js"
    }

Meaning:

“Only this file is publicly accessible.”

---

### 8. Why `exports` was introduced

Before `exports`, consumers could do:

    import secret from "my-lib/internal/secret.js";

This breaks encapsulation.

Packages unintentionally exposed internal files.

---

### 9. How `exports` fixes this

With:

    {
      "exports": "./index.js"
    }

Anything not listed becomes:

- Private  
- Inaccessible  
- Protected from direct import  

This is crucial for library authors.

---

### 10. Priority rules: `exports` vs `main`

Very important rule:

If `exports` exists, Node ignores `main`.

Resolution order:

1. exports  
2. main  
3. Fallback to index.js  

Modern packages increasingly rely on `exports`.

---

### 11. Advanced feature: conditional exports

Example:

    {
      "exports": {
        "import": "./index.mjs",
        "require": "./index.cjs"
      }
    }

Meaning:

- ES Module users get one file  
- CommonJS users get another  

This enables dual compatibility.

---

### 12. Subtle but important effect of `exports`

`exports` can:

- Restrict file access  
- Create multiple entry points  
- Define subpath imports  

Example:

    {
      "exports": {
        ".": "./index.js",
        "./utils": "./utils.js"
      }
    }

Valid imports:

    import pkg from "my-lib";
    import utils from "my-lib/utils";

Everything else is blocked.

---

### 13. Common gotcha: why imports suddenly fail

Developers often encounter:

“Cannot find module”

Even when the file exists.

Reason:

It is not listed in `exports`.

Node enforces strict boundaries.

---

### 14. The `bin` field (CLI entry points)

The `bin` field serves a different purpose.

#### What is `bin`?

It defines executable commands.

Example:

    {
      "bin": {
        "mytool": "./cli.js"
      }
    }

After installation:

A terminal command becomes available:

    mytool

---

### 15. Example CLI file

    #!/usr/bin/env node

    console.log("Hello from CLI");

Users can run:

    npx mytool

This is how tools like:

- vite  
- eslint  
- create-react-app  

operate.

---

### 16. How Node processes package entry points

When importing a package:

    import pkg from "my-lib";

Node:

1. Finds `node_modules/my-lib`  
2. Reads `package.json`  
3. Checks `exports`  
4. Else checks `main`  
5. Applies `type` rules  
6. Loads the resolved file  

Everything is deterministic.

---

### 17. Common beginner mistakes

Thinking `main` is required  
Confusing `type` with `exports`  
Accidentally exposing internal files  
Misunderstanding conditional exports  
Not understanding why valid paths fail  

---

### 18. Key mental model for mastery

`main`  
→ Legacy entry point

`type`  
→ Module system selector

`exports`  
→ Public API controller

`bin`  
→ CLI command definition

Each field solves a different problem.

---

### 19. Final takeaway

Package entry points are not magic.

They are governed by `package.json`.

Understanding:

- main  
- type  
- exports  
- bin  

Explains:

Most mysterious package import errors in Node.js.
