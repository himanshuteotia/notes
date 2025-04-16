Here’s a list of **JavaScript interview questions** with **clear, detailed explanations** and examples. These are commonly asked in **front-end and back-end** roles, especially if you're working with **Node.js, TypeScript, or React**.

---

## 🔹 **1. What are the different types of scopes in JavaScript?**

### 💡 Answer:
JavaScript has the following types of scopes:
- **Global Scope**: Variables declared outside of any function/block are globally scoped.
- **Function Scope**: Variables declared with `var` inside a function are only accessible within that function.
- **Block Scope**: Variables declared with `let` or `const` are only accessible within the block they are defined in.

### 📌 Example:
```javascript
function testScope() {
  if (true) {
    var x = 10;       // function scoped
    let y = 20;       // block scoped
    const z = 30;     // block scoped
  }
  console.log(x); // ✅
  console.log(y); // ❌ ReferenceError
}
```

---

## 🔹 **2. What is the difference between `==` and `===`?**

### 💡 Answer:
- `==` checks for **value equality** with type coercion.
- `===` checks for **value and type equality** (strict equality).

### 📌 Example:
```javascript
'5' == 5   // true
'5' === 5  // false
```

---

## 🔹 **3. What is hoisting?**

### 💡 Answer:
Hoisting is a JavaScript mechanism where **variables and function declarations are moved to the top** of their scope **before execution**.

### 📌 Example:
```javascript
console.log(a); // undefined (due to hoisting)
var a = 5;
```

Functions are hoisted fully:
```javascript
foo(); // "Hello"
function foo() {
  console.log("Hello");
}
```

---

## 🔹 **4. What is the event loop in JavaScript?**

### 💡 Answer:
The event loop is what allows JavaScript to be **non-blocking**. JavaScript has a **call stack**, **callback queue**, and **event loop** that handles asynchronous operations like `setTimeout`, `fetch`, etc.

### 📌 Flow:
1. Code runs in the **call stack**.
2. Async functions go to **Web APIs** (e.g., `setTimeout`).
3. After completion, they go to the **callback queue**.
4. Event loop pushes them back to **call stack** if it’s empty.

---

## 🔹 **5. Difference between `var`, `let`, and `const`?**

| Feature        | var            | let           | const         |
|----------------|----------------|---------------|---------------|
| Scope          | Function        | Block         | Block         |
| Re-declarable  | Yes            | No            | No            |
| Re-assignable  | Yes            | Yes           | No            |
| Hoisting       | Yes (undefined)| Yes (TDZ)     | Yes (TDZ)     |

---

## 🔹 **6. What is closure in JavaScript?**

### 💡 Answer:
A closure is formed when a function **remembers** the variables from its **lexical scope** even when executed outside that scope.

### 📌 Example:
```javascript
function outer() {
  let counter = 0;
  return function inner() {
    counter++;
    console.log(counter);
  };
}

const inc = outer();
inc(); // 1
inc(); // 2
```

---

## 🔹 **7. What is the difference between `call`, `apply`, and `bind`?**

| Method | Purpose | Parameters | Invokes Function Immediately |
|--------|---------|------------|------------------------------|
| `call` | Set `this` | Individual arguments | Yes |
| `apply` | Set `this` | Arguments as array | Yes |
| `bind` | Set `this` | Individual arguments | ❌ Returns new function |

### 📌 Example:
```javascript
function greet(city) {
  console.log(`Hello ${this.name} from ${city}`);
}

const person = { name: "Himanshu" };

greet.call(person, "Delhi");
greet.apply(person, ["Dubai"]);
const newGreet = greet.bind(person, "Mumbai");
newGreet();
```

---

## 🔹 **8. What is a promise?**

### 💡 Answer:
A **promise** is an object representing the eventual **completion** or **failure** of an asynchronous operation.

### 📌 States:
- `pending`
- `fulfilled`
- `rejected`

### 📌 Example:
```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => resolve("Done"), 1000);
});

promise.then(console.log).catch(console.error);
```

---

## 🔹 **9. What is `this` in JavaScript?**

### 💡 Answer:
The value of `this` depends on **how a function is called**:
- In global: refers to `window` or `global`.
- In a method: refers to the object.
- In a function (strict mode): `undefined`.
- In arrow functions: refers to **lexical scope** (where it's defined).

---

## 🔹 **10. What is a debounce and throttle function?**

### ✅ **Debounce**: waits for a pause before running a function.
Useful for search input, window resize.

```javascript
function debounce(fn, delay) {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
}
```

### ✅ **Throttle**: runs function at most once in every X ms.
Useful for scroll, mousemove.

```javascript
function throttle(fn, limit) {
  let inThrottle;
  return (...args) => {
    if (!inThrottle) {
      fn(...args);
      inThrottle = true;
      setTimeout(() => inThrottle = false, limit);
    }
  };
}
```

