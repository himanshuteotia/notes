# 🟢 **Understanding `Set()` in JavaScript**

The **`Set`** object in JavaScript is a **built-in data structure** that stores **unique values** of any type — whether primitive values or object references.

```javascript
const mySet = new Set();
mySet.add(1);
mySet.add(2);
mySet.add(2); // Duplicate value is ignored
console.log(mySet); // Output: Set(2) {1, 2}
```

- **Duplicates are not allowed**.
- **Order is insertion-based**.
- Elements can be of any type (primitive or objects).

---

## 📁 **1. Syntax**

```javascript
// Creating a new Set
const set = new Set();

// Initialize with values
const numbers = new Set([1, 2, 3, 4]);
console.log(numbers); // Set(4) {1, 2, 3, 4}
```

---

## ⚙️ **2. Common Methods and Properties of `Set()`**

| **Method/Property**  | **Description**                              | **Example**                                 |
|----------------------|----------------------------------------------|---------------------------------------------|
| `add(value)`         | Adds a value to the set                      | `mySet.add(5);`                             |
| `delete(value)`      | Removes a value from the set                 | `mySet.delete(2);`                          |
| `has(value)`         | Checks if value exists                       | `mySet.has(1); // true`                     |
| `clear()`            | Removes all elements                         | `mySet.clear();`                            |
| `size`               | Returns the number of elements               | `mySet.size; // 2`                          |
| `forEach(callback)`  | Iterates over set elements                   | `mySet.forEach(v => console.log(v));`       |
| `[...mySet]`         | Spread syntax to convert to array            | `let arr = [...mySet];`                     |
| `Array.from(set)`    | Another way to convert set to array          | `Array.from(mySet);`                        |

---

### ✅ **Example: Basic Usage**

```javascript
const mySet = new Set([1, 2, 3, 4]);

mySet.add(5);          // Set(5) {1, 2, 3, 4, 5}
mySet.delete(3);       // Set(4) {1, 2, 4, 5}
console.log(mySet.has(2)); // true
console.log(mySet.size);   // 4

// Convert to array
const arr = [...mySet];
console.log(arr); // [1, 2, 4, 5]
```

---

## 📊 **3. When to Use `Set()` in Algorithms and Data Structures?**

`Set` is useful in scenarios where you want to **maintain uniqueness** and achieve **faster lookups** compared to arrays.

---

### 📌 **1. Removing Duplicates from an Array**

```javascript
let arr = [1, 2, 2, 3, 4, 4, 5];
let uniqueArr = [...new Set(arr)];
console.log(uniqueArr); // [1, 2, 3, 4, 5]
```

- **Time Complexity**:  
  - Insertion & Deletion → `O(1)` (average case)  
  - Array iteration → `O(n)`

---

### 📌 **2. Set Operations: Union, Intersection, and Difference**

#### 🔷 **Union** (Combine two sets without duplicates)

```javascript
let setA = new Set([1, 2, 3]);
let setB = new Set([3, 4, 5]);

let union = new Set([...setA, ...setB]);
console.log(union); // Set(5) {1, 2, 3, 4, 5}
```

#### 🔷 **Intersection** (Common elements between sets)

```javascript
let intersection = new Set([...setA].filter(x => setB.has(x)));
console.log(intersection); // Set(1) {3}
```

#### 🔷 **Difference** (Elements in `setA` but not in `setB`)

```javascript
let difference = new Set([...setA].filter(x => !setB.has(x)));
console.log(difference); // Set(2) {1, 2}
```

---

### 📌 **3. Checking for Duplicates in an Array**

```javascript
function hasDuplicates(arr) {
  return new Set(arr).size !== arr.length;
}

console.log(hasDuplicates([1, 2, 3, 4])); // false
console.log(hasDuplicates([1, 2, 2, 4])); // true
```

- **Why Set?**: Set ignores duplicates, so comparing its size with the original array helps detect duplicates.

---

### 📌 **4. Finding Unique Elements Between Two Arrays**

```javascript
let arr1 = [1, 2, 3, 4];
let arr2 = [3, 4, 5, 6];

let set1 = new Set(arr1);
let set2 = new Set(arr2);

let uniqueElements = [...set1].filter(x => !set2.has(x));
console.log(uniqueElements); // [1, 2]
```

---

### 📌 **5. Efficient Search and Lookup Operations**

```javascript
let words = ['apple', 'banana', 'cherry', 'apple'];
let wordSet = new Set(words);

console.log(wordSet.has('banana')); // true
console.log(wordSet.has('grape'));  // false
```

