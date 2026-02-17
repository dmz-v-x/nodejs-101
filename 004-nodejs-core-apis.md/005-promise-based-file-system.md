## Node.js Promise-Based File System APIs  

## 1. Where We Are Coming From (Recap)

Classic callback-style async code:

    fs.readFile("file.txt", "utf-8", (err, data) => {

        if (err) {
            console.error(err);
            return;
        }

        console.log(data);
    });

This works ✔️

But introduces serious problems.

---

## PART 1 — The Problem With Callbacks

---

## 2. Problem 1 — Callback Hell 😱

Example:

    fs.readFile("a.txt", (err, dataA) => {

        fs.readFile("b.txt", (err, dataB) => {

            fs.writeFile("c.txt", dataA + dataB, () => {
                console.log("Done");
            });

        });

    });

Issues:

❌ Deep nesting  
❌ Hard to read  
❌ Hard to debug  
❌ Hard to maintain  

---

## 3. Problem 2 — Error Handling is Messy

Every callback requires:

    if (err) { ... }

Miss one check:

➡ Silent bugs  
➡ Crashes  
➡ Unexpected behavior  

---

## 4. Problem 3 — Control Flow Complexity

Parallel vs sequential logic becomes confusing.

Code loses clarity quickly.

---

## PART 2 — Enter Promises

---

## 5. What is a Promise? (Simple Definition)

A Promise represents:

"A value that will exist in the future."

Instead of:

“Call me when done”

We say:

“I promise to give you the result later.”

---

## 6. Why Promises Solve Callback Problems

✔ No nesting pyramid  
✔ Linear control flow  
✔ Centralized error handling  
✔ Composable logic  

---

## PART 3 — Node’s Promise-Based fs APIs

---

## 7. fs.promises / fs/promises

Node provides a promise-based filesystem API.

Modern import (Recommended):

    import fs from "fs/promises";

No callbacks.

All functions return promises.

---

## 8. Available Promise-Based APIs

✔ readFile  
✔ writeFile  
✔ appendFile  
✔ mkdir  
✔ readdir  
✔ stat  
✔ unlink  

All async.

All promise-returning.

---

## PART 4 — Reading Files With Promises

---

## 9. Old Callback Style

    fs.readFile("file.txt", "utf-8", (err, data) => {
        console.log(data);
    });

---

## 10. Modern async/await Style

    import fs from "fs/promises";

    const data = await fs.readFile("file.txt", "utf-8");

    console.log(data);

Cleaner.

Linear.

Readable.

---

## 11. Critical Clarification ⚠️

This LOOKS synchronous.

But is NOT blocking.

---

## 12. Why await Does NOT Block Node.js

Important distinction:

await:

✔ Pauses THIS async function  
❌ Does NOT block event loop  

Node remains responsive.

Other requests continue processing.

---

## 13. Mental Model of await

Think:

“Pause my function, not the entire program.”

---

## PART 5 — async Requirement (Major Gotcha)

---

## 14. JavaScript Rule

❌ Invalid:

    const data = await fs.readFile("file.txt");

✔ Syntax error outside async context.

---

## 15. Correct Usage

Inside async function:

    async function read() {
        const data = await fs.readFile("file.txt");
    }

OR top-level (modern Node supports this):

    const data = await fs.readFile("file.txt");

---

## 16. Gotcha — Older Node Versions 

Top-level await supported only in:

✔ ES Modules  
✔ Modern Node versions  

---

## PART 6 — Writing Files With Promises

---

## 17. Callback Style

    fs.writeFile("out.txt", "Hello", () => {
        console.log("Written");
    });

---

## 18. Promise Style

    await fs.writeFile("out.txt", "Hello");

    console.log("Written");

No nesting.

No callback wrapper.

---

## PART 7 — Error Handling (Major Benefit)

---

## 19. Callback Error Handling

    fs.readFile("file.txt", (err, data) => {

        if (err) {
            console.error(err);
            return;
        }

    });

Repeated everywhere.

---

## 20. Promise Error Handling (Modern)

    try {

        const data = await fs.readFile("missing.txt", "utf-8");

        console.log(data);

    } catch (err) {

        console.error("File read failed:", err.message);

    }

---

## 21. Why try/catch is Superior

✔ Centralized  
✔ Cleaner logic  
✔ Easier debugging  
✔ Familiar control flow  

---

## 22. Major Gotcha 

try/catch ONLY works with:

✔ await  
✔ Promise chains  

Will NOT catch unhandled async errors.

---

## PART 8 — Promise Chains vs async/await

---

## 23. Promise Chain Style

    fs.readFile("file.txt")
        .then(data => console.log(data))
        .catch(err => console.error(err));

---

## 24. async/await Style

    try {
        const data = await fs.readFile("file.txt");
        console.log(data);
    } catch (err) {
        console.error(err);
    }

async/await = Cleaner mental model.

---

## PART 9 — Advanced Benefits of Promise APIs

---

## 25. Sequential Logic Becomes Natural

    const a = await fs.readFile("a.txt");
    const b = await fs.readFile("b.txt");

No nesting pyramid.

---

## 26. Parallel Execution Made Easy

    const [a, b] = await Promise.all([
        fs.readFile("a.txt"),
        fs.readFile("b.txt")
    ]);

Massive performance advantage.

---

## 27. Gotcha — await inside loops

Bad pattern:

    for (...) {
        await fs.readFile(...); // Sequential, slow
    }

Better:

✔ Promise.all()

---

## 28. Memory & Performance Insight

Promises improve structure.

But still depend on:

✔ Thread pool  
✔ Disk speed  
✔ OS behavior  

Async ≠ Instant

---

## PART 10 — Common Beginner Mistakes

---

## ❌ Forgetting async keyword

## ❌ Missing try/catch

## ❌ Assuming await blocks Node

## ❌ Mixing callbacks & promises poorly

## ❌ Not handling rejected promises

---

## FINAL PROFESSIONAL RULE

---

## 29. Modern Node.js Best Practice

✔ Use `fs/promises`  
✔ Use async/await  
✔ Prefer Promise-based APIs  
✔ Avoid callback-style unless required  

This is now:

Industry standard  
Interview expectation  
Production-grade practice  

---

## FINAL MENTAL MODEL

---

Callback APIs:

✔ Work  
❌ Messy scaling  

Promise APIs:

✔ Linear  
✔ Clean  
✔ Maintainable  
✔ Composable  

---

## Final Takeaway

Promises + async/await solve:

✔ Callback hell  
✔ Error handling complexity  
✔ Control flow confusion  

They represent modern Node.js architecture.

Mastering them is mandatory for serious backend development.
