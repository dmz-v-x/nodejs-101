## Semantic Versioning

### 1. Big picture: Why Semantic Versioning exists

You often see package versions like:

    1.2.3

This is not random numbering.

It follows a convention called:

Semantic Versioning (SemVer)

SemVer explains:

- Why updates behave differently  
- Why some updates break applications  
- Why npm installs specific versions  

---

### 2. Why versioning exists at all

Imagine a world without versioning.

You install a package.

It updates.

Your app breaks.

You have no idea what changed.

Versioning exists to:

- Communicate changes  
- Set expectations  
- Reduce surprises  
- Maintain ecosystem stability  

SemVer is essentially a communication system.

---

### 3. What is Semantic Versioning?

Semantic Versioning defines a structure:

    MAJOR.MINOR.PATCH

Example:

    2.4.7

Each number carries meaning.

Not decoration.

---

### 4. PATCH version (bug fixes)

PATCH = Last number

    1.0.0 → 1.0.1

Meaning:

- Bug fixes  
- No API changes  
- Backward compatible  

Examples:

- Fix typo  
- Fix edge case  
- Improve performance  

Rule of thumb:

Should never break user code.

---

### 5. MINOR version (new features)

MINOR = Middle number

    1.1.0 → 1.2.0

Meaning:

- New functionality added  
- Backward compatible  

Examples:

- New helper function  
- Optional parameters  
- Additional utilities  

Existing code should continue working.

---

### 6. MAJOR version (breaking changes)

MAJOR = First number

    1.0.0 → 2.0.0

Meaning:

- Breaking changes introduced  
- Old code may fail  

Examples:

- Removed function  
- Changed function behavior  
- Changed exports  
- Changed return types  

Major bumps signal:

“You must review your code.”

---

### 7. Real example

Suppose your package:

    {
      "version": "1.0.0"
    }

Changes:

| Change                | Version bump |
|-----------------------|--------------|
| Fix bug               | 1.0.1        |
| Add new helper        | 1.1.0        |
| Rename function       | 2.0.0        |

SemVer is how you communicate intent.

---

### 8. Important truth: SemVer is a convention

SemVer is NOT enforced by JavaScript.

It is a social contract.

Maintainers may:

- Follow it strictly ✔️  
- Abuse it ❌  

Real-world implication:

Even patch updates can occasionally break things.

---

### 9. Special case: Version 0.x.x (major gotcha)

This is widely misunderstood.

Version:

    0.x.x

Means:

“Initial development phase.”

Rules change slightly.

---

### 10. SemVer rules for 0.x.x

In version 0:

- MINOR may contain breaking changes  
- PATCH is still bug fixes  

Example:

    0.2.0 → 0.3.0

May break your code.

Why?

Major version is still zero.

Stability not guaranteed.

---

### 11. Why many libraries stay in 0.x

Signals:

- API unstable  
- Frequent changes  
- Early-stage development  

Professional insight:

Be cautious with 0.x dependencies.

---

### 12. How npm interprets versions (critical concept)

When you see:

    "chalk": "^5.3.0"

That symbol matters more than the numbers.

---

### 13. Common version range symbols

| Symbol   | Meaning                      |
|----------|------------------------------|
| ^1.2.3   | Allow MINOR & PATCH updates |
| ~1.2.3   | Allow PATCH updates only     |
| 1.2.3    | Exact version               |

---

### 14. Caret (^) behavior

    "^1.2.3"

Allows:

- 1.2.4 ✔️  
- 1.3.0 ✔️  
- 2.0.0 ❌  

Meaning:

Accept backward-compatible updates.

---

### 15. Caret behavior with 0.x (major gotcha)

    "^0.2.3"

Allows:

- 0.2.4 ✔️  
- 0.3.0 ❌  

Why?

Because MINOR may contain breaking changes in 0.x.

Caret becomes stricter.

---

### 16. Tilde (~) behavior

    "~1.2.3"

Allows:

- 1.2.4 ✔️  
- 1.3.0 ❌  

Used when:

You want safer updates.

---

### 17. Exact versions

    "1.2.3"

Locks dependency completely.

No automatic upgrades.

Trade-off:

- Maximum stability ✔️  
- No bug fixes automatically ❌  

---

### 18. Why npm sometimes installs newer versions

Because:

npm respects SemVer ranges.

Not randomness.

---

### 19. Lock files (critical professional concept)

Files like:

- package-lock.json  
- pnpm-lock.yaml  
- yarn.lock  

Exist to:

Freeze exact dependency versions.

Even when ranges allow updates.

---

### 20. Why lock files are essential

Without lock files:

- Team installs different versions  
- CI installs newer versions  
- Bugs become inconsistent  

Lock files prevent chaos.

---

### 21. Pre-release versions (advanced topic)

Versions like:

    1.0.0-alpha
    2.0.0-beta
    3.1.0-rc.1

Meaning:

- alpha → very unstable  
- beta → mostly stable  
- rc → release candidate  

npm treats these specially.

---

### 22. Breaking change reality (professional insight)

Major bump means breaking change.

But:

Minor bumps can also introduce subtle behavior shifts.

Always read changelogs.

---

### 23. SemVer as ecosystem stability mechanism

Without SemVer:

- Dependency hell increases  
- Libraries break constantly  
- Teams lose trust  

SemVer is a shared language.

---

### 24. Common beginner misunderstandings

“Any update is safe”  
“Versions don’t matter”   
“npm updated randomly” 

Correct understanding:

npm follows SemVer rules.

---

### 25. Final mental model

    MAJOR.MINOR.PATCH

- PATCH → Fix problems  
- MINOR → Add functionality  
- MAJOR → Break compatibility  

SemVer is not just numbering.

It is a contract between library authors and users.

Understanding SemVer explains:

Most mysterious npm update behaviors.
