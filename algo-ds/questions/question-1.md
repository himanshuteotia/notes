**Easy–Medium level DSA questions** (closer to Medium in difficulty) with **full details** including:

### 1. **Move Zeroes**
✅ **Problem:** Move all `0`s to the end of the array while maintaining the order of non-zero elements.

📥 Input: `nums = [0, 1, 0, 3, 12]`  
📤 Output: `[1, 3, 12, 0, 0]`

🧠 Explanation: Shift non-zero elements forward, then fill the rest with zeros.

---

### 2. **Find All Duplicates in an Array**
✅ **Problem:** Given an integer array `nums` where elements appear either once or twice, return all elements that appear **twice**.

📥 Input: `nums = [4,3,2,7,8,2,3,1]`  
📤 Output: `[2,3]`

🧠 Explanation: Only 2 and 3 appear twice. You must do it in O(n) time and constant extra space.

---

### 3. **Remove Duplicates from Sorted Array**
✅ **Problem:** Given a sorted array, remove the duplicates **in-place**, returning the length of the unique part.

📥 Input: `nums = [1,1,2]`  
📤 Output: `2`  
➡️ Modified array: `[1,2,_]`

🧠 Explanation: After removing duplicates, the first two elements are 1 and 2.

---

### 4. **Backspace String Compare**
✅ **Problem:** Compare two strings where `#` means backspace.

📥 Input: `s = "ab#c", t = "ad#c"`  
📤 Output: `true`

🧠 Explanation: Both become `"ac"` after processing `#`.

---

### 5. **Merge Intervals**
✅ **Problem:** Merge overlapping intervals.

📥 Input: `intervals = [[1,3],[2,6],[8,10],[15,18]]`  
📤 Output: `[[1,6],[8,10],[15,18]]`

🧠 Explanation: `[1,3]` and `[2,6]` overlap and merge to `[1,6]`.

---

### 6. **Majority Element**
✅ **Problem:** Find the element that appears **more than ⌊n / 2⌋** times.

📥 Input: `nums = [3,2,3]`  
📤 Output: `3`

🧠 Explanation: 3 appears twice out of 3 elements — more than half.

---

### 7. **Best Time to Buy and Sell Stock**
✅ **Problem:** Find the max profit from buying and selling once.

📥 Input: `prices = [7,1,5,3,6,4]`  
📤 Output: `5` (Buy on day 2, sell on day 5)

🧠 Explanation: `6 - 1 = 5` is the max profit.

---

### 8. **Intersection of Two Arrays II**
✅ **Problem:** Find all common elements including duplicates.

📥 Input: `nums1 = [1,2,2,1], nums2 = [2,2]`  
📤 Output: `[2,2]`

🧠 Explanation: Two `2`s are common.

---

### 9. **Find Pivot Index**
✅ **Problem:** Return the index where the sum of numbers to the left and right are equal.

📥 Input: `nums = [1,7,3,6,5,6]`  
📤 Output: `3`  
🧠 Explanation: Left sum = 1+7+3 = 11, Right sum = 5+6 = 11

---

### 10. **Pascal's Triangle**
✅ **Problem:** Return the first `numRows` of Pascal's triangle.

📥 Input: `numRows = 5`  
📤 Output:  
```
[
 [1],
 [1,1],
 [1,2,1],
 [1,3,3,1],
 [1,4,6,4,1]
]
```

🧠 Explanation: Each row is built using values from the previous row.

---

### 11. **Check If N and Its Double Exist**  
✅ **Problem:** Check if the array contains `N` and `2*N`.

📥 Input: `arr = [10, 2, 5, 3]`  
📤 Output: `true`  
🧠 Explanation: 5 and 10 both exist (10 = 2 × 5).

---

### 12. **Third Maximum Number**  
✅ **Problem:** Return the 3rd largest unique number, or max if less than 3 unique.

📥 Input: `nums = [3, 2, 1]`  
📤 Output: `1`

📥 Input: `nums = [1, 2]`  
📤 Output: `2`

---

### 13. **Is Subsequence**  
✅ **Problem:** Check if `s` is a subsequence of `t`.

📥 Input: `s = "abc", t = "ahbgdc"`  
📤 Output: `true`

🧠 Explanation: a → h → b → g → d → c (order maintained).

---

### 14. **Find the Difference**  
✅ **Problem:** Given two strings `s` and `t` where `t` is `s` + one extra character, find the extra.

📥 Input: `s = "abcd", t = "abcde"`  
📤 Output: `"e"`

