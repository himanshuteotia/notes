### 🔹 What is `useEffect`?

`useEffect` is a **React Hook** that lets you perform **side effects** in your function components.

Side effects are operations that interact with the outside world or affect something outside the component, such as:

* Fetching data from an API
* Updating the DOM
* Setting up timers or subscriptions

---

### 🔹 Basic Syntax

```jsx
useEffect(() => {
  // Your code here (side effect)
}, [dependencies]);
```

---

### 🔹 Example 1: Run once on mount (like `componentDidMount`)

```jsx
useEffect(() => {
  console.log("Component mounted");
}, []);
```

📝 Explanation:
The empty array `[]` means this effect runs **only once** when the component first renders.

---

### 🔹 Example 2: Run every time a variable changes

```jsx
useEffect(() => {
  console.log("Count changed:", count);
}, [count]);
```

📝 Explanation:
This runs the effect **every time** `count` changes.

---

### 🔹 Example 3: Run on mount and cleanup on unmount (like `componentWillUnmount`)

```jsx
useEffect(() => {
  const timer = setInterval(() => {
    console.log("Tick");
  }, 1000);

  return () => {
    clearInterval(timer); // Cleanup
    console.log("Component unmounted");
  };
}, []);
```

📝 Explanation:
The return function is a **cleanup function**. It's used to clean up resources when the component unmounts.

---

### 🔹 Common Uses of `useEffect`

| Use Case               | How to Use                                         |
| ---------------------- | -------------------------------------------------- |
| Fetch data             | Call API in `useEffect` and store results in state |
| Event listeners        | Add listeners in effect, remove them in cleanup    |
| Timers / intervals     | Start timer in effect, clear in cleanup            |
| Subscribing to streams | Subscribe in effect, unsubscribe in cleanup        |

---

### 🔹 Summary Rules

1. `useEffect` runs **after** the component renders.
2. Dependency array `[]` controls **when** the effect runs.
3. Return a function to clean up (like removing listeners, clearing timers).


### ❓What if you **don’t provide** a dependency array in `useEffect`?

```jsx
useEffect(() => {
  console.log("This runs");
});
```

### ✅ Behavior:

If you omit the dependency array entirely, \*\*the effect will run after **every single render** of the component — including the **initial mount** and **every update**.

---

### 🔁 Example:

```jsx
import React, { useState, useEffect } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log("Effect ran!");
  });

  return (
    <div>
      <p>{count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

### 👇 What Happens:

* On first render: `console.log("Effect ran!")` runs ✅
* On every button click (i.e., every `count` update): it runs **again** ✅

---

### ⚠️ Why this can be **dangerous or inefficient**:

If you:

* Perform expensive operations (like API calls or heavy computations),
* Set state inside the effect without a proper check,

...it can cause **performance issues** or even **infinite loops**.

---

### ✅ Best Practice:

✔️ **Always** provide a dependency array — even if it's empty — to control when your effect should run.

## ❌ Example Without Dependency Array (Causes Infinite Loop)

```jsx
import React, { useState, useEffect } from 'react';

function UserComponent() {
  const [user, setUser] = useState(null);

  useEffect(() => {
    // Simulate API call
    fetch("https://jsonplaceholder.typicode.com/users/1")
      .then((res) => res.json())
      .then((data) => {
        setUser(data); // 🔁 State update triggers re-render
      });
  });

  return <div>{user ? user.name : "Loading..."}</div>;
}
```

### ❌ What’s the Problem?

* The `setUser(data)` updates the state.
* That causes a re-render.
* On re-render, `useEffect()` **runs again** because no dependency array is given.
* It fetches data again and sets state again...
* 🔁 Infinite loop of fetch ➡️ setState ➡️ re-render ➡️ fetch...

---

## ✅ Fixed Version With Dependency Array

```jsx
useEffect(() => {
  fetch("https://jsonplaceholder.typicode.com/users/1")
    .then((res) => res.json())
    .then((data) => {
      setUser(data);
    });
}, []); // ✅ Runs only once on mount
```

Now:

* Effect runs only once when the component mounts.
* `setUser` updates state.
* React re-renders, but `useEffect` doesn’t run again because dependencies haven’t changed.
* ✅ No infinite loop.

---

## ⚠️ Rule of Thumb:

If your effect:

* **Should only run once** → use `[]`
* **Should run when specific values change** → use `[thoseValues]`
* **Should run every render** → (rare) omit array — but be careful!
