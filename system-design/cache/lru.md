Think of an **LRU cache (Least Recently Used cache)** like a small box with limited space for items.

➭ It stores a fixed number of key-value pairs.

➭ If you add more items than it can hold, it throws out the one you used **longest ago** (least recently used).

➭ The idea is: recently used data is more likely to be used again soon, so keep that, and drop the “stale” ones.

---

### Example in real life

Imagine your browser’s “back” button:

* It keeps a history of visited pages.
* If it can only remember 5 pages, when you visit a 6th page, it forgets the one you visited the longest time ago.

---

### Why do we need it?

LRU cache is useful when:

➭ Data access is expensive (e.g., calling an API, reading from disk, or querying a database).

➭ You want to store only a limited number of recent results to speed up future requests.

---

### Core rules

1. **get(key)** → return the value if the key exists, else -1.

   * When accessed, that key becomes the "most recently used".

2. **put(key, value)** → add or update a key.

   * If the cache is full, remove the **least recently used** entry.

---

### Quick visual

Capacity = 3

* Insert \[1] → Cache = \[1]
* Insert \[2] → Cache = \[2, 1]
* Insert \[3] → Cache = \[3, 2, 1]
* Access \[1] → Cache = \[1, 3, 2] (1 is now most recent)
* Insert \[4] → Cache = \[4, 1, 3] (2 was least recent → removed)

Alright, let’s break it down.

An **LRU cache** (Least Recently Used) is a cache that keeps only a fixed number of items. When the cache is full and a new item needs to be added, it removes the **least recently used** item.

The two operations you need fast are:

➭ `get(key)` → return value if key exists, otherwise -1

➭ `put(key, value)` → insert/update a key. If full, evict least recently used

The key point: both operations should be **O(1)**.

---

### Data structures to use

1. **HashMap** → quick lookup of nodes by key.
2. **Doubly Linked List** → keeps track of usage order (most recent at the head, least recent at the tail).

This combo works because:

➭ HashMap gives O(1) access to nodes

➭ Doubly Linked List allows O(1) move-to-front and O(1) deletion from tail

---

### Steps

1. When you `get(key)`:

   * If key exists → move that node to the head (most recently used).
   * If not → return -1.

2. When you `put(key, value)`:

   * If key already exists → update value + move to head.
   * If key doesn’t exist:

     * If capacity is full → remove the tail node (least recently used).
     * Insert new node at head.

---

### Example in JavaScript

Here’s a clean implementation:

```js
class Node {
  constructor(key, value) {
    this.key = key;
    this.value = value;
    this.prev = null;
    this.next = null;
  }
}

class LRUCache {
  constructor(capacity) {
    this.capacity = capacity;
    this.map = new Map();

    // dummy head and tail
    this.head = new Node(null, null);
    this.tail = new Node(null, null);
    this.head.next = this.tail;
    this.tail.prev = this.head;
  }

  _remove(node) {
    node.prev.next = node.next;
    node.next.prev = node.prev;
  }

  _add(node) {
    node.next = this.head.next;
    node.prev = this.head;
    this.head.next.prev = node;
    this.head.next = node;
  }

  get(key) {
    if (!this.map.has(key)) return -1;

    const node = this.map.get(key);
    this._remove(node);
    this._add(node);
    return node.value;
  }

  put(key, value) {
    if (this.map.has(key)) {
      const node = this.map.get(key);
      node.value = value;
      this._remove(node);
      this._add(node);
    } else {
      if (this.map.size >= this.capacity) {
        // remove LRU (before tail)
        const lru = this.tail.prev;
        this._remove(lru);
        this.map.delete(lru.key);
      }
      const newNode = new Node(key, value);
      this._add(newNode);
      this.map.set(key, newNode);
    }
  }
}
```

---

### Usage

```js
const cache = new LRUCache(2);

cache.put(1, 1); // cache: {1=1}
cache.put(2, 2); // cache: {2=2, 1=1}
console.log(cache.get(1)); // 1 (moves 1 to most recent)
cache.put(3, 3); // evicts 2 → cache: {3=3, 1=1}
console.log(cache.get(2)); // -1
```
