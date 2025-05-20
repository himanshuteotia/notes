## 🪝 What is `useCallback`?

`useCallback` is a React hook that **memoizes a function**, meaning it **remembers** the same function reference between renders — **unless its dependencies change**.

---

## 🤔 Why use it?

In React, functions are **re-created on every render**. This is usually fine, but:

* If you pass those functions to **child components**, it can trigger **unnecessary re-renders**.
* Or if used in `useEffect`/`useMemo`, it may cause unexpected behavior.

So `useCallback` helps optimize performance by keeping the same function reference **unless something actually changes**.

---

## 🔧 Syntax:

```jsx
const memoizedCallback = useCallback(() => {
  // function logic
}, [dependencies]);
```

---

## ✅ Simple Example (Without `useCallback`)

```jsx
function Parent() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    console.log("Clicked");
  };

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <Child onClick={handleClick} />
    </div>
  );
}
```

Every time you click the button, `Parent` re-renders → new `handleClick` function → `Child` also re-renders (even if it didn’t need to).

---

## ✅ Now with `useCallback`

```jsx
const handleClick = useCallback(() => {
  console.log("Clicked");
}, []);
```

Now `handleClick` won’t change unless its dependencies do — so `Child` won't re-render unnecessarily.

---

## 🔍 Realistic Use Case: Preventing Unwanted Re-renders

```jsx
const Child = React.memo(({ onClick }) => {
  console.log("Child rendered");
  return <button onClick={onClick}>Child Button</button>;
});
```

Now wrap the child with `React.memo`, and use `useCallback` for the function. This way, the child only re-renders if the function changes — improving performance in larger apps.

---

## ⚠️ When **not** to use `useCallback`

If you're not passing the function down to memoized children or not using it inside `useEffect`, you probably **don’t need it**. Overusing `useCallback` can **hurt performance** by adding unnecessary complexity.

Example : 

Let’s build a small project to **see the effect of `useCallback`** and understand when it really matters.

---

## 🔧 Project: Counter + Memoized Button

### 🎯 Goal:

* We’ll have a `Counter` and a `Button`.
* The `Button` will be a separate **memoized** child component.
* We’ll use and compare the behavior **with and without `useCallback`**.

---

### ✅ Step 1: Set up the basic structure

```jsx
import React, { useState, useCallback } from "react";

// Memoized Child
const Button = React.memo(({ onClick, label }) => {
  console.log(`Rendering Button - ${label}`);
  return <button onClick={onClick}>{label}</button>;
});

function App() {
  const [count, setCount] = useState(0);
  const [otherCount, setOtherCount] = useState(0);

  // 🔁 Without useCallback
  // const increment = () => setCount(count + 1);

  // ✅ With useCallback
  const increment = useCallback(() => {
    setCount((c) => c + 1);
  }, []);

  const incrementOther = () => setOtherCount(otherCount + 1);

  return (
    <div>
      <h2>Count: {count}</h2>
      <h2>Other Count: {otherCount}</h2>

      <Button onClick={increment} label="Increment Count" />
      <Button onClick={incrementOther} label="Increment Other Count" />
    </div>
  );
}

export default App;
```

---

### 🔍 What to Observe:

1. Open the browser console.
2. Click the **"Increment Other Count"** button.
3. Notice:

   * The **button for "Increment Count" does not re-render** — thanks to `useCallback`.
   * If you comment out `useCallback` and use a normal function, both buttons will log "Rendering Button..." on every change.

---

### ⚡ Why it works:

* `React.memo()` prevents a component from re-rendering **if props don’t change**.
* Without `useCallback`, the function `increment` is re-created on every render → `Button` sees it as a new prop → triggers re-render.
* With `useCallback`, the function is **memoized**, so `Button` sees the **same reference** → no re-render.


Many developers feel confused about when to use `useCallback`. Let’s break it down further with **real-world guidance** and answer this:

---

## 🧠 TL;DR — When should you use `useCallback`?

> **Only use `useCallback` when:**
>
> ✅ You're passing a function as a prop to a **memoized child component** (`React.memo`)
> ✅ Or you want to **preserve the function identity** across renders (e.g., used in a `useEffect`, dependency array, or custom hook)
> ❌ Don’t use it just to "optimize" — it can backfire if not needed.

---

## 🔁 Example Without `useCallback` (Problem)

Here’s what React sees:

```js
const handleClick = () => { console.log("Click"); };
```

Every time the parent re-renders, `handleClick` becomes a **new function**, even if it *does the same thing*.
So when passed to a memoized child:

```js
<ChildComponent onClick={handleClick} />
```

React.memo compares the old vs new `onClick`. Since it's a new function, the child **re-renders unnecessarily**.

---

## ✅ Fix with `useCallback`

```js
const handleClick = useCallback(() => {
  console.log("Click");
}, []);
```

Now, `handleClick` will **only change if dependencies change**, preserving the identity and avoiding a child re-render.

---

## ⚠️ But... Don’t Use it Just Because

```js
const handleClick = useCallback(() => {
  console.log("Click");
}, []);
```

If you’re **not** passing it to a memoized child OR using it in a dependency array, this provides **no benefit** — and can make code harder to read.

---

## 🔍 Real-World Situations to Use `useCallback`

### ✅ Case 1: Props to `React.memo`

```js
const SearchInput = React.memo(({ onSearch }) => {
  console.log("SearchInput rendered");
  return <input onChange={onSearch} />;
});
```

In the parent:

```js
const handleSearch = useCallback((e) => {
  console.log(e.target.value);
}, []);

<SearchInput onSearch={handleSearch} />
```

Prevents re-renders of `SearchInput` on unrelated state changes.

