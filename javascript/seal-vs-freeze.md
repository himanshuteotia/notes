Compare `Object.freeze()` and `Object.seal()` in JavaScript with simple words and examples 👇

---

### 🔒 `Object.freeze(obj)`

> Prevents **any changes** to the object.

#### ✅ What it does:

* ❌ Can't **add** new properties
* ❌ Can't **remove** existing properties
* ❌ Can't **change** existing values
* ❌ Can't change property attributes (`writable`, `configurable`, etc.)

#### Example:

```js
const user = {
  name: "Himanshu",
};

Object.freeze(user);
user.name = "Avik";     // ❌ No effect
user.age = 30;          // ❌ No effect
delete user.name;       // ❌ No effect

console.log(user);      // { name: 'Himanshu' }
```

---

### 🧯 `Object.seal(obj)`

> Allows value changes but **locks structure**.

#### ✅ What it does:

* ❌ Can't **add** new properties
* ❌ Can't **remove** properties
* ✅ **Can change** values of existing properties (if writable)

#### Example:

```js
const user = {
  name: "Himanshu",
};

Object.seal(user);
user.name = "Avik";     // ✅ Works
user.age = 30;          // ❌ Can't add
delete user.name;       // ❌ Can't delete

console.log(user);      // { name: 'Avik' }
```

---

### 🧠 Summary Table

| Feature               | `Object.freeze()` | `Object.seal()` |
| --------------------- | ----------------- | --------------- |
| Add new property      | ❌ No              | ❌ No            |
| Delete property       | ❌ No              | ❌ No            |
| Modify existing value | ❌ No              | ✅ Yes           |
| Make object immutable | ✅ Fully           | ⚠️ Partially    |

---

### 📌 Use Cases

* 🔐 `freeze()` → When you want a **fully locked** object (e.g., constants or configs).
* 🔐 `seal()` → When you want to **keep object structure fixed** but **allow value changes** (e.g., model instances).
