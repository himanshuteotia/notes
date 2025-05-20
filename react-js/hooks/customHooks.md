### 🔧 What are Custom Hooks in React?

**Custom Hooks** are **JavaScript functions** whose names start with "`use`" and **can call other Hooks** like `useState`, `useEffect`, etc. They allow you to **extract and reuse stateful logic** across components — essentially helping you **DRY up** your React code.

> Think of custom hooks as a way to bundle `useState`, `useEffect`, or any other hooks logic into a reusable function.

---

### ✅ Why Use Custom Hooks?

* Reuse logic across multiple components
* Improve readability and separation of concerns
* Keep components clean and focused
* Avoid prop drilling or repeating logic

---

### 🛠️ How to Create a Custom Hook?

Let’s say you want to track the window width in multiple components.

#### 1. Create a file `useWindowWidth.js`:

```js
import { useState, useEffect } from 'react';

function useWindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    const handleResize = () => setWidth(window.innerWidth);
    
    window.addEventListener('resize', handleResize);

    // Clean up
    return () => window.removeEventListener('resize', handleResize);
  }, []);

  return width;
}

export default useWindowWidth;
```

#### 2. Use it in a component:

```jsx
import React from 'react';
import useWindowWidth from './useWindowWidth';

function MyComponent() {
  const width = useWindowWidth();

  return <h1>Window width is: {width}px</h1>;
}
```

---

### 🧠 Tips for Custom Hooks

* Always start the name with `use`
* You **can** call other hooks inside
* They should **not** return JSX — only logic/data
* Keep them **pure** and side-effect-safe like other React hooks

### 🧠 Why do we use **hooks inside custom hooks**?

React **Hooks** (like `useState`, `useEffect`, etc.) **can only be used inside**:

1. Functional components
2. Custom hooks

They **cannot be used** inside regular JavaScript functions, conditionals, loops, or class components. So we wrap logic that uses hooks **inside custom hooks** to **make them reusable** while still obeying the **Rules of Hooks**.

---

### 🔄 Is a custom hook a component?

**No**, a custom hook is **not** a component. It is **just a regular function** that:

* Must start with `use`
* Can call other hooks inside
* Returns data, not JSX

Think of it as **a helper function for stateful logic** — **not for rendering UI**.

---

### ⚙️ What happens when you call a custom hook?

Let’s say you call:

```js
const [count, setCount] = useCounter();
```

Here's what happens under the hood:

1. `useCounter()` is invoked **during component render**
2. Inside `useCounter()`, any hooks like `useState` or `useEffect` get executed **in the same order** every render (which React relies on)
3. React tracks the state or effect using its internal hook system
4. The returned values from the custom hook become part of your component’s state/logic

So essentially:

* **The custom hook doesn’t have its own state instance.**
* The **component using it owns the state**.

---

### ✅ Example: What actually happens?

```js
// Custom Hook
function useCounter(initial = 0) {
  const [count, setCount] = useState(initial);
  return [count, setCount];
}

// Component
function MyComponent() {
  const [count, setCount] = useCounter(5); // behaves like useState(5)

  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
```

Here:

* `useCounter` simply wraps `useState`
* `MyComponent` owns the `count` state
* Every render, `useCounter` runs again (like any function in React), but **React knows which hook to track**

---

### 🔒 React's Hook Rules: Why all this matters?

Custom hooks exist to **respect** these 2 rules:

1. 🔁 **Always call hooks at the top level** — not inside loops, conditions, or nested functions.
2. 🔗 **Only call hooks from functional components or custom hooks**

So instead of repeating:

```js
const [value, setValue] = useState('');
useEffect(() => { /* logic */ }, []);
```

…in every component, you can just write:

```js
const [value, setValue] = useMyCustomLogic();
```


### 🧩 What does “**The custom hook doesn’t have its own state instance**” mean?

It means:

> **The state defined inside a custom hook actually belongs to the component that calls the custom hook — not the hook itself.**

---

### 🔍 Why?

React is **component-driven**, and **only components have lifecycle and state instances.** When a component renders, React executes the entire function, including any custom hooks inside it.

Custom hooks are **just plain JavaScript functions** — they don’t maintain their own state across renders.

Instead:

* They **call React hooks** (like `useState`, `useEffect`)
* React **associates the state with the component** calling them — not with the custom hook

---

### 🧪 Example to illustrate:

#### Custom hook:

```js
function useCounter(initial = 0) {
  const [count, setCount] = useState(initial);
  return [count, setCount];
}
```

#### Using it in two components:

