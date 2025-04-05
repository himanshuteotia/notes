### ✅ What is a Closure?

A **closure** is when a function **remembers** the variables from its **lexical scope**, even after the outer function has finished execution.

---

### 🔍 Example:

```javascript
function outer() {
  let count = 0;

  return function inner() {
    count++;
    console.log("Count is:", count);
  };
}

const counter = outer(); // outer() returns inner, and counter now holds that inner function

counter(); // Count is: 1
counter(); // Count is: 2
counter(); // Count is: 3
```

---

### 📌 Explanation:

- `outer()` defines a variable `count` and returns `inner()`.
- `inner()` has access to `count` because of **lexical scoping**.
- Even though `outer()` has finished executing, the `inner()` function still **remembers** `count` — this is a **closure**.
- So, every time you call `counter()`, it keeps the updated value of `count`.

---

### 🔁 Real-life use case:

You can use closures to create **private variables** or **encapsulated state** — very useful in JavaScript when you don’t want some data to be accessed directly.


Let’s create a **practical closure example** — a **counter** with a **reset feature**, like something you might actually use in an app.

---

### ✅ Counter with Increment & Reset using Closure

```javascript
function createCounter() {
  let count = 0;

  return {
    increment: function () {
      count++;
      console.log("Count:", count);
    },
    reset: function () {
      count = 0;
      console.log("Counter reset");
    },
  };
}

const counter = createCounter();

counter.increment(); // Count: 1
counter.increment(); // Count: 2
counter.reset();     // Counter reset
counter.increment(); // Count: 1
```

---

### 🧠 Why it works?

- `createCounter()` defines `count` privately.
- It returns an object with two functions: `increment` and `reset`.
- Both functions **remember** the `count` variable via **closure**.
- `count` is **not accessible directly**, which keeps it **private**.

---

### 🧪 Try accessing `count` directly:

```javascript
console.log(counter.count); // undefined ❌
```

---

This pattern is commonly used when building reusable utilities or hooks in frontend frameworks like React.

Let’s understand a **debounce function** — it’s a common and super useful use-case of **closures** in JavaScript, especially in frontend development.

---

### 🔁 What is **Debouncing**?

Debouncing means **delaying a function execution** until after a certain time has passed since the **last time it was called**.

👉 Useful when:
- Typing in a search input (don’t want to call API on every key press)
- Window resizing
- Button clicks

---

### ✅ Debounce Function using Closure:

```javascript
function debounce(fn, delay) {
  let timeoutId;

  return function (...args) {
    clearTimeout(timeoutId); // cancel the last timer if it’s running

    timeoutId = setTimeout(() => {
      fn(...args); // call the original function
    }, delay);
  };
}
```

---

### 🔍 How to use it:

```javascript
function saveInput(value) {
  console.log("Saving data:", value);
}

const debouncedSave = debounce(saveInput, 1000);

// Simulate typing
debouncedSave("H");
debouncedSave("Hi");
debouncedSave("Himanshu"); // 👈 Only this will be saved after 1 second
```

---

### 🧠 Why closure?

- The `timeoutId` is **private** to each call of `debounce()`.
- It’s **remembered** across multiple `debouncedSave()` calls because of **closure**.

Let’s now learn about **throttling**, another powerful use case of **closures**.

---

### 🔁 What is **Throttling**?

**Throttling** ensures that a function is **only called once in a given time interval**, **no matter how many times** the event occurs.

👉 Useful when:
- Scrolling
- Resize events
- Repeated button clicks

---

### ✅ Throttle Function using Closure:

```javascript
function throttle(fn, delay) {
  let lastCall = 0;

  return function (...args) {
    const now = Date.now();

    if (now - lastCall >= delay) {
      lastCall = now;
      fn(...args);
    }
  };
}
```

---

### 🔍 How to use it:

```javascript
function handleScroll() {
  console.log("Scroll event:", new Date().toLocaleTimeString());
}

const throttledScroll = throttle(handleScroll, 2000);

// Simulating scroll events:
window.addEventListener("scroll", throttledScroll);
```

---

### 🧠 Why closure?

- The variable `lastCall` is **preserved** between calls using **closure**.
- Each call compares the current time to `lastCall` to decide whether to run `fn()`.

---

### ✅ Summary of Debounce vs Throttle:

| Feature       | Debounce                           | Throttle                         |
|---------------|------------------------------------|----------------------------------|
| Use case      | Wait till user stops triggering    | Limit how often function runs    |
| Triggered     | **After** delay                    | **Every** delay interval         |
| Example       | Search box input                   | Scroll/resize events             |

---

Want me to show a real example using a **button click** with throttle/debounce or a React use-case?