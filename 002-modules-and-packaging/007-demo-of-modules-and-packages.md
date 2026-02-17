## Demo of Modules & Packages

### 1. Big picture: What are we building?

Goal:

Create your own reusable package and use it exactly like a real npm library.

Instead of:

    import { add } from "./utils.js";

You will be able to write:

    import { add } from "my-utils";

This is how real-world libraries work.

---

### 2. Step 1 — Create your library folder

Create a new folder outside your application:

    my-utils

Move into it:

    cd my-utils

This folder will become your package.

---

### 3. Step 2 — Initialize as an npm package

Run:

    npm init -y

Now your folder officially becomes a Node package.

You now have:

    package.json

This file defines how Node and npm treat your library.

---

### 4. Step 3 — Decide the module system (important)

Open package.json and add:

    {
      "type": "module"
    }

Meaning:

- All `.js` files are ES Modules  
- You must use `import / export`  
- `require()` will no longer work  

---

### 5. Major gotcha — Module system mismatch

If your consumer app uses ES Modules:

    "type": "module"

Your package should also use ES Modules.

Mixing systems incorrectly causes errors like:

- "Cannot use import statement outside a module"
- "require is not defined"

Always align module systems.

---

### 6. Step 4 — Declare your package entry point

Update package.json:

    {
      "name": "my-utils",
      "version": "1.0.0",
      "type": "module",
      "exports": "./index.js"
    }

Meaning:

“When someone imports my-utils, load index.js.”

Important:

Modern packages use `exports`, not `main`.

---

### 7. Gotcha — exports vs main

If you define:

    "exports"

Node ignores:

    "main"

exports is stricter and preferred.

---

### 8. Step 5 — Create the entry file

Create:

    index.js

Example:

    export function add(a, b) {
      return a + b;
    }

    export function multiply(a, b) {
      return a * b;
    }

This file becomes your public API.

---

### 9. Critical design insight

index.js is NOT required by Node.

It is simply a convention.

You may name it anything — but exports must match.

---

### 10. Step 6 — Create a consumer application

Create another folder:

    my-app

Initialize:

    cd my-app
    npm init -y

Enable ES Modules:

    {
      "type": "module"
    }

---

### 11. How does Node find your package?

Your package is not published.

Node cannot magically discover it.

You must connect it manually.

---

### 12. Method 1 — npm link (development mode)

Inside **my-utils**:

    npm link

What happens:

- Registers your package globally  
- Creates a symbolic link  

Inside **my-app**:

    npm link my-utils

Now:

    node_modules/my-utils → symlink → local folder

---

### 13. What npm link really does (important)

npm link:

- Creates a symlink (not a copy)  
- Live connection to your library  
- Changes reflect instantly  
- No reinstall needed  

Perfect for:

- Active development  
- Local libraries  
- Monorepos  

---

### 14. Step 7 — Use your package

Inside my-app:

    app.js

    import { add, multiply } from "my-utils";

    console.log(add(2, 3));
    console.log(multiply(2, 3));

Run:

    node app.js

You are now using your own package.

---

### 15. Major gotcha — exports restrictions

If you define:

    "exports": "./index.js"

This fails:

    import x from "my-utils/internal.js"; // ❌ Error

exports blocks internal file access.

This is intentional.

---

### 16. Why this restriction is GOOD

Without exports:

Consumers can depend on internal files.

Refactoring becomes dangerous.

exports enforces a clean public API.

---

### 17. Method 2 — npm pack (production-like simulation)

Inside **my-utils**:

    npm pack

Generates:

    my-utils-1.0.0.tgz

Inside **my-app**:

    npm install ../my-utils/my-utils-1.0.0.tgz

Now:

- Package is copied (not linked)  
- No live updates  
- Behaves like real npm install  

---

### 18. Difference between link vs pack

| Feature        | npm link | npm pack |
|---------------|----------|----------|
| Live updates  | ✔️ Yes   | ❌ No    |
| File copy     | ❌ No    | ✔️ Yes   |
| Dev workflow  | ✔️ Ideal | Good for testing |

---

### 19. When to use each method

npm link  
→ Active development

npm pack  
→ Test before publishing

npm publish  
→ Public release

---

### 20. Professional insight (very important)

Monorepos (pnpm, yarn workspaces, nx, turbo):

Are automated versions of this exact linking mechanism.

You just learned the core concept manually.

---

### 21. Common beginner mistakes (critical section)

Forgetting `"exports"`  
Expecting Node to auto-detect packages  
Module system mismatch  
Importing blocked internal files  
Forgetting `"type": "module"`  
Version confusion  

---

### 22. Gotcha — Package name conflicts

If you name your package:

    "name": "lodash"

Node may resolve the real lodash instead.

Use unique names.

---

### 23. Gotcha — Cached modules

Linked packages are cached like normal modules.

If behavior seems inconsistent:

Restart Node.

---

### 24. Gotcha — Dependency handling

Your package may need dependencies.

Install them inside **my-utils**, not my-app.

Each package manages its own dependencies.

---

### 25. Final mental model

Creating a package involves:

1. Folder → npm init  
2. package.json → rules  
3. exports → entry point  
4. npm link / pack → connection  
5. import by package name  

No magic.

Only structured resolution.

---

### 26. Final takeaway

You now understand:

- How npm packages work internally  
- How Node resolves libraries  
- How local development linking works  
- How publishing simulation works  

This is the foundation of real-world library development.