```jsx
function A() {
  const [countA, setCountA] = useCounter();
  return <button onClick={() => setCountA(c => c + 1)}>A: {countA}</button>;
}

function B() {
  const [countB, setCountB] = useCounter();
  return <button onClick={() => setCountB(c => c + 1)}>B: {countB}</button>;
}
```

---

### 🎯 What’s happening:

* `useCounter()` is called inside both `A` and `B`
* Each call creates a **new `useState` inside that component**
* So `countA` and `countB` are **completely separate**
* The custom hook just wraps the logic — but the **actual state is owned and tracked by the calling component**

---

### 🧠 So in simple terms:

> Calling a custom hook is like copying-pasting the logic inside your component — React tracks the state per component, **not per hook function**.


### ✅ If any state inside a custom hook changes, it **will re-render** the component that uses it.

This happens because:

* The **state defined in the custom hook is actually part of the component** that calls it.
* When `useState` updates, React triggers a **re-render of that component** — just like it would if you used `useState` directly inside the component.

---

### 🔍 Let’s see a concrete example:

```js
// ✅ Custom Hook
function useCounter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log('Custom hook: useEffect triggered');
  }, [count]);

  return [count, setCount];
}
```

```jsx
// ✅ Component
function CounterComponent() {
  const [count, setCount] = useCounter();

  console.log('Component rendered');

  return (
    <button onClick={() => setCount(prev => prev + 1)}>
      Count is {count}
    </button>
  );
}
```

---

### 🧠 What happens when the button is clicked:

1. `setCount(prev => prev + 1)` is called.
2. `count` state (inside `useCounter`) is updated.
3. Since `count` is tracked by React **for the component**, React re-renders `CounterComponent`.
4. That re-render also re-invokes `useCounter()` (just like any function during render).
5. The updated `count` is shown in the UI.

---

### 💡 Summary:

| Thing                         | Triggers Re-render? | Why?                           |
| ----------------------------- | ------------------- | ------------------------------ |
| State update in a custom hook | ✅ Yes               | State is tied to the component |
| Custom hook code changing     | ❌ No                | Unless props/state change      |
| Return value of hook changes  | ✅ Yes               | If tied to state change        |


Here are **5 practical examples** of custom hooks to help you learn and reuse logic in real-world scenarios:

---

### 1️⃣ `useLocalStorage` – Persist state in `localStorage`

```js
import { useState, useEffect } from 'react';

function useLocalStorage(key, initialValue) {
  const [value, setValue] = useState(() => {
    const stored = localStorage.getItem(key);
    return stored !== null ? JSON.parse(stored) : initialValue;
  });

  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [key, value]);

  return [value, setValue];
}
```

**Usage:**

```jsx
const [name, setName] = useLocalStorage('username', '');
```

---

### 2️⃣ `useDebounce` – Debounce a value (e.g., input field)

```js
import { useState, useEffect } from 'react';

function useDebounce(value, delay = 500) {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const timer = setTimeout(() => setDebouncedValue(value), delay);
    return () => clearTimeout(timer);
  }, [value, delay]);

  return debouncedValue;
}
```

**Usage:**

```jsx
const debouncedSearchTerm = useDebounce(searchTerm, 300);
```

---

### 3️⃣ `usePrevious` – Track the previous value of a variable

```js
import { useRef, useEffect } from 'react';

function usePrevious(value) {
  const ref = useRef();
  useEffect(() => {
    ref.current = value;
  }, [value]);
  return ref.current;
}
```

**Usage:**

```jsx
const prevCount = usePrevious(count);
```

---

### 4️⃣ `useToggle` – Toggle a boolean value

```js
import { useState } from 'react';

function useToggle(initialValue = false) {
  const [state, setState] = useState(initialValue);
  const toggle = () => setState(prev => !prev);
  return [state, toggle];
}
```

**Usage:**

```jsx
const [isOpen, toggleOpen] = useToggle();
```

---

### 5️⃣ `useOnlineStatus` – Detect if the user is online or offline

```js
import { useState, useEffect } from 'react';

function useOnlineStatus() {
  const [isOnline, setIsOnline] = useState(navigator.onLine);

  useEffect(() => {
    const handleOnline = () => setIsOnline(true);
    const handleOffline = () => setIsOnline(false);

    window.addEventListener('online', handleOnline);
    window.addEventListener('offline', handleOffline);

    return () => {
      window.removeEventListener('online', handleOnline);
      window.removeEventListener('offline', handleOffline);
    };
  }, []);

  return isOnline;
}
```

**Usage:**

```jsx
const isOnline = useOnlineStatus();
```

