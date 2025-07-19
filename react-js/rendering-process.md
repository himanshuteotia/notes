React renders HTML on any change using a **diffing and reconciliation process with Virtual DOM** instead of reloading the whole page. Here’s how it works step by step:

---

### ✅ **React Rendering Process on State/Prop Change**

1. **Component State or Props Change**

   * When `setState` (or `useState` hook) updates state, React schedules a re-render of that component.

---

2. **Re-run Component Function**

   * For **functional components**, React calls the function again to get the **new JSX** (UI description).
   * Example:

     ```jsx
     function Counter() {
       const [count, setCount] = useState(0);
       return <h1>{count}</h1>;
     }
     ```

     When `setCount(count + 1)` is called, React **calls `Counter()` again** to get updated JSX.

---

3. **JSX → Virtual DOM**

   * The returned JSX is converted into a **new Virtual DOM tree**.
   * Virtual DOM is just a **JavaScript object** representing the UI structure.

---

4. **Diffing Algorithm**

   * React compares **previous Virtual DOM** with **new Virtual DOM** using its **reconciliation algorithm**.
   * It finds **only the parts that changed** (e.g., `<h1>0</h1>` → `<h1>1</h1>`).

---

5. **Minimal Real DOM Update**

   * React **does NOT replace the entire DOM**.
   * It updates only the changed node:

     * `document.querySelector('h1').textContent = "1"`

---

6. **Batching & Fiber**

   * React **batches multiple state updates** and uses **React Fiber** for prioritizing updates (e.g., animations vs data).

---

### ✅ **Example**

```jsx
import React, { useState } from 'react';

function App() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

**Process on Button Click:**

* `setCount(count + 1)` → triggers **re-render**.
* React **creates new Virtual DOM**: `<h1>1</h1>`.
* Compares old vs new → only `<h1>` text changes.
* Updates **only the text** in the real DOM → **fast and efficient**.

---

✅ **In short:** React renders HTML changes by:

* Re-rendering the component → Generating new Virtual DOM → Diffing → Updating only changed parts in the Real DOM.

**React saves the previous Virtual DOM inside its internal Fiber tree structure**, which is part of the **React Reconciliation algorithm**.

---

### ✅ Where is the Previous Virtual DOM Stored?

* React **doesn’t keep a separate big copy of the entire Virtual DOM** for every render (that would be too heavy).
* Instead, React uses a **Fiber tree** to represent the UI, and each component (element) has a **Fiber node**.
* Each Fiber node contains:

  * **type**: Component type (function, div, etc.)
  * **pendingProps**: Props for the upcoming render
  * **memoizedProps**: Props from the previous render
  * **memoizedState**: State from the previous render
  * **child**, **sibling**, **return**: Pointers to other fibers
  * **alternate**: A link to the previous Fiber node (old tree)

---

### ✅ How It Works

1. On **first render**:

   * React creates a **Fiber tree** for the UI.
2. On **update**:

   * React creates a **work-in-progress Fiber tree** for the new Virtual DOM.
   * The old Fiber tree is **linked** via `alternate`.
   * React compares the old fiber (previous Virtual DOM) with the new one **node by node**.
3. After reconciliation:

   * React swaps references: the new Fiber tree becomes the **current tree**.
   * Old tree is kept for the next render as `alternate`.

---

### ✅ In Simple Terms

* React doesn't keep a separate Virtual DOM object in memory for every render.
* Instead, it uses a **linked list of Fiber nodes**, where **each node knows its previous version** via the `alternate` pointer.

---

✅ **So where?**

* Inside **React's internal memory**, in a structure called **Fiber Tree**.
* Each component instance has **current Fiber** (current UI) and **work-in-progress Fiber** (next UI).