- Faster than using `.includes()` on arrays (`O(1)` vs. `O(n)`).

---

### 📌 **6. Counting Distinct Elements (Leetcode-Style Problem)**

**Problem:** Given an array, count the number of distinct elements.

```javascript
function countDistinct(arr) {
  return new Set(arr).size;
}

console.log(countDistinct([1, 2, 2, 3, 4, 4, 5])); // 5
```

---

## 📊 **4. Performance in Algorithmic Context**

| **Operation**       | **Array**  | **Set**    |
|---------------------|------------|------------|
| **Add Element**      | O(1) / O(n)| O(1)       |
| **Delete Element**   | O(n)       | O(1)       |
| **Check Existence**  | O(n)       | O(1)       |
| **Iterate Elements** | O(n)       | O(n)       |

---

## ⚖️ **5. Set vs. Map vs. Array**

| **Feature**          | **Set**       | **Map**            | **Array**       |
|----------------------|---------------|--------------------|-----------------|
| Unique Values        | ✅             | ❌ (keys must be unique) | ❌         |
| Key-Value Pairs       | ❌             | ✅                  | ❌             |
| Order Maintained      | ✅ (insertion order) | ✅ (insertion order) | ✅     |
| Performance (Search)  | Fast (O(1))   | Fast (O(1))        | Slow (O(n))    |
| Use Case             | Unique values | Key-value mapping  | Ordered data   |

---

## 💡 **6. Common Algorithm Problems Using `Set()`**

### 🟢 **1. Find Intersection of Two Arrays (Leetcode #349)**
```javascript
function intersection(nums1, nums2) {
  let set1 = new Set(nums1);
  return [...new Set(nums2.filter(n => set1.has(n)))];
}

console.log(intersection([1, 2, 2, 1], [2, 2])); // [2]
```

---

### 🟢 **2. Happy Number Problem (Leetcode #202)**
Detect loops in a number transformation process using a `Set`.

```javascript
function isHappy(n) {
  let seen = new Set();
  while (n !== 1 && !seen.has(n)) {
    seen.add(n);
    n = n.toString().split('').reduce((sum, num) => sum + Math.pow(Number(num), 2), 0);
  }
  return n === 1;
}

console.log(isHappy(19)); // true
```

---

### 🟢 **3. Longest Consecutive Sequence (Leetcode #128)**
```javascript
function longestConsecutive(nums) {
  let set = new Set(nums);
  let max = 0;

  for (let num of set) {
    if (!set.has(num - 1)) { // Start of a sequence
      let currentNum = num;
      let streak = 1;

      while (set.has(currentNum + 1)) {
        currentNum++;
        streak++;
      }

      max = Math.max(max, streak);
    }
  }
  return max;
}

console.log(longestConsecutive([100, 4, 200, 1, 3, 2])); // 4 (1, 2, 3, 4)
```

---

## 🧠 **7. When to Use `Set()` in Data Structures & Algorithms?**

| **Use Case**                          | **Why Set?**                  |
|---------------------------------------|--------------------------------|
| **Removing duplicates**               | Automatically enforces uniqueness |
| **Tracking visited nodes** (Graph/DFS/BFS) | Fast lookups                  |
| **Finding common or unique elements** | Set operations like union, intersection |
| **Cycle detection in Linked Lists/Graphs** | Efficient cycle detection     |
| **Counting distinct elements**        | Size property gives O(1) count |
| **Subset/Power Set problems**         | Easy set combinations         |

---

## 🚀 **8. Key Takeaways**

- **`Set()` is optimized for uniqueness and fast lookups (`O(1)`).**
- Ideal for **de-duplication, membership checks,** and **set operations** (union, intersection, difference).
- Frequently used in **graph algorithms**, **array problems**, and **data filtering**.

---

With these insights, you can effectively use **`Set()`** in JavaScript for solving **algorithmic problems** and **data structure challenges** efficiently. 🚀

Here are **10 advanced algorithmic problems** involving **`Set()` in JavaScript**, along with **detailed analysis** on **time complexity**, **space complexity**, and when to use **`Set()`**.

---

# 📊 **1. Two Sum (Leetcode #1) – Using Set for Fast Lookups**

**Problem:**  
Given an array of integers, return **true** if there are any two numbers that sum up to a specific target.

### ✅ **Solution Using `Set()`**
```javascript
function hasPairWithSum(arr, target) {
  let seen = new Set();

  for (let num of arr) {
    if (seen.has(target - num)) {
      return true;
    }
    seen.add(num);
  }
  return false;
}

console.log(hasPairWithSum([1, 2, 4, 9], 8)); // false
console.log(hasPairWithSum([1, 2, 4, 4], 8)); // true
```