---

### ✅ Case 2: Function dependency in `useEffect`

```js
const fetchData = useCallback(() => {
  // fetch logic
}, []);

useEffect(() => {
  fetchData();
}, [fetchData]); // ✅ dependency won't change unless needed
```

If `fetchData` wasn't memoized, this `useEffect` could trigger unnecessarily.

---

### ✅ Case 3: Used inside a custom hook

When writing hooks that depend on stable callbacks:

```js
function useEventListener(event, handler) {
  useEffect(() => {
    window.addEventListener(event, handler);
    return () => window.removeEventListener(event, handler);
  }, [handler]); // you need handler to be stable
}
```

Caller:

```js
const handleKey = useCallback((e) => {
  if (e.key === "Escape") closeModal();
}, [closeModal]);

useEventListener("keydown", handleKey);
```

---

## 🧪 Rule of Thumb:

| Scenario                                     | Use `useCallback`? |
| -------------------------------------------- | ------------------ |
| Function is passed to a **memoized** child   | ✅ Yes              |
| Function is a **dependency** in `useEffect`  | ✅ Yes              |
| Inside a **custom hook** with stable handler | ✅ Yes              |
| Not passed down or tracked                   | ❌ No               |


## ❓ If I don't use `React.memo`, will `useCallback` still work?

### ✅ Yes — `useCallback` **still works**, but...

> **Its main benefit — preventing re-renders — won’t be visible unless you’re using `React.memo`.**

Let’s break it down:

---

### 🎯 What `useCallback` Does:

* It **remembers the same function** reference between renders.
* This is useful when React **compares dependencies** — such as in:

  * `useEffect`
  * `React.memo`
  * Custom hooks

So if you're not **passing that function** as a prop to a memoized component or not using it in a dependency array — it’s **doing nothing useful**.

---

### 🔍 Visual Example

#### Without `React.memo`:

```js
const Button = ({ onClick }) => {
  console.log("Button rendered");
  return <button onClick={onClick}>Click me</button>;
};
```

Even if you write this in the parent:

```js
const handleClick = useCallback(() => console.log("clicked"), []);
<Button onClick={handleClick} />
```

👉 `Button` will still **re-render every time the parent renders**, because it's **not memoized**.

---

### ✅ With `React.memo`:

```js
const Button = React.memo(({ onClick }) => {
  console.log("Button rendered");
  return <button onClick={onClick}>Click me</button>;
});
```

Now, if `onClick` does **not change** (thanks to `useCallback`), the child won’t re-render.

---

## ✅ Summary

| Feature Used          | Will `useCallback` help? | Why?                                                      |
| --------------------- | ------------------------ | --------------------------------------------------------- |
| ✅ `React.memo`        | Yes                      | It avoids re-rendering by comparing props (functions too) |
| ✅ `useEffect` with fn | Yes                      | Dependency won’t falsely trigger                          |
| ❌ Just passing a fn   | No                       | No visible benefit                                        |

---

### 📌 Conclusion:

* `useCallback` **does work alone**, but **you need `React.memo`** to **see** the performance gain when passing functions to children.
* Without `React.memo`, the function might stay the same, but React **won’t skip re-rendering the child**.


## 💡 `useCallback` **only prevents re-renders** of child components **when used with `React.memo`.**

Here’s why:

### 🔁 What `useCallback` does:

It **returns the same function reference** across renders — unless dependencies change.

But React doesn't skip rendering a component **just because a function is the same** — it only skips rendering **if that component is wrapped in `React.memo`**, which tells React to compare props.

---

### 🧠 Think of it like this:

* `useCallback` = **don’t recreate the function** every time.
* `React.memo` = **don’t re-render unless props change**.
* **Together** = performance win ✅

---

### ✅ Real-world analogy:

You're calling a delivery guy (`Child component`) with the same order (`onClick`).

* `useCallback` ensures the **order is identical every time** (same props).
* `React.memo` ensures the **delivery guy won’t re-process the order if it hasn’t changed**.

If there’s no `React.memo`, he still **re-delivers the same order**, wasting time.

---

### 🧪 Example to Prove It

#### Without `React.memo`

```jsx
const Button = ({ onClick }) => {
  console.log("Button rendered");
  return <button onClick={onClick}>Click</button>;
};
```

Even with `useCallback`, this still logs every time parent renders. ❌

#### With `React.memo`

```jsx
const Button = React.memo(({ onClick }) => {
  console.log("Button rendered");
  return <button onClick={onClick}>Click</button>;
});
```

Now, if `onClick` is memoized with `useCallback`, the child won’t log again ✅

## ✅ `React.memo` and `useCallback` are **separate tools** that solve different parts of the puzzle:

| Concept       | What it does                                       | When to use                                                                    |
| ------------- | -------------------------------------------------- | ------------------------------------------------------------------------------ |
| `React.memo`  | Prevents re-render if props haven’t changed        | Use on **child components** you want to optimize                               |
| `useCallback` | Keeps function **reference stable** across renders | Use when passing a function to `React.memo` child, `useEffect`, or custom hook |

---

### 🔁 Without `useCallback`:

Even if a function does the same thing every time, its reference is **new** → React.memo sees this as a "changed prop" → **re-renders the child**.

### 🧠 With `useCallback`:

Function reference **stays the same** → React.memo sees "no change" → **skips re-render** ✅

---

### 🧪 Final Check: Minimal Example

```jsx
const handleClick = () => console.log("clicked");
```

✅ *Same logic*
❌ *New reference every time*

```jsx
const handleClick = useCallback(() => console.log("clicked"), []);
```

✅ *Same logic*
✅ *Same reference (until dependencies change)*



