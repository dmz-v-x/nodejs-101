## Module Resolution in Node

### 1. What is a “module” in Node.js?

In Node.js:

Every file is a module.

That’s the entire concept.

Examples:

- index.js → one module  
- utils.js → another module  
- node_modules/chalk/index.js → another module  

Each module:

- Has its own private scope  
- Can export values  
- Can import values  

Nothing is shared automatically.

---

### 2. Why module resolution exists

When you write:

    require("./utils");

Node must determine:

- Where is "utils"?  
- Which file exactly?  
- Is it a file or folder?  
- Which extension?  

This lookup process is called:

Module Resolution

---

### 3. Two major categories of imports

Node resolves modules differently based on how the path is written.

Two main types:

1. Relative / absolute paths  
2. Package names  

These follow completely different resolution rules.

---

### 4. Relative paths (./ and ../)

Example:

    require("./math");

Meaning:

“Look relative to the current file.”

If the current file is:

    src/index.js

Node searches in this order:

1. src/math.js  
2. src/math.json  
3. src/math.node  
4. src/math/index.js  

First match wins.

---

### 5. Why "./" is critically important

Compare:

    require("math");   // ❌ package lookup
    require("./math"); // ✅ file lookup

Without `./`, Node assumes:

“This is a package inside node_modules.”

This is one of the biggest beginner mistakes.

---

### 6. Using "../" (move up directories)

Example:

    require("../utils/helper");

Meaning:

- Go up one directory  
- Then locate utils/helper  

This behaves exactly like filesystem navigation.

---

### 7. File vs directory resolution

If you require a directory:

    require("./utils");

Node attempts:

1. utils/package.json  
2. utils/index.js  

---

### 8. Directory resolution via package.json

Inside:

    utils/package.json

    {
      "main": "helper.js"
    }

Node loads:

    utils/helper.js

If `main` is missing:

Node defaults to:

    utils/index.js

---

### 9. Supported file extensions (CommonJS)

Node automatically tries:

1. .js  
2. .json  
3. .node  

Example:

    require("./config");

Node may load:

- config.js  
- config.json  
- config.node  

---

### 10. Absolute paths (rare but valid)

Example:

    require("/Users/name/project/utils.js");

Node loads that exact file.

Drawbacks:

- Not portable  
- Breaks across machines  
- Avoid in real projects  

---

### 11. Package imports (no "./")

Example:

    require("chalk");

Meaning:

“This is a package.”

Now Node switches to:

node_modules resolution

---

### 12. How Node searches for packages (very important)

When you write:

    require("chalk");

Node searches upward:

1. ./node_modules/chalk  
2. ../node_modules/chalk  
3. ../../node_modules/chalk  
4. Continues until filesystem root  

This explains why:

- One node_modules folder is enough  
- Deep files still find packages  
- Monorepos function correctly  

---

### 13. Why Node walks upward

Because Node:

- Starts from the current file’s directory  
- Moves upward until node_modules is found  
- Stops at first match  

This avoids duplication of dependencies.

---

### 14. Inside a package folder

Once Node finds:

    node_modules/chalk/

It checks:

1. package.json  

        {
          "main": "index.js"
        }

2. If no main → index.js  

---

### 15. Modern package entry points (advanced)

Modern packages may define:

    {
      "main": "index.js",
      "exports": {
        ".": "./index.js"
      }
    }

The `exports` field:

- Restricts accessible files  
- Controls public API  
- Improves encapsulation  

---

### 16. require() vs import resolution differences

CommonJS:

- Extension optional  
- Flexible resolution  

ES Modules:

- Extension mandatory  
- Strict resolution rules  

Example:

    import x from "./math.js"; // required

---

### 17. Module caching behavior

Modules are loaded once.

Example:

    require("./math");
    require("./math");

Executed only the first time.

Effects:

- Faster subsequent loads  
- Shared state across imports  

---

### 18. require.resolve() (powerful debugging trick)

You can ask Node:

    require.resolve("chalk");

This returns:

Exact resolved file path.

Useful for:

- Debugging resolution issues  
- Understanding dependency trees  

---

### 19. NODE_PATH (legacy feature, avoid)

Node once supported:

Environment-based module lookup paths.

Rarely used today.

Can cause confusing bugs.

Avoid in modern projects.

---

### 20. Common beginner mistakes

Forgetting relative prefix:

    require("utils");   // ❌ package lookup
    require("./utils"); // ✅ file lookup

Assuming index.js always loads:

`package.json.main` may override.

Misunderstanding directory resolution order.

---

### 21. Final mental model

Node module resolution follows predictable rules:

Relative paths (`./`, `../`):

- Resolved from current file  
- Filesystem-style lookup  

Package names:

- Resolved via node_modules  
- Upward directory traversal  

Key principles:

- First match wins  
- Modules are cached  
- node_modules lookup walks upward  

Understanding module resolution explains:

Most mysterious "Cannot find module" errors.