### 📊 **Analysis:**
- **Time Complexity:** `O(n)` — Iterates once through the array.
- **Space Complexity:** `O(n)` — Stores up to `n` elements in the Set.
- **Why Use Set?**  
  Efficient lookup for complements using **`O(1)`** average time.

---

# 📊 **2. Longest Substring Without Repeating Characters (Leetcode #3)**

**Problem:**  
Given a string, find the length of the longest substring without repeating characters.

### ✅ **Solution Using `Set()`**
```javascript
function lengthOfLongestSubstring(s) {
  let set = new Set();
  let left = 0;
  let maxLen = 0;

  for (let right = 0; right < s.length; right++) {
    while (set.has(s[right])) {
      set.delete(s[left]);
      left++;
    }
    set.add(s[right]);
    maxLen = Math.max(maxLen, right - left + 1);
  }

  return maxLen;
}

console.log(lengthOfLongestSubstring("abcabcbb")); // 3 ("abc")
console.log(lengthOfLongestSubstring("bbbbb"));    // 1 ("b")
```

### 📊 **Analysis:**
- **Time Complexity:** `O(n)` — Each character is visited at most twice.
- **Space Complexity:** `O(n)` — Stores up to `n` characters in Set.
- **Why Use Set?**  
  Fast existence check for duplicate characters.

---

# 📊 **3. Intersection of Two Arrays (Leetcode #349)**

**Problem:**  
Return an array of unique elements found in both input arrays.

### ✅ **Solution Using `Set()`**
```javascript
function intersection(nums1, nums2) {
  let set1 = new Set(nums1);
  let result = new Set();

  for (let num of nums2) {
    if (set1.has(num)) {
      result.add(num);
    }
  }

  return [...result];
}

console.log(intersection([1, 2, 2, 1], [2, 2])); // [2]
```

### 📊 **Analysis:**
- **Time Complexity:** `O(m + n)` — `m` for Set creation, `n` for lookup.
- **Space Complexity:** `O(min(m, n))` — For the result Set.
- **Why Use Set?**  
  Simplifies the logic for uniqueness and faster lookups.

---

# 📊 **4. First Non-Repeating Character in a String**

**Problem:**  
Return the index of the first non-repeating character in a string.

### ✅ **Solution Using `Set()` and Map**
```javascript
function firstUniqChar(s) {
  let count = new Map();

  for (let char of s) {
    count.set(char, (count.get(char) || 0) + 1);
  }

  for (let i = 0; i < s.length; i++) {
    if (count.get(s[i]) === 1) {
      return i;
    }
  }

  return -1;
}

console.log(firstUniqChar("leetcode")); // 0
console.log(firstUniqChar("loveleetcode")); // 2
```

### 📊 **Analysis:**
- **Time Complexity:** `O(n)` — Two passes over the string.
- **Space Complexity:** `O(n)` — For the Map.
- **Why Use Set/Map?**  
  To count occurrences efficiently.

---

# 📊 **5. Happy Number (Leetcode #202)**

**Problem:**  
A **happy number** is a number that leads to 1 after a sequence of steps where each step replaces the number by the sum of squares of its digits. Determine if a number is happy.

### ✅ **Solution Using `Set()`**
```javascript
function isHappy(n) {
  let seen = new Set();

  while (n !== 1 && !seen.has(n)) {
    seen.add(n);
    n = n.toString().split('').reduce((sum, digit) => sum + Math.pow(Number(digit), 2), 0);
  }

  return n === 1;
}

console.log(isHappy(19)); // true
console.log(isHappy(2));  // false
```

### 📊 **Analysis:**
- **Time Complexity:** `O(log n)` — Because number size reduces over iterations.
- **Space Complexity:** `O(log n)` — Stores previous states to detect cycles.
- **Why Use Set?**  
  To detect cycles and avoid infinite loops.

---

# 📊 **6. Longest Consecutive Sequence (Leetcode #128)**

**Problem:**  
Given an unsorted array, find the length of the longest consecutive sequence.

### ✅ **Solution Using `Set()`**
```javascript
function longestConsecutive(nums) {
  let numSet = new Set(nums);
  let longest = 0;

  for (let num of numSet) {
    if (!numSet.has(num - 1)) { // Start of sequence
      let currentNum = num;
      let streak = 1;

      while (numSet.has(currentNum + 1)) {
        currentNum++;
        streak++;
      }

      longest = Math.max(longest, streak);
    }
  }

  return longest;
}

console.log(longestConsecutive([100, 4, 200, 1, 3, 2])); // 4 (1, 2, 3, 4)
```

