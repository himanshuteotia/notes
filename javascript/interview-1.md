Here are some **commonly asked JavaScript interview questions**, categorized by **difficulty level** along with **brief explanations** and examples:

---

## 🟢 **Beginner Level**

1. **What are the data types available in JavaScript?**
   - **Primitive**: `String`, `Number`, `Boolean`, `Null`, `Undefined`, `Symbol`, `BigInt`
   - **Non-Primitive**: `Object`, `Array`, `Function`

2. **What is the difference between `var`, `let`, and `const`?**
   - `var`: Function-scoped, hoisted, can be re-declared.
   - `let`: Block-scoped, hoisted but not initialized.
   - `const`: Block-scoped, cannot be re-assigned.

3. **Explain the concept of Hoisting.**
   - JavaScript moves variable and function declarations to the top of their scope before execution.

4. **What is the difference between `==` and `===`?**
   - `==`: Loose equality (performs type coercion).
   - `===`: Strict equality (no type coercion).

5. **What is a Closure in JavaScript?**
   - A function that remembers its outer variables even after the outer function has closed.

   ```javascript
   function outer() {
     let count = 0;
     return function inner() {
       return ++count;
     };
   }
   const counter = outer();
   console.log(counter()); // 1
   console.log(counter()); // 2
   ```

6. **What are truthy and falsy values?**
   - **Falsy**: `false`, `0`, `''`, `null`, `undefined`, `NaN`
   - **Truthy**: Everything else

---

## 🟡 **Intermediate Level**

1. **Explain Event Delegation.**
   - Using a parent element to handle events for its children using **event bubbling**.

2. **What is the Event Loop?**
   - The Event Loop handles asynchronous tasks using a **queue** and the **call stack**, enabling non-blocking I/O.

3. **What is the difference between `null` and `undefined`?**
   - `undefined`: Variable declared but not assigned a value.
   - `null`: Assigned intentionally to indicate "no value."

4. **What are Promises?**
   - A way to handle asynchronous operations.

   ```javascript
   const promise = new Promise((resolve, reject) => {
     setTimeout(() => resolve("Done!"), 1000);
   });

   promise.then(console.log); // Done!
   ```

5. **What is the difference between `call()`, `apply()`, and `bind()`?**
   - `call()`: Calls a function with a given `this` value and arguments.
   - `apply()`: Similar to `call()`, but accepts arguments as an array.
   - `bind()`: Returns a new function with `this` bound.

6. **What is a Higher-Order Function?**
   - A function that takes another function as an argument or returns a function.

   ```javascript
   const numbers = [1, 2, 3];
   const doubled = numbers.map(num => num * 2);
   ```

---

## 🔴 **Advanced Level**

1. **What are Async/Await?**
   - Syntactic sugar over Promises for cleaner asynchronous code.

   ```javascript
   async function fetchData() {
     try {
       let response = await fetch('https://api.example.com');
       let data = await response.json();
       console.log(data);
     } catch (error) {
       console.error(error);
     }
   }
   ```

2. **What is Currying?**
   - Transforming a function with multiple arguments into a sequence of functions, each with a single argument.

   ```javascript
   const add = a => b => a + b;
   console.log(add(2)(3)); // 5
   ```

3. **Explain Debouncing and Throttling.**
   - **Debouncing**: Delays function execution until after a specified time has elapsed.
   - **Throttling**: Ensures a function is called at most once every specified time interval.

4. **What is the difference between `map()`, `filter()`, and `reduce()`?**
   - `map()`: Transforms elements.
   - `filter()`: Selects elements that pass a test.
   - `reduce()`: Reduces an array to a single value.

5. **Explain Prototypal Inheritance.**
   - Objects can inherit properties from another object using the prototype chain.

6. **What is the Module Pattern?**
   - A design pattern to encapsulate code into private and public parts using IIFEs (Immediately Invoked Function Expressions).

---

## ⚡ **Bonus: Tricky Questions**

1. **What will be the output?**

   ```javascript
   console.log([] + []);          // ''
   console.log([] + {});          // '[object Object]'
   console.log({} + []);          // 0
   console.log(+'abc');           // NaN
   console.log(0.1 + 0.2 === 0.3); // false
   ```

2. **How does `this` work in JavaScript?**
   - Depends on how the function is called:
     - In a method → refers to the object.
     - In a function → refers to `window` (in non-strict mode).
     - In arrow functions → inherits from parent scope.

3. **Explain Memory Leaks in JavaScript.**
   - Occur when objects are no longer used but are still referenced, preventing garbage collection.

---

### 💡 **Tips for Interviews:**
- Master **asynchronous JS** (`Promises`, `Async/Await`, `Event Loop`).
- Understand **ES6+ features** (e.g., **Spread/Rest**, **Destructuring**, **Modules**).
- Practice **coding questions** on platforms like **LeetCode** and **HackerRank**.