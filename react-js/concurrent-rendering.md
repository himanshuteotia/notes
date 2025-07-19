Concurrent Rendering in React is about **how React prepares and applies UI updates in a non-blocking way**. It was introduced starting with React 18 and the new **Concurrent Mode** features. The goal is to make apps feel more responsive by letting React interrupt, pause, or restart rendering work when something more urgent (like user input) happens.

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/731c0456-1cae-4060-aa41-3601ce7b79cb" />


### ✅ What It Is

Traditionally, React rendering was **synchronous**:

* React would render the entire component tree for an update in one go.
* During this process, the UI could "freeze" because JavaScript was busy rendering.

With **Concurrent Rendering**:

* React can split rendering work into smaller units.
* It can **pause** rendering and let the browser handle urgent tasks (like animations or input).
* Then it **resumes** rendering later, without discarding progress.

---

### ✅ How It Works

* React uses **interruptible rendering** and a **scheduler** internally.
* Priorities: React assigns priorities to tasks (e.g., user input > data fetching update).
* If something urgent happens while React is rendering, it can **abort current work** and handle the urgent task first.

---

### ✅ Key Benefits

➭ Smoother UI even with heavy components
➭ Better handling of slow devices or network
➭ Enables **features like Suspense** (for data fetching)
➭ Helps **Streaming SSR** (Server-Side Rendering)

---

### ✅ Features Built on Concurrent Rendering

* `useTransition()` → mark some updates as low-priority
* `Suspense` → show fallback while loading data
* `startTransition()` → non-blocking state updates
* Streaming SSR with Suspense

---

### ✅ Example

```jsx
import { useState, useTransition } from "react";

function App() {
  const [input, setInput] = useState("");
  const [list, setList] = useState([]);
  const [isPending, startTransition] = useTransition();

  const handleChange = (e) => {
    setInput(e.target.value);
    startTransition(() => {
      // Low priority update
      const items = Array.from({ length: 20000 }, (_, i) => e.target.value + i);
      setList(items);
    });
  };

  return (
    <>
      <input value={input} onChange={handleChange} />
      {isPending && <p>Loading...</p>}
      <ul>{list.map((item, i) => <li key={i}>{item}</li>)}</ul>
    </>
  );
}
```

Here:

* Typing feels smooth because list generation is **deferred**.


