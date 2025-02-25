## 💡 **Lexical Scope in JavaScript**

**Lexical Scope** (also known as **Static Scope**) means that the scope of a variable is determined by its **position** in the source code **at the time of writing**, not at runtime.

In simple terms:
- **Where** you **declare** a variable in the code decides where it can be accessed.
- **Inner functions** have access to variables defined in their **outer (parent) functions**, but the reverse is not true.

---

### ✅ **Key Points:**
1. **Scope is fixed during code writing, not during execution.**
2. Functions can **access variables** from their **parent scope**.
3. It follows a **hierarchical structure** (think of it like layers).

---

### 📊 **Example of Lexical Scope:**

```javascript
function outer() {
  let outerVar = "I'm from outer!";

  function inner() {
    console.log(outerVar); // ✅ Can access outerVar
  }

  inner();
}

outer();
```

**Output:**
```
I'm from outer!
```

Here, the `inner()` function has access to `outerVar` because of **Lexical Scope**.

---

### ⚠️ **What if we reverse it?**

```javascript
function outer() {
  function inner() {
    let innerVar = "I'm inside inner!";
  }

  console.log(innerVar); // ❌ ReferenceError: innerVar is not defined
  inner();
}

outer();
```

Here, the **outer function** can't access `innerVar` because **Lexical Scope** only allows **inner functions** to access outer variables, **not the other way around**.

---

### 🎯 **Lexical Scope + Closures:**
Closures work **because of lexical scoping**.

```javascript
function counter() {
  let count = 0;

  return function () {
    count++;
    return count;
  };
}

const increment = counter();
console.log(increment()); // 1
console.log(increment()); // 2
```

Here, the **inner function** "remembers" the `count` variable even after `counter()` has finished executing.

---

### 💬 **In Simple Terms:**
- **"Lexical"** → Related to the code's location.
- **"Scope"** → Where variables can be accessed.

👉 **Lexical Scope** → Variables can be accessed based on **where** they're **defined** in the code, **not how or when** they're called.

Let's dive into **Closures**, **Scope Chains**, and **Hoisting** — all closely tied to **Lexical Scope** in JavaScript.

---

## 🔒 **1. Closures**

### ✅ **What is a Closure?**
A **closure** is a function that **remembers** variables from its **outer lexical scope** even when the outer function has **finished execution**.

### 📊 **Example:**

```javascript
function outer() {
  let outerVar = "I'm from outer!";

  return function inner() {
    console.log(outerVar); // Still accessible
  };
}

const closureFunc = outer();
closureFunc(); // Output: I'm from outer!
```

**💡 Why it works?**  
Even though `outer()` has finished, the `inner()` function retains access to `outerVar` because of the closure.

### ⚡ **Real-World Use Case (Counter Example):**

```javascript
function counter() {
  let count = 0;

  return function () {
    count++;
    return count;
  };
}

const increment = counter();
console.log(increment()); // 1
console.log(increment()); // 2
```

The `count` variable is **preserved** across multiple calls due to the closure.

---

## 🌳 **2. Scope Chain**

### ✅ **What is a Scope Chain?**
When a variable is not found in the current scope, JavaScript looks **up the chain** to parent scopes until it finds the variable or hits the **global scope**.

### 📊 **Example:**

```javascript
let globalVar = "Global";

function outer() {
  let outerVar = "Outer";

  function inner() {
    let innerVar = "Inner";
    console.log(globalVar); // ✅ Global
    console.log(outerVar);  // ✅ Outer
    console.log(innerVar);  // ✅ Inner
  }

  inner();
}

outer();
```

### ⚠️ **But this will throw an error:**

```javascript
function outer() {
  function inner() {
    let innerVar = "Inner";
  }

  console.log(innerVar); // ❌ ReferenceError
}

outer();
```

**💡 Why?**  
The **outer function** can’t access variables from its **inner function** — the scope chain only works **downwards**, not upwards.

---

## 🎈 **3. Hoisting**

### ✅ **What is Hoisting?**
**Hoisting** is JavaScript’s default behavior of **moving declarations to the top** of their scope **before code execution**.

- **Variables (`var`)** are **hoisted** but initialized as `undefined`.
- **Functions** are fully hoisted.
- **`let` and `const`** are hoisted but placed in a **"Temporal Dead Zone" (TDZ)** — they can’t be accessed before declaration.

### 📊 **Example of Hoisting with `var`:**

```javascript
console.log(a); // undefined (due to hoisting)
var a = 5;
```

The above code is interpreted like this:

```javascript
var a;
console.log(a); // undefined
a = 5;
```

---

### ⚡ **Example with `let` and `const`:**

```javascript
console.log(b); // ❌ ReferenceError: Cannot access 'b' before initialization
let b = 10;
```

### ⚡ **Function Hoisting:**

```javascript
greet(); // ✅ "Hello!"

function greet() {
  console.log("Hello!");
}
```

But **Function Expressions** aren’t hoisted:

```javascript
greet(); // ❌ TypeError: greet is not a function

const greet = function () {
  console.log("Hi!");
};
```

---

## 📌 **Summary Table:**

| Concept        | Definition                                                  | Key Rule                               |
|----------------|-------------------------------------------------------------|----------------------------------------|
| **Lexical Scope** | Variables are accessible based on where they are declared. | Inner functions can access outer vars. |
| **Closures**     | A function remembers variables from its outer scope.        | Keeps variables alive even after outer function ends. |
| **Scope Chain**  | Variable lookup climbs up the scope chain.                  | Looks up until global scope is reached. |
| **Hoisting**     | Declarations are moved to the top before execution.         | `var` is hoisted as `undefined`, but `let`/`const` are in TDZ. |

---

💬 **Want me to explain topics like `this` binding, async/await, or ES6+ features next?** 🚀