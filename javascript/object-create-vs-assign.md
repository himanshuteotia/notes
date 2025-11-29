## Object.create

Think of this as:
“Create a new object whose **prototype** is another object.”

```js
const proto = { greet() { console.log("hi") } }

const obj = Object.create(proto)
```

Now `obj` has no own properties, but it **inherits** from `proto`.

Key points:

* Creates a new object.
* Sets the prototype to whatever you pass.
* Doesn’t copy properties, just links to them.
* Useful when you want a clean prototype chain.

Example:

```js
const a = { x: 1 }
const b = Object.create(a)

console.log(b.x) // 1 (comes from prototype)
```

## Object.assign

This one simply **copies properties** from one object to another.

```js
const target = {}
Object.assign(target, { a: 1, b: 2 })
```

Key points:

* Performs a shallow copy of own enumerable properties.
* Doesn’t touch the prototype.
* Merges objects into the target.
* Useful for cloning or combining objects.

Example:

```js
const a = { x: 1 }
const b = Object.assign({}, a)

console.log(b.x) // 1 (own property, not prototype)
console.log(Object.getPrototypeOf(b)) // Object.prototype
```

## The real difference

Here’s the thing:
Object.create creates a **new object with a prototype link**.
Object.assign **copies data onto an object**.

They solve totally different problems.

### When to use what

* Use **Object.create** when you want inheritance or a specific prototype.
* Use **Object.assign** when you want to copy or merge objects, especially for configuration objects.

Here are examples with classes, how this affects performance and memory.

Alright, let’s go deeper and make the contrast even clearer with **practical examples** you’ll actually use as a Node.js/TS dev.

---

# 1. How they behave with classes (real-world scenario)

## Object.create -> useful for inheritance-like setups

```js
const Animal = {
  walk() {
    console.log('walking...');
  }
}

const dog = Object.create(Animal);
dog.bark = function() {
  console.log('woof');
}

dog.walk(); // inherited
dog.bark(); // own method
```

Here, `dog` has **Animal as its prototype**. Perfect for lightweight inheritance without classes.

---

## Object.assign -> helps copy values into an instance

```js
class User {
  constructor() {
    this.role = 'guest';
  }
}

const user = new User();
Object.assign(user, { name: 'Himanshu', age: 28 });

console.log(user);
// { role: 'guest', name: 'Himanshu', age: 28 }
```

No prototype changes. Just updates properties.

---

# 2. How they behave with functions in objects

### Object.create → prototype functions stay shared

```js
const proto = {
  count: 0,
  inc() { this.count++ }
}

const obj1 = Object.create(proto);
const obj2 = Object.create(proto);

obj1.inc();
console.log(obj1.count); // 1
console.log(obj2.count); // 0 (independent)
```

`inc` is **shared**, but the properties are on each object.

---

### Object.assign → copies functions to each object

```js
const source = {
  count: 0,
  inc() { this.count++ }
}

const obj1 = Object.assign({}, source);
const obj2 = Object.assign({}, source);

obj1.inc();
console.log(obj1.count); // 1
console.log(obj2.count); // 1 (same initial copy)
```

Functions are copied, not shared.
Good for config objects, not good for building prototype chains.

---

# 3. How they behave with nested objects

## Object.assign → shallow copy

```js
const a = { x: { y: 10 } };
const b = Object.assign({}, a);

b.x.y = 20;
console.log(a.x.y); // 20 (same reference)
```

This surprises many people.

---

## Object.create → doesn't copy anything

Only sets the prototype.

```js
const a = { x: { y: 10 } };
const b = Object.create(a);

b.x.y = 20;
console.log(a.x.y); // 20 (same reference, inherited)
```

So nested objects are always shared unless you explicitly clone.

---

# 4. Performance difference

### Object.create

Lightweight.
Just sets the prototype.
Good for creating many objects that share behavior (like in game engines, bots, worker pools).

### Object.assign

More expensive.
Copies each enumerable property.
For small objects it's fine.
For large configs or many clones, it becomes costly.

---

# 5. Real-world Node.js scenario

Imagine you're writing a request context object for Express or Fastify.

### Using Object.create (better)

```js
const baseContext = {
  user: null,
  requestId: null,
  log() { console.log(this.requestId) }
}

function createContext() {
  return Object.create(baseContext);
}
```

Fast, minimal memory, shared methods.

---

### Using Object.assign (common but heavier)

```js
function createContext() {
  return Object.assign({}, {
    user: null,
    requestId: null,
    log() { console.log(this.requestId) }
  });
}
```

Each request creates new copies of `log`.
Higher overhead.

---

# Summary you’ll remember

* **Object.create**
  Creates a **new object with a prototype link**.
  Use it for inheritance, shared behavior, and performance.

