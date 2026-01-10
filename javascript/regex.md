## What a Regular Expression Is

A regular expression, or regex, is just a pattern for matching text.
You give it a rule, and it tells you whether a string matches that rule or which parts of the string match.

In JavaScript, regex is built in. You use it with strings or directly with the `RegExp` object.

---

## Basic Syntax

There are two ways to write regex in JS:

```js
const r1 = /hello/
const r2 = new RegExp("hello")
```

Most of the time, you’ll use the first one. It’s cleaner.

---

## Literal Characters

The simplest regex is plain text.

```js
/abc/
```

Matches:

* "abc"
* "123abc456"

Does not match:

* "ab"
* "acb"

Regex matches *anywhere* in the string by default.

---

## Common Metacharacters

These characters have special meaning.

| Symbol | Meaning              |    |
| ------ | -------------------- | -- |
| `.`    | any single character |    |
| `*`    | 0 or more            |    |
| `+`    | 1 or more            |    |
| `?`    | 0 or 1               |    |
| `      | `                    | OR |
| `()`   | grouping             |    |

Examples:

```js
/a.c/      // "abc", "a-c", "a c"
```

```js
/ab*/      // "a", "ab", "abbb"
```

```js
/ab+/      // "ab", "abbb" (not "a")
```

```js
/cat|dog/ // "cat" or "dog"
```

---

## Character Classes `[ ]`

Match **one character from a set**.

```js
/[abc]/    // "a" or "b" or "c"
```

```js
/[a-z]/    // any lowercase letter
```

```js
/[0-9]/    // any digit
```

```js
/[^0-9]/   // NOT a digit
```

Real-world example:

```js
/^[a-zA-Z0-9]+$/
```

Means: only letters and numbers, nothing else.

---

## Anchors `^` and `$`

These are huge in interviews.

| Anchor | Meaning         |
| ------ | --------------- |
| `^`    | start of string |
| `$`    | end of string   |

```js
/^hello/   // must start with "hello"
```

```js
/hello$/   // must end with "hello"
```

```js
/^hello$/ // must be exactly "hello"
```

Without anchors, regex matches anywhere.

---

## Predefined Character Sets

Shortcuts you’ll use all the time:

| Pattern | Meaning                      |
| ------- | ---------------------------- |
| `\d`    | digit (0–9)                  |
| `\w`    | word char (a-z, A-Z, 0-9, _) |
| `\s`    | whitespace                   |
| `\D`    | not a digit                  |
| `\W`    | not a word char              |
| `\S`    | not whitespace               |

```js
/\d+/      // one or more digits
```

---

## Quantifiers `{ }`

Control how many times something appears.

```js
/a{3}/     // "aaa"
```

```js
/a{2,4}/   // "aa", "aaa", "aaaa"
```

```js
/a{2,}/    // at least 2
```

---

## Escaping Special Characters

If a character has a special meaning, escape it with `\`.

```js
/\./   // matches a literal dot
```

```js
/\$/   // matches $
```

---

## Using Regex in JavaScript

Here’s how it’s actually used.

### `test()`

Returns true or false.

```js
/^\d+$/.test("123") // true
```

---

### `match()`

Extracts matches.

```js
"abc123".match(/\d+/)
// ["123"]
```

---

### `replace()`

Replace matched text.

```js
"hello 123".replace(/\d+/, "###")
// "hello ###"
```

---

### `split()`

Split using a pattern.

```js
"a,b;c".split(/[,;]/)
// ["a", "b", "c"]
```

---

## Flags

Flags change how regex behaves.

| Flag | Meaning              |
| ---- | -------------------- |
| `g`  | global (all matches) |
| `i`  | case-insensitive     |
| `m`  | multiline            |

```js
/hello/gi
```
