The **Fiber Tree** is React’s internal data structure that represents your application’s UI and powers its **reconciliation** and **rendering process**. It was introduced in React 16 to make rendering more efficient and allow features like **time slicing**, **suspense**, and **concurrent rendering**.

---

### ✅ **What is a Fiber Tree?**

* A **tree of Fiber nodes** that describes the entire UI and its components.
* Each **Fiber node** is a JavaScript object that represents:

  * A component (functional or class)
  * A DOM element (like `<div>`, `<button>`)
* It stores both **current render info** and **link to the previous render**.

---

### ✅ **Why Fiber?**

Before Fiber (React 15 and earlier):

* React used a **stack-based reconciler**, which was synchronous and **blocked the main thread** during large updates.

With Fiber (React 16+):

* React **breaks rendering into chunks** (small units of work) so it can pause, resume, and prioritize updates for a smooth user experience.

---

### ✅ **Structure of a Fiber Node**

A single **Fiber node** typically contains:

```js
{
  type: ComponentType or DOM tag (e.g., 'div'),
  stateNode: DOM node or class instance,
  pendingProps: Props for upcoming render,
  memoizedProps: Props from previous render,
  memoizedState: State from previous render,
  child: First child Fiber,
  sibling: Next sibling Fiber,
  return: Parent Fiber,
  alternate: The old Fiber node (previous render),
  effectTag: What needs to be done (update, delete),
}
```

---

### ✅ **Fiber Tree Layout**

The **Fiber Tree** is like a linked structure:

* **child** → points to the first child component
* **sibling** → points to the next component on the same level
* **return** → points to the parent
* **alternate** → points to the previous version (used for diffing)

Example:

```plaintext
App
 ├─ Header
 │   └─ Logo
 └─ Main
     ├─ Sidebar
     └─ Content
```

Each of these is a Fiber node connected through **child**, **sibling**, and **return**.

---

### ✅ **How It Helps React Render Changes**

1. When state changes:

   * React creates a **Work-In-Progress Fiber Tree**.
2. Compares it with the **Current Fiber Tree** (using `alternate` links).
3. Applies **minimal DOM changes**.
4. Swaps trees: Work-in-progress becomes the current tree.

---

✅ **In short:**
The Fiber Tree is **React's internal representation of your UI**, designed for **efficient diffing, scheduling, and incremental rendering**.

Let’s go through a **real example** of how React creates **Fiber nodes** internally for a small component tree.

---

### ✅ **Example React App**

```jsx
function Header() {
  return <h1>Header</h1>;
}

function App() {
  return (
    <div>
      <Header />
      <p>Hello</p>
    </div>
  );
}
```

When `App` renders, React builds a **Fiber Tree** to represent it.

---

### ✅ **Fiber Tree Structure**

For this component tree:

```
<App>
 └─ <div>
      ├─ <Header>
      │    └─ <h1>
      └─ <p>
```

React creates **Fiber nodes** for each element.

---

### ✅ **Fiber Node Example (Simplified)**

#### 1. **App Fiber Node**

```js
{
  tag: FunctionComponent,
  type: App,
  stateNode: null,        // functional components don't have a DOM node
  pendingProps: {},
  memoizedProps: {},
  memoizedState: null,
  child: FiberNodeForDiv, // first child
  sibling: null,          // no siblings at root
  return: null,           // no parent (it's root)
  alternate: previousAppFiberNode, // previous render
}
```

---

#### 2. **div Fiber Node**

```js
{
  tag: HostComponent,    // DOM element
  type: 'div',
  stateNode: <div>,      // actual DOM node
  pendingProps: {},
  memoizedProps: {},
  child: FiberNodeForHeader, // first child inside div
  sibling: FiberNodeForP,    // next element in same div
  return: FiberNodeForApp,   // parent
  alternate: previousDivFiberNode,
}
```

---

#### 3. **Header Fiber Node**

```js
{
  tag: FunctionComponent,
  type: Header,
  stateNode: null,
  child: FiberNodeForH1,
  sibling: null,       // no siblings inside Header
  return: FiberNodeForDiv,
}
```

---

#### 4. **h1 Fiber Node**

```js
{
  tag: HostComponent,
  type: 'h1',
  stateNode: <h1>,     // actual DOM node
  child: null,
  sibling: null,
  return: FiberNodeForHeader,
}
```

#### 5. **p Fiber Node**

```js
{
  tag: HostComponent,
  type: 'p',
  stateNode: <p>,      // actual DOM node
  child: null,
  sibling: null,
  return: FiberNodeForDiv,
}
```

---

### ✅ **How They Connect**

* `child` → points to the first child node.
* `sibling` → next element at same level.
* `return` → parent node.
* `alternate` → previous version (used for diffing).

So the **Fiber Tree** looks like this:

```
App
 └── div
      ├── Header
      │     └── h1
      └── p
```

---

✅ **Important Points**

* React **does not keep a copy of the full old Virtual DOM**; it stores old values via `alternate` in each Fiber node.
* This structure allows React to **compare old vs new efficiently** during reconciliation.