---

### 15. **Power of Three**  
✅ **Problem:** Return `true` if `n` is a power of 3.

📥 Input: `n = 27`  
📤 Output: `true`  
📥 Input: `n = 10`  
📤 Output: `false`

---

### 16. **Reverse Words in a String III**  
✅ **Problem:** Reverse each word in a sentence.

📥 Input: `"Let's take LeetCode contest"`  
📤 Output: `"s'teL ekat edoCteeL tsetnoc"`

---

### 17. **Assign Cookies**  
✅ **Problem:** Assign cookies to children to maximize satisfied kids. `greed` vs `cookie size`.

📥 Input: `g = [1,2,3], s = [1,1]`  
📤 Output: `1`

🧠 Explanation: Only one child with greed ≤ 1 can be satisfied.

---

### 18. **Binary Search**  
✅ **Problem:** Implement binary search.

📥 Input: `nums = [-1,0,3,5,9,12], target = 9`  
📤 Output: `4` (index of 9)

---

### 19. **Ransom Note**  
✅ **Problem:** Return `true` if `ransomNote` can be constructed using `magazine`.

📥 Input: `ransomNote = "aa", magazine = "aab"`  
📤 Output: `true`

---

### 20. **Detect Capital**  
✅ **Problem:** Return `true` if capital usage is correct:
- All caps: "USA"
- All lowercase: "leetcode"
- Only first letter capital: "Google"

📥 Input: `"FlaG"`  
📤 Output: `false`

---

### 21. **Valid Anagram**  
✅ **Problem:** Check if two strings are anagrams.

📥 Input: `s = "anagram", t = "nagaram"`  
📤 Output: `true`  
📥 Input: `s = "rat", t = "car"`  
📤 Output: `false`

---

### 22. **First Unique Character in a String**  
✅ **Problem:** Find the index of the first non-repeating character.

📥 Input: `"leetcode"`  
📤 Output: `0`  
📥 Input: `"loveleetcode"`  
📤 Output: `2`

---

### 23. **Maximum Subarray (Kadane's Algorithm)**  
✅ **Problem:** Find the contiguous subarray with the maximum sum.

📥 Input: `[-2,1,-3,4,-1,2,1,-5,4]`  
📤 Output: `6`  
🧠 Explanation: `[4,-1,2,1]` sums to 6

---

### 24. **Length of Last Word**  
✅ **Problem:** Return the length of the last word.

📥 Input: `"Hello World"`  
📤 Output: `5`  
📥 Input: `"   fly me   to   the moon  "`  
📤 Output: `4`

---

### 25. **Climbing Stairs**  
✅ **Problem:** You can climb 1 or 2 steps. How many distinct ways to reach the top?

📥 Input: `n = 3`  
📤 Output: `3`  
🧠 Explanation: (1+1+1), (1+2), (2+1)

---

### 26. **Plus One**  
✅ **Problem:** Add 1 to a number represented by an array of digits.

📥 Input: `[1,2,3]`  
📤 Output: `[1,2,4]`  
📥 Input: `[9,9,9]`  
📤 Output: `[1,0,0,0]`

---

### 27. **Valid Parentheses**  
✅ **Problem:** Check if the input string has valid open-close parentheses.

📥 Input: `"()"`  
📤 Output: `true`  
📥 Input: `"(]"`  
📤 Output: `false`

---

### 28. **Merge Sorted Array**  
✅ **Problem:** Merge two sorted arrays in-place.

📥 Input: `nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3`  
📤 Output: `[1,2,2,3,5,6]`

---

### 29. **Two Sum II – Input Array is Sorted**  
✅ **Problem:** Find indices of two numbers that add up to target.

📥 Input: `numbers = [2,7,11,15], target = 9`  
📤 Output: `[1,2]` (1-indexed)

---

### 30. **Longest Common Prefix**  
✅ **Problem:** Find the longest common prefix among an array of strings.

📥 Input: `["flower","flow","flight"]`  
📤 Output: `"fl"`  
📥 Input: `["dog","racecar","car"]`  
📤 Output: `""`

---

### 31. **Remove Duplicates from Sorted Array**  
✅ **Problem:** Remove duplicates in-place from a sorted array and return the new length.

📥 Input: `[1,1,2]`  
📤 Output: `2`  
🧠 Explanation: Array becomes `[1,2,_]`

---

### 32. **Majority Element**  
✅ **Problem:** Return the element that appears more than ⌊n/2⌋ times.

