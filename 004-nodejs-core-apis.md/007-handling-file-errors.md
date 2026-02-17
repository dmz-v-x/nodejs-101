## Handling File Errors Properly in Node.js  

## PART 1 — The Big Picture

---

## 1. Why File Errors Happen

File operations interact with:

✔ Operating System  
✔ Disk  
✔ Permissions  
✔ Filesystem state  

Many things can go wrong:

❌ File missing  
❌ File already exists  
❌ Permission denied  
❌ Invalid paths  
❌ Locked files  

---

## 2. How Node Represents Errors

Node.js returns errors as objects:

    {
      code: "ENOENT",
      errno: -2,
      syscall: "open",
      path: "file.txt"
    }

**error.code** is what matters most.

---

## 3. Golden Rule

❗ Never rely only on error.message  
❗ Always check error.code

Messages vary.

Codes are stable.

---

## PART 2 — ENOENT 

---

## 4. What is ENOENT?

ENOENT = Error NO ENTity

Meaning:

File or directory does not exist.

---

## 5. Example Scenario

    await fs.readFile("missing.txt");

Error:

    code: "ENOENT"

---

## 6. Proper Handling Pattern

    try {

        await fs.readFile("missing.txt");

    } catch (err) {

        if (err.code === "ENOENT") {
            console.log("File does not exist");
        } else {
            throw err;
        }

    }

---

## 7. Common Beginner Mistake

Treating all errors the same:

❌ Hides real issues  
❌ Makes debugging harder  

---

## 8. Real-World ENOENT Causes

✔ Wrong relative path  
✔ Incorrect cwd  
✔ File deleted  
✔ Race conditions  
✔ Typo in filename  

---

## 9. Professional Strategy

Sometimes ENOENT is expected.

Example:

✔ Optional config files  
✔ Cache files  
✔ User-generated files  

---

## PART 3 — EEXIST (Critical Write Error)

---

## 10. What is EEXIST?

EEXIST = Error EXISTs

Meaning:

Trying to create something that already exists.

---

## 11. Example Scenario

    await fs.mkdir("logs");

If logs folder exists:

    code: "EEXIST"

---

## 12. Proper Handling Pattern

    try {

        await fs.mkdir("logs");

    } catch (err) {

        if (err.code === "EEXIST") {
            console.log("Directory already exists");
        } else {
            throw err;
        }

    }

---

## 13. Cleaner Modern Alternative 

Use built-in options:

    await fs.mkdir("logs", { recursive: true });

Avoids EEXIST entirely.

Preferred approach.

---

## 14. Common Beginner Mistake

Ignoring EEXIST:

❌ Can crash apps  
❌ Break startup logic  

---

## PART 4 — Other Important File Errors

---

## 15. EACCES (Permission Denied)

Meaning:

Insufficient permissions.

Example:

✔ Writing protected folder  
✔ Reading restricted file  

Must never be silently ignored.

---

## 16. EPERM (Operation Not Permitted)

Often similar to EACCES.

Common on Windows.

---

## 17. EISDIR (Expected File, Got Directory)

Example:

    fs.readFile("folder");

Error:

✔ It's a directory, not file.

---

## 18. ENOTDIR (Expected Directory, Got File)

Opposite scenario.

---

## 19. EMFILE (Too Many Open Files)

Meaning:

File descriptor exhaustion.

Common in:

✔ Heavy FS workloads  
✔ Loops without throttling  

---

## PART 5 — Advanced Error Handling Concepts

---

## 20. Expected vs Unexpected Errors

Professional mindset:

✔ Some errors are normal  
✔ Some errors are critical  

Example:

ENOENT for cache file → Normal ✔️  
ENOENT for DB config → Critical ❌  

---

## 21. Fail-Fast Principle

Critical file missing?

➡ Crash early.

Better than unpredictable behavior.

---

## 22. Race Conditions (Very Important)

Bad pattern:

    if (existsSync(file)) {
        writeFile(file);
    }

Danger:

File may change between check & write  

Correct pattern:

➡ Attempt operation → Handle error

---

## 23. Correct Race-Safe Strategy

    try {
        await fs.writeFile(file, data, { flag: "wx" });
    } catch (err) {
        if (err.code === "EEXIST") {
            console.log("File already exists");
        }
    }

---

## 24. Why Checking First is Dangerous

Filesystem state is dynamic.

Never assume stability.

---

## PART 6 — Error Handling Best Practices

---

## 25. Always Inspect error.code

Never rely on string messages.

---

## 26. Never Silently Ignore Errors

Bad:

    catch (err) {}

Danger:

Hides bugs  
Creates undefined behavior  

---

## 27. Log Meaningful Context

Better:

    console.error("File write failed:", err.code, err.path);

---

## 28. Wrap Low-Level Errors (Professional Practice)

    throw new Error(`Config load failed: ${err.code}`);

Cleaner debugging upstream.

---

## FINAL PROFESSIONAL MENTAL MODEL

---

## Common Error Codes

| Code   | Meaning |
|--------|----------|
| ENOENT | File/Dir not found |
| EEXIST | File/Dir already exists |
| EACCES | Permission denied |
| EPERM  | Operation not permitted |
| EISDIR | Path is directory |
| ENOTDIR| Path is not directory |
| EMFILE | Too many open files |

---

## Critical Rules

Always check error.code  
Distinguish expected vs fatal errors  
Never pre-check filesystem state blindly  
Avoid existsSync → write patterns  
Never swallow errors silently  
Handle ENOENT & EEXIST intentionally  

---

## Final Takeaway

File errors are NOT exceptional.

They are normal operating conditions.

Correct handling leads to:

✔ Stable applications  
✔ Predictable behavior  
✔ Easier debugging  
✔ Production safety  

Incorrect handling leads to:

Random crashes  
Data corruption  
Race condition bugs  
Nightmare debugging  

This is core Node.js engineering discipline.
