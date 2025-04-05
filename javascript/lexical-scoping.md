**Lexical scoping** (also called **static scoping**) means that the **scope of a variable is determined by its location in the source code**, and **nested functions have access to variables declared in their outer scope**.

In JavaScript, this is how the language determines what variables a function can access.

---

### 🔍 Example:

```javascript
function outer() {
  let outerVar = "I am from outer";

  function inner() {
    console.log(outerVar); // ✅ Can access outerVar because of lexical scoping
  }

  inner();
}

outer();
```

**Explanation:**
- The `inner` function is **defined inside** the `outer` function.
- Because of **lexical scoping**, `inner()` has access to `outerVar`, even though `outerVar` is not declared inside `inner()`.

---

### ❌ What lexical scoping does NOT allow:
You **cannot access** inner function variables from the outer scope.

```javascript
function outer() {
  function inner() {
    let innerVar = "I am inner";
  }

  console.log(innerVar); // ❌ Error: innerVar is not defined
}

outer();
```

---

### Summary:
- **Lexical scope is fixed at the time of writing the code**, not at runtime.
- Functions can **look "outward"** for variables but not **"inward"**.
- This is a key reason **closures** work in JavaScript.