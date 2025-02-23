Here is a **comprehensive list of JavaScript String methods** with examples and explanations:

---

## 📁 **1. String Creation and Initialization**

| **Method**        | **Description**                         | **Example**                             |
|-------------------|-----------------------------------------|-----------------------------------------|
| `String()`        | Creates a new string object             | `let str = new String("Hello");`       |
| String Literal    | Direct initialization                   | `let str = "Hello";`                   |
| Template Literal  | Embeds variables in string using backticks | ``let str = `Hello, ${name}`;``     |

---

## 🔄 **2. String Access and Information**

| **Method/Property** | **Description**                     | **Example**                           |
|---------------------|-------------------------------------|---------------------------------------|
| `length`            | Returns length of string            | `"hello".length;` → `5`              |
| `charAt()`          | Returns character at specific index  | `"hello".charAt(1);` → `'e'`         |
| `charCodeAt()`      | Returns Unicode of character         | `"A".charCodeAt(0);` → `65`          |
| `codePointAt()`     | Returns Unicode code point           | `"😊".codePointAt(0);` → `128522`    |
| `at()`              | Returns character at index (ES2022)  | `"hello".at(-1);` → `'o'`            |

---

## ✂️ **3. String Extraction and Slicing**

| **Method**      | **Description**                          | **Example**                          |
|-----------------|------------------------------------------|--------------------------------------|
| `slice()`       | Extracts part of string                  | `"hello".slice(1, 4);` → `'ell'`    |
| `substring()`   | Similar to slice, but negative values treated as 0 | `"hello".substring(1, 4);` → `'ell'` |
| `substr()`      | Extracts from start index, length chars   | `"hello".substr(1, 3);` → `'ell'`   |

---

## 🔍 **4. String Searching and Matching**

| **Method**      | **Description**                          | **Example**                          |
|-----------------|------------------------------------------|--------------------------------------|
| `indexOf()`     | Finds index of first occurrence          | `"hello".indexOf('l');` → `2`       |
| `lastIndexOf()` | Finds index of last occurrence           | `"hello".lastIndexOf('l');` → `3`   |
| `includes()`    | Checks if substring exists               | `"hello".includes('ell');` → `true` |
| `startsWith()`  | Checks if string starts with substring   | `"hello".startsWith('he');` → `true`|
| `endsWith()`    | Checks if string ends with substring     | `"hello".endsWith('lo');` → `true`  |
| `match()`       | Matches regex and returns result         | `"hello".match(/l/g);` → `['l', 'l']`|
| `matchAll()`    | Returns iterator of all regex matches    | `[... "hello".matchAll(/l/g)];` → `['l', 'l']`|
| `search()`      | Returns index of regex match             | `"hello".search(/l/);` → `2`        |

---

## 🔄 **5. String Manipulation**

| **Method**      | **Description**                          | **Example**                          |
|-----------------|------------------------------------------|--------------------------------------|
| `toUpperCase()` | Converts to uppercase                    | `"hello".toUpperCase();` → `'HELLO'`|
| `toLowerCase()` | Converts to lowercase                    | `"HELLO".toLowerCase();` → `'hello'`|
| `trim()`        | Removes whitespace from both ends        | `"  hello  ".trim();` → `'hello'`   |
| `trimStart()`   | Removes whitespace from start            | `"  hello".trimStart();` → `'hello'`|
| `trimEnd()`     | Removes whitespace from end              | `"hello  ".trimEnd();` → `'hello'`  |
| `padStart()`    | Pads start to reach desired length       | `"5".padStart(3, '0');` → `'005'`   |
| `padEnd()`      | Pads end to reach desired length         | `"5".padEnd(3, '0');` → `'500'`     |
| `repeat()`      | Repeats string N times                   | `"ha".repeat(3);` → `'hahaha'`      |
| `replace()`     | Replaces first match                     | `"hello".replace('l', 'L');` → `'heLlo'`|
| `replaceAll()`  | Replaces all matches                      | `"hello".replaceAll('l', 'L');` → `'heLLo'`|
| `split()`       | Splits string into array                 | `"a,b,c".split(',');` → `['a', 'b', 'c']`|