* **Object.assign**
  **Copies properties** into an object.
  Use it for cloning, merging, configs, overriding defaults.

---

**side-by-side visual diagram** to make it even clearer.

Here’s a clean, visual way to see the difference. No fluff — just the picture in your head you need.

---

# Object.create vs Object.assign (side-by-side diagram)

## 1. Object.create

**Goal:** Make a new object that *inherits* from another object.

```
   prototype object
        |
        v
   [ proto ]   ← shared methods, shared structure

        |
        v
   [ newObj ]  ← your actual object
```

* Nothing is copied.
* Only a prototype link is created.
* Methods stay in the parent, not duplicated.

Code picture:

```js
const parent = { talk() {} }
const child = Object.create(parent)
```

child → parent (in the prototype chain)

---

## 2. Object.assign

**Goal:** Copy properties into a target object.

```
source object ---- copy values ---> target object
                        (no prototype link)
```

```
   [ source ]  { a:1, b:2 }
        |
   copies values
        v
   [ target ]  { a:1, b:2 }
```

* Properties are duplicated into the target.
* No prototype chain change.

Code picture:

```js
const target = {}
Object.assign(target, { a: 1, b: 2 })
```

target becomes a flat copy.

---

# Combined visual: The difference in one shot

```
Object.create(parent)
---------------------
parent
  |
  -> child (no copied properties)
     inherits everything


Object.assign(target, source)
-----------------------------
source { a, b, c }
copied ↓
target { a, b, c }
(no inheritance relationship)
```

---

here’s a **clear, engine-level visual** of how both actually behave under the hood.
Think of it like watching V8 build your objects in slow motion.

---

# 1. How Object.create works inside the engine

Let’s say you do:

```js
const proto = { a: 10 }
const obj = Object.create(proto)
```

Behind the scenes:

```
        ┌───────────────────────────────┐
proto → │  { a: 10 }                    │
        └───────────────────────────────┘
                  ▲
                  │  [[Prototype]]
                  │
        ┌───────────────────────────────┐
obj   → │  { }                          │
        └───────────────────────────────┘
```

Steps the engine takes:

1. Allocate a new empty object.
2. Set its internal `[[Prototype]]` pointer to `proto`.
3. Done.
   No property copying, no function duplication.

When you access `obj.a`, V8 does:

```
obj → (no a)
look at prototype → found a → return 10
```

Fast and memory-efficient.

---

# 2. How Object.assign works inside the engine

```js
const target = {}
const source = { a: 10, b: 20 }
Object.assign(target, source)
```

Behind the scenes:

```
        ┌───────────────────────────────┐
source →│ { a: 10, b: 20 }              │
        └───────────────────────────────┘

        ┌───────────────────────────────┐
target →│ { a: 10, b: 20 }              │
        └───────────────────────────────┘
```

Steps the engine takes:

1. Iterate all **own enumerable properties** of source.
2. For each:

   * Create a property on target.
   * Copy value reference.
3. Prototype doesn’t change at all.

So target doesn’t inherit, it just receives copies.

If source has functions:

```js
const source = { say() {} }
```

The function is **duplicated** onto target as its own property.

---

# 3. How it looks with nested objects

```js
const source = { x: { y: 10 } }
const copy = Object.assign({}, source)
```

Visual:

```
source.x ─────────────┐
                      │  (same reference)
copy.x ───────────────┘
```

It’s a shallow copy.

---

Now compare with Object.create:

```js
const child = Object.create(source)
```

Visual:

```
child.x → same reference (inherited)
```

But:

```
child does NOT have x in itself.
x is only in the prototype.
```

---

# 4. Combined full diagram (the clearest view)

```
Object.create(proto)
------------------------------------
        proto { a: 1, b: 2 }
               ▲
               │
   obj { } ----┘
   (nothing copied, just linked)


Object.assign(target, source)
------------------------------------
        source { a:1, b:2 }
                 │  │
            copy values
                 │  │
        target { a:1, b:2 }
        (no link, all duplicated)
```



let’s make this practical. Below are concrete Express patterns where **Object.create** and **Object.assign** naturally fit, with short, copy-paste-ready examples and notes about why one is better than the other in each case.

---

# 1) Extending or overriding Express prototypes — use `Object.create`

Express exposes prototype extension points (`express.request`, `express.response`, `app.request`, `app.response`). If you want to add methods that behave like native request/response methods (shared and inherited, not copied per request), put them on the prototype — that’s prototype inheritance territory. ([Express][1])

**Why `Object.create` matters here:** you want a prototype relationship so each `req`/`res` instance sees the method via the prototype chain (no per-request duplication).