📥 Input: `[3,2,3]`  
📤 Output: `3`

---

### 33. **Move Zeroes**  
✅ **Problem:** Move all 0’s to the end while maintaining order of non-zero elements.

📥 Input: `[0,1,0,3,12]`  
📤 Output: `[1,3,12,0,0]`

---

### 34. **Best Time to Buy and Sell Stock**  
✅ **Problem:** Find max profit by buying and selling one time.

📥 Input: `[7,1,5,3,6,4]`  
📤 Output: `5`  
🧠 Buy at 1, sell at 6

---

### 35. **Single Number**  
✅ **Problem:** Every element appears twice except one. Find that one.

📥 Input: `[4,1,2,1,2]`  
📤 Output: `4`

---

### 36. **Intersection of Two Arrays II**  
✅ **Problem:** Return the intersection elements including duplicates.

📥 Input: `nums1 = [1,2,2,1], nums2 = [2,2]`  
📤 Output: `[2,2]`

---

### 37. **Valid Palindrome**  
✅ **Problem:** Check if the given string is a palindrome (ignore spaces and punctuation).

📥 Input: `"A man, a plan, a canal: Panama"`  
📤 Output: `true`

---

### 38. **Missing Number**  
✅ **Problem:** Find the missing number from range [0...n].

📥 Input: `[3,0,1]`  
📤 Output: `2`

---

### 39. **Find All Numbers Disappeared in an Array**  
✅ **Problem:** Find all numbers missing in range [1..n] (array size = n).

📥 Input: `[4,3,2,7,8,2,3,1]`  
📤 Output: `[5,6]`

---

### 40. **Relative Ranks**  
✅ **Problem:** Assign medals/ranks to athletes based on scores.

📥 Input: `[5,4,3,2,1]`  
📤 Output: `["Gold Medal","Silver Medal","Bronze Medal","4","5"]`

---

✅ The file **"Easy-Medium DSA Questions 40–50 with Full Details"** has been created.

Here are **DSA questions 41 to 50**:

---

### 41. **Fizz Buzz**  
✅ **Problem:** For a number n, return list of strings from 1 to n:  
- "Fizz" if divisible by 3  
- "Buzz" if divisible by 5  
- "FizzBuzz" if both

📥 Input: `n = 5`  
📤 Output: `["1", "2", "Fizz", "4", "Buzz"]`

---

### 42. **Power of Three**  
✅ **Problem:** Check if a number is a power of 3.

📥 Input: `27`  
📤 Output: `true`  
📥 Input: `0`  
📤 Output: `false`

---

### 43. **Roman to Integer**  
✅ **Problem:** Convert Roman numeral to integer.

📥 Input: `"III"`  
📤 Output: `3`  
📥 Input: `"LVIII"`  
📤 Output: `58`

---

### 44. **Add Binary**  
✅ **Problem:** Add two binary strings.

📥 Input: `a = "11", b = "1"`  
📤 Output: `"100"`

---

### 45. **Reverse Bits**  
✅ **Problem:** Reverse bits of a 32-bit unsigned integer.

📥 Input: `00000010100101000001111010011100`  
📤 Output: `964176192`  
🧠 Output in binary: `00111001011110000010100101000000`

---

### 46. **Hamming Distance**  
✅ **Problem:** Number of positions at which two bits are different.

📥 Input: `x = 1, y = 4`  
📤 Output: `2`  
🧠 Explanation:  
```
1 -> 0001  
4 -> 0100  
Diff: 2 bits
```

---

### 47. **Number of 1 Bits**  
✅ **Problem:** Return the number of 1 bits in binary form of a number.

📥 Input: `00000000000000000000000000001011`  
📤 Output: `3`

---

### 48. **Reverse String**  
✅ **Problem:** Reverse a character array in-place.

📥 Input: `["h","e","l","l","o"]`  
📤 Output: `["o","l","l","e","h"]`

---

### 49. **Find the Difference**  
✅ **Problem:** Given strings s and t, where t is s with one extra letter, find the letter.

📥 Input: `s = "abcd", t = "abcde"`  
📤 Output: `"e"`

---

### 50. **Island Perimeter**  
✅ **Problem:** Given a 2D grid map of 0s (water) and 1s (land), return perimeter of the island.

📥 Input:  
```
[[0,1,0,0],  
 [1,1,1,0],  
 [0,1,0,0],  
 [1,1,0,0]]
```  
📤 Output: `16`