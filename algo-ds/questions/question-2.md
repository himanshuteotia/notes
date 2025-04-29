### 51. **Set Matrix Zeroes**  
✅ **Problem:** If an element is 0 in matrix, set its row and column to 0 (in-place).

📥 Input:  
```
[
 [1,1,1],
 [1,0,1],
 [1,1,1]
]
```
📤 Output:  
```
[
 [1,0,1],
 [0,0,0],
 [1,0,1]
]
```

---

### 52. **Spiral Matrix**  
✅ **Problem:** Return matrix elements in spiral order.

📥 Input:  
```
[
 [1,2,3],
 [4,5,6],
 [7,8,9]
]
```  
📤 Output: `[1,2,3,6,9,8,7,4,5]`

---

### 53. **Subarray Sum Equals K**  
✅ **Problem:** Find total number of continuous subarrays summing to `k`.

📥 Input: `nums = [1,2,3], k = 3`  
📤 Output: `2`  
🧠 Explanation: `[1,2]`, `[3]`

---

### 54. **Product of Array Except Self**  
✅ **Problem:** Return array where each element is product of all elements except itself (no division).

📥 Input: `[1,2,3,4]`  
📤 Output: `[24,12,8,6]`

---

### 55. **Longest Substring Without Repeating Characters**  
✅ **Problem:** Return length of longest substring with all unique characters.

📥 Input: `"abcabcbb"`  
📤 Output: `3` (`"abc"`)

---

### 56. **Group Anagrams**  
✅ **Problem:** Group words that are anagrams.

📥 Input: `["eat","tea","tan","ate","nat","bat"]`  
📤 Output: `[["eat","tea","ate"],["tan","nat"],["bat"]]`

---

### 57. **Minimum Window Substring**  
✅ **Problem:** Smallest substring of s with all characters of t.

📥 Input: `s = "ADOBECODEBANC", t = "ABC"`  
📤 Output: `"BANC"`

---

### 58. **3Sum**  
✅ **Problem:** Find all unique triplets that sum to 0.

📥 Input: `[-1,0,1,2,-1,-4]`  
📤 Output: `[[-1,-1,2],[-1,0,1]]`

---

### 59. **Sort Colors (Dutch National Flag)**  
✅ **Problem:** Sort array of 0s, 1s, 2s in-place.

📥 Input: `[2,0,2,1,1,0]`  
📤 Output: `[0,0,1,1,2,2]`

---

### 60. **Rotate Image (Matrix)**  
✅ **Problem:** Rotate NxN matrix 90 degrees clockwise (in-place).

📥 Input:  
```
[
 [1,2,3],
 [4,5,6],
 [7,8,9]
]
```  
📤 Output:  
```
[
 [7,4,1],
 [8,5,2],
 [9,6,3]
]
```
---

### 61. **Insert Interval**  
✅ **Problem:** Given a list of non-overlapping intervals sorted by start time, insert a new interval and merge if necessary.

📥 Input: `intervals = [[1,3],[6,9]], newInterval = [2,5]`  
📤 Output: `[[1,5],[6,9]]`

---

### 62. **Merge Intervals**  
✅ **Problem:** Merge all overlapping intervals.

📥 Input: `[[1,3],[2,6],[8,10],[15,18]]`  
📤 Output: `[[1,6],[8,10],[15,18]]`

---

### 63. **Jump Game**  
✅ **Problem:** Can you reach the last index?

📥 Input: `nums = [2,3,1,1,4]`  
📤 Output: `true`  
🧠 Reason: You can reach index 4 via 0 → 1 → 4

---

### 64. **Coin Change**  
✅ **Problem:** Minimum coins to make amount. Return -1 if not possible.

📥 Input: `coins = [1,2,5], amount = 11`  
📤 Output: `3` (`5+5+1`)

---

### 65. **Maximum Subarray (Kadane's Algorithm)**  
✅ **Problem:** Find the contiguous subarray with the maximum sum.

📥 Input: `[-2,1,-3,4,-1,2,1,-5,4]`  
📤 Output: `6`  
🧠 Subarray: `[4,-1,2,1]`

---

### 66. **Longest Palindromic Substring**  
✅ **Problem:** Return longest palindromic substring.

📥 Input: `"babad"`  
📤 Output: `"bab"` or `"aba"`

---

### 67. **Unique Paths**  
✅ **Problem:** In m x n grid, how many unique paths from top-left to bottom-right (only right or down).

📥 Input: `m = 3, n = 7`  
📤 Output: `28`

---

### 68. **Climbing Stairs**  
✅ **Problem:** You can climb 1 or 2 steps. How many distinct ways to climb `n` stairs?

📥 Input: `n = 3`  
📤 Output: `3`  
🧠 Ways: `1+1+1`, `1+2`, `2+1`

---

### 69. **Letter Combinations of a Phone Number**  
✅ **Problem:** Return all letter combinations that the number could represent.

📥 Input: `"23"`  
📤 Output: `["ad","ae","af","bd","be","bf","cd","ce","cf"]`

---

### 70. **Word Search**  
✅ **Problem:** Given a grid of characters, check if a word exists by searching adjacent letters (used once only).

📥 Input: `board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"`  
📤 Output: `true`

---