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
