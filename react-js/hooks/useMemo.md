The `useMemo` hook in React is used to **optimize performance** by **memoizing a computed value** — meaning it **remembers the result of a calculation** until its dependencies change.

### 📌 Syntax

```jsx
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

---

### ✅ When to use `useMemo`:

Use it when:

* You have an **expensive computation** (e.g., filtering, sorting, complex calculations).
* The computed value is used in rendering.
* The inputs to the computation (dependencies) don’t change often.

---

### 📦 Example

```jsx
import React, { useMemo, useState } from 'react';

function App() {
  const [count, setCount] = useState(0);
  const [input, setInput] = useState('');

  const expensiveCalculation = (num) => {
    console.log('Calculating...');
    let result = 0;
    for (let i = 0; i < 100000000; i++) {
      result += num;
    }
    return result;
  };

  const computedValue = useMemo(() => expensiveCalculation(count), [count]);

  return (
    <div>
      <input value={input} onChange={(e) => setInput(e.target.value)} />
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <p>Computed Value: {computedValue}</p>
    </div>
  );
}
```

In the example above:

* Typing in the input won’t trigger the expensive computation.
* The `expensiveCalculation` only runs when `count` changes.

---

### ⚠️ Common Mistakes:

* Using `useMemo` for simple values — unnecessary and adds complexity.
* Forgetting to list all dependencies in the array.


Here's a clear **comparison between `useMemo` and `useCallback`** in React:

---

### 🔁 `useMemo` vs `useCallback`

| Feature            | `useMemo`                                       | `useCallback`                                    |
| ------------------ | ----------------------------------------------- | ------------------------------------------------ |
| **Purpose**        | Memoizes the **result of a function** (a value) | Memoizes the **function itself** (its reference) |
| **Returns**        | The **computed value**                          | The **memoized function**                        |
| **Use Case**       | Optimize **expensive calculations**             | Prevent **function recreation on re-renders**    |
| **Syntax**         | `useMemo(() => compute(a, b), [a, b])`          | `useCallback(() => doSomething(a), [a])`         |
| **Example Result** | `const value = useMemo(...);`                   | `const handler = useCallback(...);`              |
| **Common Usage**   | Derived state, filtered/sorted lists            | Event handlers passed as props                   |

---

### 🧠 Think of it like this:

* ✅ `useMemo` caches the **value** returned by a function.
* ✅ `useCallback` caches the **function definition** itself.

---

### 🧪 Simple Code Examples

#### `useMemo` – Expensive calculation

```jsx
const expensiveValue = useMemo(() => slowFunction(count), [count]);
```

#### `useCallback` – Memoized handler

```jsx
const handleClick = useCallback(() => {
  console.log('Clicked!', count);
}, [count]);
```

When `handleClick` is passed as a prop to a child component with `React.memo`, it **won’t trigger a re-render unless `count` changes**.

---

### 🧩 Together with `React.memo`

```jsx
const Child = React.memo(({ onClick }) => {
  console.log('Child rendered');
  return <button onClick={onClick}>Click</button>;
});
```

Passing `handleClick` created with `useCallback` avoids re-rendering `Child` on every parent update.

---

### 🔁 What it means:

> **“`useMemo` caches the value returned by a function”**
> means that React will **save (or "memoize") the result of a function** so that it **doesn't run the function again unless needed**.

---

### 🧠 Real-world analogy:

Imagine you're doing a hard math problem, like `1000 * 1234`.

* The first time, you solve it (takes effort).
* You write the answer on paper: `1234000`.
* Next time someone asks, instead of solving again, you **just read it from the paper** — that's caching!

---

### ✅ React version:

```js
const result = useMemo(() => 1000 * 1234, []);
```

* First render: React runs the function → `1000 * 1234` → result is `1234000`.
* Stores (caches) that value.
* On the next render, it **doesn't recalculate** `1000 * 1234` again, unless the dependencies change.

---

### ⚠️ Without `useMemo`:

Every render would **recalculate** the value — even if nothing changed. This can slow down performance, especially for **heavy calculations**.


> ❌ **No**, you **cannot do the same with `useCallback`** — because `useCallback` is for **functions**, not **values**.

---

### 🔁 Comparison by Example:

#### ✅ `useMemo` – Caches a value

```jsx
const result = useMemo(() => 1000 * 1234, []);
// result is a number: 1234000
```

#### 🔁 `useCallback` – Caches a function

```jsx
const multiply = useCallback(() => 1000 * 1234, []);
// multiply is a function you can call: multiply()
```

So if you want the **value now**, use `useMemo`.
If you want to **remember a function** to call later, use `useCallback`.

---

### 🔧 When to Use Each

| Goal                        | Hook to Use   |
| --------------------------- | ------------- |
| Avoid recalculating a value | `useMemo`     |
| Avoid recreating a function | `useCallback` |

---

### 🎯 Example to Understand Better

#### ❌ Wrong: Using `useCallback` to get a value

```js
const getValue = useCallback(() => 1000 * 1234, []);
console.log(getValue); // returns a function
console.log(getValue()); // returns 1234000
```

You must call it manually — not helpful if you just want the value.

#### ✅ Right: Using `useMemo`

```js
const value = useMemo(() => 1000 * 1234, []);
console.log(value); // directly gives 1234000
```

---

So in short:

* 🧠 `useMemo`: "Give me the result and cache it."
* 🛠 `useCallback`: "Give me the function and cache it."

---

## ✅ What it really means

### 🧠 `useMemo`

> "Run the function **now**, get the **value**, and cache that **value**."

```js
const value = useMemo(() => 5 + 10, []);
// value is 15
```

* The arrow function `() => 5 + 10` is called immediately.
* React stores (caches) the result: `15`.

You get **the value directly**: `value === 15`.

---

### 🛠 `useCallback`

> "Don’t run the function now. Just **remember (cache)** the **function itself**."

```js
const getValue = useCallback(() => 5 + 10, []);
// getValue is a function
```

* Nothing is calculated yet.
* You only have a reference to the function: `() => 5 + 10`.

To get the result, you still need to **call it**:

```js
const result = getValue(); // result === 15
```

So `useCallback` gives you a **function**, not the result.

---

## 🧪 Summary Table

| Hook          | What it returns         | When it runs the function         |
| ------------- | ----------------------- | --------------------------------- |
| `useMemo`     | The **value/result**    | Immediately on render (if needed) |
| `useCallback` | The **function itself** | Only runs when you **call** it    |

---

## 💡 Real-Life Analogy

| Hook          | Analogy                                                       |
| ------------- | ------------------------------------------------------------- |
| `useMemo`     | You do the math `5 + 10`, write `15` on paper and reuse it.   |
| `useCallback` | You write a **calculator** on paper, so you can use it later. |

