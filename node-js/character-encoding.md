Let’s walk through how your computer actually handles text at the lowest level.

---

### Step 1: You type characters

Let’s say you type:

```
Hello
```

On your keyboard, pressing `H` sends a signal (a key code) to the operating system.

---

### Step 2: Characters are mapped to **code points**

Every character has a numeric ID called a **Unicode code point**.

* `H` → U+0048
* `e` → U+0065
* `l` → U+006C
* `o` → U+006F

So `"Hello"` becomes this sequence:

```
U+0048 U+0065 U+006C U+006C U+006F
```

---

### Step 3: Code points are **encoded** into bytes

The computer doesn’t store "U+0048" literally — it stores raw bytes in memory or disk.
Encoding decides how to turn those numbers into bytes.

* In **UTF-8**:

  * `H (U+0048)` → `0x48` → 72
  * `e (U+0065)` → `0x65` → 101
  * `l (U+006C)` → `0x6C` → 108
  * `o (U+006F)` → `0x6F` → 111

So `"Hello"` in UTF-8 is stored as:

```
[72, 101, 108, 108, 111]
```

(5 bytes in memory).

---

### Step 4: Stored in memory or disk

At the hardware level, each byte is represented as **bits** (0s and 1s):

```
72   → 01001000
101  → 01100101
108  → 01101100
108  → 01101100
111  → 01101111
```

So `"Hello"` is ultimately:

```
01001000 01100101 01101100 01101100 01101111
```

---

### Step 5: Reading back

When you open the file or read the memory:

1. Computer reads the raw bits `01001000 …`.
2. Groups them into bytes.
3. Decodes bytes using the **encoding** (UTF-8, UTF-16, ASCII, etc.).
4. Converts them back to characters (H, e, l, l, o).
5. Displays text on your screen.

---

### Important thing:

* If you save `"नमस्ते"` (Hindi text), UTF-8 uses **3 bytes per character**, not 1. So the computer still stores just bytes, but more of them.
* If you read the same bytes with the **wrong encoding**, you’ll see gibberish. That’s why “mojibake” happens (weird characters showing up).

---

✅ So the flow is:
**Keyboard input → Unicode code point → Encoding → Bytes in memory/disk → Decoding → Characters on screen**

---