### 📊 **Analysis:**
- **Time Complexity:** `O(n)` — Each number is checked once.
- **Space Complexity:** `O(n)` — Stores all numbers in the Set.
- **Why Use Set?**  
  Quick lookups to check for the start of sequences.

---

# 📊 **7. Unique Email Addresses (Leetcode #929)**

**Problem:**  
Given a list of emails, count how many unique addresses exist, ignoring dots and plus signs in the local part.

### ✅ **Solution Using `Set()`**
```javascript
function numUniqueEmails(emails) {
  let set = new Set();

  for (let email of emails) {
    let [local, domain] = email.split('@');
    local = local.split('+')[0].replace(/\./g, '');
    set.add(`${local}@${domain}`);
  }

  return set.size;
}

console.log(numUniqueEmails(["test.email+alex@leetcode.com", "test.e.mail@leetcode.com"])); // 1
```

### 📊 **Analysis:**
- **Time Complexity:** `O(n * m)` — Where `n` is number of emails and `m` is email length.
- **Space Complexity:** `O(n)` — Stores unique emails.
- **Why Use Set?**  
  To store unique normalized emails.

---

# 📊 **8. Number of Islands (Leetcode #200) – Using Set for Visited Nodes**

**Problem:**  
Given a grid of '1's (land) and '0's (water), count the number of islands.

### ✅ **Solution Using DFS and Set**
```javascript
function numIslands(grid) {
  let rows = grid.length, cols = grid[0].length;
  let visited = new Set();
  let count = 0;

  function dfs(r, c) {
    if (r < 0 || c < 0 || r >= rows || c >= cols || grid[r][c] === '0' || visited.has(`${r},${c}`)) {
      return;
    }

    visited.add(`${r},${c}`);
    dfs(r + 1, c);
    dfs(r - 1, c);
    dfs(r, c + 1);
    dfs(r, c - 1);
  }

  for (let r = 0; r < rows; r++) {
    for (let c = 0; c < cols; c++) {
      if (grid[r][c] === '1' && !visited.has(`${r},${c}`)) {
        dfs(r, c);
        count++;
      }
    }
  }

  return count;
}

console.log(numIslands([
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
])); // 3
```

### 📊 **Analysis:**
- **Time Complexity:** `O(m * n)` — Each cell is visited once.
- **Space Complexity:** `O(m * n)` — Stores visited cells in Set.
- **Why Use Set?**  
  To keep track of visited cells efficiently.

---

# 📊 **9. Word Ladder II (Leetcode #126) – BFS with Set**

**Problem:**  
Given two words (beginWord and endWord) and a dictionary, return all shortest transformation sequences.

### ✅ **Why Use Set?**  
- To **quickly check valid words** during BFS.
- Prevents revisiting words, avoiding cycles.

---

# 📊 **10. Subarray Sum Equals K (Leetcode #560)**

**Problem:**  
Count the number of continuous subarrays whose sum equals `k`.

### ✅ **Solution Using Prefix Sum and Set**
```javascript
function subarraySum(nums, k) {
  let count = 0, sum = 0;
  let prefixSum = new Map();
  prefixSum.set(0, 1);

  for (let num of nums) {
    sum += num;
    if (prefixSum.has(sum - k)) {
      count += prefixSum.get(sum - k);
    }
    prefixSum.set(sum, (prefixSum.get(sum) || 0) + 1);
  }

  return count;
}

console.log(subarraySum([1, 1, 1], 2)); // 2 ([1,1] twice)
```

### 📊 **Analysis:**
- **Time Complexity:** `O(n)`
- **Space Complexity:** `O(n)`
- **Why Use Set/Map?**  
  For efficient prefix sum lookups.

---

# 💡 **Key Takeaways for Using `Set()` in Algorithms:**

| **Problem Type**                       | **Use Case for Set**                      |
|----------------------------------------|-------------------------------------------|
| **Duplicates Detection**               | Fast O(1) lookup to avoid repeats        |
| **Cycle Detection** (Graphs, Linked Lists) | Track visited nodes/states             |
| **Membership Checks**                  | Quick O(1) existence check               |
| **Uniqueness Requirements**            | Storing unique values                    |
| **Intersection/Union/Difference**      | Set operations in constant time          |
| **Backtracking & Recursion**           | Avoid revisiting states                  |

---

By mastering these **`Set()`-based algorithms**, you'll be well-prepared to handle **interview problems** and optimize solutions that require **efficient lookups** and **unique value tracking**. 🚀