---

## 🧮 **6. String Comparison and Concatenation**

| **Method**      | **Description**                          | **Example**                          |
|-----------------|------------------------------------------|--------------------------------------|
| `localeCompare()` | Compares two strings based on locale   | `"a".localeCompare("b");` → `-1`    |
| `concat()`      | Concatenates two strings                 | `"Hello".concat(" World");` → `'Hello World'`|
| `+` Operator    | Joins strings                            | `"Hello" + " World";` → `'Hello World'`|
| Template Literal| Concatenates using backticks             | `` `Hello ${name}` `` → `'Hello John'`|

---

## 🔗 **7. Utility Methods**

| **Method**      | **Description**                          | **Example**                          |
|-----------------|------------------------------------------|--------------------------------------|
| `toString()`    | Converts to string                       | `(123).toString();` → `'123'`       |
| `valueOf()`     | Returns primitive value                  | `str.valueOf();` → `'hello'`        |
| `normalize()`   | Normalizes Unicode strings               | `"\u0041\u006d\u00e9".normalize();` → `'Amé'`|
| `charCodeAt()`  | Returns Unicode of character             | `"A".charCodeAt(0);` → `65`         |
| `fromCharCode()`| Creates string from Unicode code         | `String.fromCharCode(65);` → `'A'`  |
| `codePointAt()` | Handles emojis and special characters    | `"😊".codePointAt(0);` → `128522`    |

---

## 🧾 **8. Example Snippets for Common Use-Cases**

#### ✅ **1. Reverse a String**
```javascript
let str = "hello";
let reversed = str.split('').reverse().join('');
console.log(reversed); // 'olleh'
```

---

#### ✅ **2. Count Occurrences of a Character**
```javascript
let str = "hello world";
let count = str.split('l').length - 1;
console.log(count); // 3
```

---

#### ✅ **3. Palindrome Checker**
```javascript
function isPalindrome(str) {
  let cleaned = str.toLowerCase().replace(/[^a-z0-9]/g, '');
  return cleaned === cleaned.split('').reverse().join('');
}
console.log(isPalindrome("A man, a plan, a canal: Panama")); // true
```

---

#### ✅ **4. Capitalize First Letter of Each Word**
```javascript
let str = "hello world";
let capitalized = str.split(' ')
                      .map(word => word.charAt(0).toUpperCase() + word.slice(1))
                      .join(' ');
console.log(capitalized); // "Hello World"
```

---

#### ✅ **5. Remove Duplicates from String**
```javascript
let str = "aabbccdde";
let unique = [...new Set(str)].join('');
console.log(unique); // "abcde"
```

---

#### ✅ **6. Check for Anagrams**
```javascript
function isAnagram(str1, str2) {
  return str1.split('').sort().join('') === str2.split('').sort().join('');
}
console.log(isAnagram("listen", "silent")); // true
```

---

#### ✅ **7. Extract Numbers from String**
```javascript
let str = "Order #1234 and #5678";
let numbers = str.match(/\d+/g);
console.log(numbers); // ["1234", "5678"]
```

---

## 🟢 **9. ECMAScript 6+ Additions to Strings**
- **Template Literals** for interpolation and multiline strings:
  ```javascript
  let name = "John";
  console.log(`Hello, ${name}`); // Hello, John
  ```

- **String Padding** (`padStart()` and `padEnd()`):
  ```javascript
  console.log("5".padStart(3, '0')); // 005
  ```

- **`startsWith()`, `endsWith()`, and `includes()`**:
  ```javascript
  console.log("hello".startsWith("he")); // true
  console.log("hello".includes("ll"));   // true
  ```

- **`replaceAll()`** (ES2021):
  ```javascript
  let str = "hello hello";
  console.log(str.replaceAll("hello", "hi")); // "hi hi"
  ```

---

This is a **complete reference** for working with **JavaScript Strings**. 🚀 Mastering these methods will allow you to handle any string manipulation task with ease and write cleaner, more efficient code! 🧠