```js
// add a convenience on every req (app-level)
const express = require('express')
const app = express()

// define a prototype object (could also modify app.request directly)
const proto = {
  userInfo() { return `${this.user?.name || 'guest'}` }
}

// set app.request to inherit from proto (demo — express allows app.request hooking)
app.request = Object.create(app.request)
Object.assign(app.request, proto) // assign prototype methods onto the app.request prototype

app.use((req, res, next) => {
  // req.userInfo is available via prototype
  console.log(req.userInfo())
  next()
})
```

This is precisely the pattern Express docs mention for overriding/extending the API — those extension points rely on prototypes. ([Express][1])

---

# 2) Creating lightweight per-request contexts — `Object.create` for shared behavior, small per-request state

When you want each request to get its own object with small, request-specific fields but shared methods, create an object that inherits behavior from a shared prototype:

```js
const baseCtx = {
  log(msg) { console.log(this.requestId, msg) },
  getUser() { return this.user }
}

app.use((req, res, next) => {
  const ctx = Object.create(baseCtx)
  ctx.requestId = req.headers['x-request-id'] || Date.now().toString()
  ctx.user = null
  req.ctx = ctx
  next()
})

// later
app.get('/', (req, res) => {
  req.ctx.log('incoming') // shared method, uses request-specific data
  res.send('ok')
})
```

This keeps one copy of methods (memory efficient) while giving each request its own fields.

---

# 3) Mocking `req`/`res` in tests — `Object.create` is your friend

A common testing approach is to create fake request/response objects that inherit from Express prototypes so middleware behaves the same as in real runtime. The Express repo and community discuss creating `req`/`res` from the prototypes for tests. Use `Object.create(express.request)` / `Object.create(express.response)`. ([GitHub][2])

```js
const express = require('express')
const req = Object.create(express.request)
const res = Object.create(express.response)

req.method = 'GET'
req.url = '/'
res.statusCode = 200

// now call middleware(req, res, next) and assertions behave like real objects
```

---

# 4) Merging middleware options or default configs — use `Object.assign`

When building middleware factories, you usually merge user options with defaults. `Object.assign` is straightforward and idiomatic:

```js
function createAuthMiddleware(opts = {}) {
  const defaults = { cookieName: 'sid', secure: true, logger: console }
  const config = Object.assign({}, defaults, opts) // shallow merge
  return function authMiddleware(req, res, next) {
    // use config.cookieName, config.secure
    next()
  }
}
```

This pattern (merge defaults with user-provided overrides) is ubiquitous in Express middleware and libraries.

---

# 5) Setting `res.locals` or `app.locals` — `Object.assign` is handy for merging data into those plain objects

`res.locals` is a per-request object intended for view data; `app.locals` is application-scoped. Both are plain objects you can mutate or merge into. For merging multiple sources into `res.locals` during middleware, `Object.assign` is common. ([GeeksforGeeks][3])

```js
app.use((req, res, next) => {
  // merge user and request info into res.locals for templates
  Object.assign(res.locals, {
    user: req.user,
    path: req.path,
  })
  next()
})
```

Note: `res.locals` lifetime is the request (it’s not persisted across requests), while `app.locals` lives for the app lifetime. Use the appropriate scope. ([TutorialsPoint][4])

---

# 6) Shallow-copy vs deep-copy gotchas (both apply)

Remember `Object.assign` performs a *shallow* copy. If your config or locals contain nested objects and you want true isolation, clone nested structures or use a deep merge/cloning utility. The prototype approach (`Object.create`) also inherits references — nested objects on the prototype are still shared unless explicitly shadowed.

---

# Quick rules-of-thumb (Express-focused)

* Add shared methods to `express.request`/`express.response` (prototype) — use prototype patterns / `Object.create` when creating custom instances or tests. ([Express][1])
* Merge configs, options, or per-request view data into plain objects — use `Object.assign` for shallow merges. ([TutorialsPoint][4])
* For testing middleware, create fake `req`/`res` from Express prototypes via `Object.create` so behavior matches real runtime. ([GitHub][2])

---

Want a small repo-style example that shows both patterns together (middleware factory that merges options with `Object.assign`, and a `req.ctx` created with `Object.create`)? I can drop a single file example you can run with `node` or convert to TypeScript. Which would you prefer?

[1]: https://expressjs.com/en/guide/overriding-express-api.html?utm_source=chatgpt.com "Overriding the Express API"
[2]: https://github.com/expressjs/express/issues/2812?utm_source=chatgpt.com "Decoupling express.request and express.response from ..."
[3]: https://www.geeksforgeeks.org/web-tech/express-js-res-locals-property/?utm_source=chatgpt.com "Express res.locals Property"
[4]: https://www.tutorialspoint.com/express-js-app-locals-property?utm_source=chatgpt.com "Express.js – app.locals Property"



