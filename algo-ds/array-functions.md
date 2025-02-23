Here is a **comprehensive list of JavaScript Array functions** with examples and explanations:

---

## 📁 **1. Array Creation and Initialization**

| **Method**        | **Description**                         | **Example**                             |
|-------------------|-----------------------------------------|-----------------------------------------|
| `Array()`         | Creates a new array                     | `let arr = new Array(3);`              |
| `Array.of()`      | Creates array from arguments            | `Array.of(1, 2, 3); // [1, 2, 3]`      |
| `Array.from()`    | Creates array from iterable/array-like  | `Array.from("hello"); // ['h','e','l','l','o']` |
| `[...spread]`     | Clones or merges arrays                 | `[...arr1, ...arr2]`                    |

---

## 🔄 **2. Adding and Removing Elements**

| **Method**   | **Description**                      | **Example**                       |
|--------------|--------------------------------------|-----------------------------------|
| `push()`     | Adds to end                          | `arr.push(4);` → `[1, 2, 3, 4]`  |
| `pop()`      | Removes from end                     | `arr.pop();` → `[1, 2, 3]`       |
| `unshift()`  | Adds to start                        | `arr.unshift(0);` → `[0, 1, 2, 3]` |
| `shift()`    | Removes from start                   | `arr.shift();` → `[1, 2, 3]`     |
| `splice()`   | Adds/Removes at specific index       | `arr.splice(1, 1);` → removes 2nd element |
| `slice()`    | Returns subarray without modifying   | `arr.slice(1, 3);` → `[2, 3]`    |
| `concat()`   | Merges arrays                        | `arr.concat([4, 5]);` → `[1, 2, 3, 4, 5]` |
| `fill()`     | Fills array with static value        | `[1, 2, 3].fill(0);` → `[0, 0, 0]` |

---

## 🔍 **3. Searching and Filtering**

| **Method**      | **Description**                   | **Example**                        |
|-----------------|-----------------------------------|------------------------------------|
| `indexOf()`     | Finds first occurrence            | `arr.indexOf(3);` → `2`           |
| `lastIndexOf()` | Finds last occurrence             | `arr.lastIndexOf(3);`             |
| `includes()`    | Checks if value exists            | `arr.includes(3);` → `true`       |
| `find()`        | Returns first matching element    | `arr.find(x => x > 2);` → `3`     |
| `findIndex()`   | Returns index of first match      | `arr.findIndex(x => x > 2);` → `2` |
| `filter()`      | Returns filtered array            | `arr.filter(x => x > 2);` → `[3]` |

---

## 🧮 **4. Iteration and Transformation**

| **Method**     | **Description**                   | **Example**                            |
|----------------|-----------------------------------|----------------------------------------|
| `forEach()`    | Iterates over array               | `arr.forEach(x => console.log(x));`   |
| `map()`        | Returns new array after mapping   | `arr.map(x => x * 2);` → `[2, 4, 6]` |
| `reduce()`     | Reduces array to single value     | `arr.reduce((a, b) => a + b);` → `6` |
| `reduceRight()`| Same as reduce but right to left  | `arr.reduceRight((a, b) => a + b);`  |
| `some()`       | Returns true if any match exists  | `arr.some(x => x > 2);` → `true`     |
| `every()`      | Returns true if all match         | `arr.every(x => x < 4);` → `true`    |

---

## 📊 **5. Sorting and Reordering**

| **Method**    | **Description**                   | **Example**                          |
|---------------|-----------------------------------|--------------------------------------|
| `sort()`      | Sorts array (default lexicographic)| `[3, 1, 2].sort();` → `[1, 2, 3]`   |
| `reverse()`   | Reverses array                    | `[1, 2, 3].reverse();` → `[3, 2, 1]`|
| `flat()`      | Flattens nested arrays            | `[1, [2, 3]].flat();` → `[1, 2, 3]` |
| `flatMap()`   | Maps and flattens in one step     | `[1, 2].flatMap(x => [x, x * 2]);` → `[1, 2, 2, 4]`|

---

## 🔗 **6. Combining and Slicing**

| **Method**     | **Description**                    | **Example**                         |
|----------------|------------------------------------|-------------------------------------|
| `join()`       | Joins array into string            | `[1, 2, 3].join('-');` → `"1-2-3"`  |
| `split()`      | Splits string into array           | `'1-2-3'.split('-');` → `[1, 2, 3]`|
| `toString()`   | Converts array to string           | `[1, 2, 3].toString();` → `"1,2,3"`|
| `copyWithin()` | Copies part of array within itself | `[1, 2, 3, 4].copyWithin(1, 2);` → `[1, 3, 4, 4]`|

---

## 📊 **7. Utility Methods**

| **Method**         | **Description**                    | **Example**                        |
|--------------------|------------------------------------|------------------------------------|
| `Array.isArray()`   | Checks if value is an array        | `Array.isArray([1, 2, 3]);` → `true` |
| `length`           | Returns length of array            | `[1, 2, 3].length;` → `3`          |
| `keys()`           | Returns iterator of keys (indexes)  | `[1, 2].keys();` → `[0, 1]`        |
| `values()`         | Returns iterator of values         | `[1, 2].values();` → `[1, 2]`      |
| `entries()`        | Returns iterator of key/value pairs | `[1, 2].entries();` → `[[0,1],[1,2]]`|

---

## 🧾 **8. Example Snippets for Common Use-Cases**

#### ✅ **1. Sum of All Elements**
```javascript
let arr = [1, 2, 3, 4];
let sum = arr.reduce((acc, curr) => acc + curr, 0);
console.log(sum); // 10
```

#### ✅ **2. Remove Duplicates**
```javascript
let arr = [1, 2, 2, 3, 4, 4];
let unique = [...new Set(arr)];
console.log(unique); // [1, 2, 3, 4]
```

#### ✅ **3. Find Intersection of Two Arrays**
```javascript
let a = [1, 2, 3];
let b = [2, 3, 4];
let intersection = a.filter(x => b.includes(x));
console.log(intersection); // [2, 3]
```

#### ✅ **4. Flatten a Nested Array**
```javascript
let nestedArr = [1, [2, [3, 4]], 5];
let flatArr = nestedArr.flat(2);
console.log(flatArr); // [1, 2, 3, 4, 5]
```

#### ✅ **5. Count Occurrences of Elements**
```javascript
let arr = ['apple', 'banana', 'apple', 'orange'];
let count = arr.reduce((acc, fruit) => {
  acc[fruit] = (acc[fruit] || 0) + 1;
  return acc;
}, {});
console.log(count); // { apple: 2, banana: 1, orange: 1 }
```

---

## 🟢 **9. ECMAScript 6+ Additions to Arrays**
- **Spread Operator (`...`)** for cloning and merging arrays.
- **`Array.from()`** for creating arrays from iterables.
- **`Array.prototype.includes()`** for easier existence checks.
- **`Array.prototype.flat()`** and **`flatMap()`** for working with nested arrays.

---

This is a **complete reference** for working with **JavaScript Arrays**. 🚀 By mastering these methods, you can handle any data manipulation challenge with ease! 🧠