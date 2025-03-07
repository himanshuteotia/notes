Sure! Let's start with **Tree Algorithms** from the basics to advanced concepts.

---

# **1. Introduction to Trees**
A **tree** is a hierarchical data structure consisting of **nodes**, where each node has:
- A **value** (data)
- References to **child nodes**
- A single **parent node** (except the root)

## **Types of Trees**
1. **Binary Tree** → Each node has at most **two** children.
2. **Binary Search Tree (BST)** → A binary tree where:
   - Left child < Parent
   - Right child > Parent
3. **Balanced Tree (AVL, Red-Black)** → Ensures height balance.
4. **Heap (Min Heap, Max Heap)** → Used in priority queues.
5. **Trie (Prefix Tree)** → Used for string operations.
6. **Segment Tree & Fenwick Tree** → Used for range queries.

---

# **2. Basic Tree Traversals**
### **2.1 Depth-First Search (DFS)**
DFS explores as far as possible along a branch before backtracking.

#### **Types of DFS Traversals**
1. **Preorder (Root → Left → Right)**
2. **Inorder (Left → Root → Right)**
3. **Postorder (Left → Right → Root)**

### **Implementation (Recursive)**
```javascript
class TreeNode {
  constructor(val) {
    this.val = val;
    this.left = null;
    this.right = null;
  }
}

// Preorder Traversal (Root → Left → Right)
function preorderTraversal(root) {
  if (!root) return;
  console.log(root.val);
  preorderTraversal(root.left);
  preorderTraversal(root.right);
}

// Inorder Traversal (Left → Root → Right)
function inorderTraversal(root) {
  if (!root) return;
  inorderTraversal(root.left);
  console.log(root.val);
  inorderTraversal(root.right);
}

// Postorder Traversal (Left → Right → Root)
function postorderTraversal(root) {
  if (!root) return;
  postorderTraversal(root.left);
  postorderTraversal(root.right);
  console.log(root.val);
}
```

---

### **2.2 Breadth-First Search (BFS) (Level Order Traversal)**
BFS explores each level before moving to the next level.

### **Implementation (Using Queue)**
```javascript
function levelOrderTraversal(root) {
  if (!root) return;
  
  let queue = [root];
  
  while (queue.length > 0) {
    let node = queue.shift();
    console.log(node.val);
    
    if (node.left) queue.push(node.left);
    if (node.right) queue.push(node.right);
  }
}
```

---

# **3. Common Tree Algorithms**
### **3.1 Finding Maximum Depth (Height) of a Tree**
The height of a tree is the number of edges in the longest path from root to a leaf.

**Recursive Approach:**
```javascript
function maxDepth(root) {
  if (!root) return 0;
  return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}
```

---

### **3.2 Checking if a Tree is Balanced**
A tree is **balanced** if the height difference between the left and right subtree is ≤ 1.

```javascript
function isBalanced(root) {
  function height(node) {
    if (!node) return 0;
    let leftHeight = height(node.left);
    let rightHeight = height(node.right);
    
    if (Math.abs(leftHeight - rightHeight) > 1) return -1; // Unbalanced
    
    return 1 + Math.max(leftHeight, rightHeight);
  }

  return height(root) !== -1;
}
```

---

### **3.3 Lowest Common Ancestor (LCA)**
Find the lowest node that is an ancestor of both nodes **p** and **q**.

```javascript
function lowestCommonAncestor(root, p, q) {
  if (!root || root === p || root === q) return root;
  
  let left = lowestCommonAncestor(root.left, p, q);
  let right = lowestCommonAncestor(root.right, p, q);
  
  if (left && right) return root;
  
  return left ? left : right;
}
```

---

### **3.4 Diameter of a Binary Tree**
The **diameter** of a tree is the longest path between any two nodes.

```javascript
function diameterOfBinaryTree(root) {
  let diameter = 0;

  function dfs(node) {
    if (!node) return 0;
    let left = dfs(node.left);
    let right = dfs(node.right);
    diameter = Math.max(diameter, left + right);
    return 1 + Math.max(left, right);
  }

  dfs(root);
  return diameter;
}
```

---

### **3.5 Validate a Binary Search Tree (BST)**
Check if a tree follows BST rules: left < root < right.

```javascript
function isValidBST(root, min = null, max = null) {
  if (!root) return true;
  if ((min !== null && root.val <= min) || (max !== null && root.val >= max)) return false;
  
  return isValidBST(root.left, min, root.val) && isValidBST(root.right, root.val, max);
}
```

---

### **3.6 Serialize and Deserialize a Binary Tree**
Convert a tree into a string and back.

```javascript
function serialize(root) {
  if (!root) return "X";
  return root.val + "," + serialize(root.left) + "," + serialize(root.right);
}

function deserialize(data) {
  let nodes = data.split(",");
  
  function buildTree() {
    let val = nodes.shift();
    if (val === "X") return null;
    
    let node = new TreeNode(parseInt(val));
    node.left = buildTree();
    node.right = buildTree();
    
    return node;
  }
  
  return buildTree();
}
```

---

### **3.7 Convert Sorted Array to a Balanced BST**
Given a sorted array, construct a balanced BST.

```javascript
function sortedArrayToBST(nums, left = 0, right = nums.length - 1) {
  if (left > right) return null;
  
  let mid = Math.floor((left + right) / 2);
  let node = new TreeNode(nums[mid]);
  
  node.left = sortedArrayToBST(nums, left, mid - 1);
  node.right = sortedArrayToBST(nums, mid + 1, right);
  
  return node;
}
```

---

### **3.8 Flatten Binary Tree to Linked List**
Convert a tree into a right-skewed linked list.

```javascript
function flatten(root) {
  if (!root) return;
  
  flatten(root.left);
  flatten(root.right);
  
  let temp = root.right;
  root.right = root.left;
  root.left = null;
  
  let curr = root;
  while (curr.right) {
    curr = curr.right;
  }
  curr.right = temp;
}
```

---

# **Summary**
| Algorithm | Concept |
|-----------|---------|
| **DFS** | Preorder, Inorder, Postorder Traversals |
| **BFS** | Level Order Traversal |
| **Tree Height** | Maximum Depth of a Tree |
| **Check Balance** | Height Difference of Subtrees |
| **LCA** | Find Lowest Common Ancestor |
| **Diameter** | Longest Path in the Tree |
| **Validate BST** | Check BST Properties |
| **Serialize/Deserialize** | Convert Tree to String & Back |
| **Convert Sorted Array to BST** | Build a Balanced BST from an Array |
| **Flatten Tree** | Convert to a Linked List |


Sure! Let's break down **tree algorithms** with more explanations, examples, and visual representations. We'll go step by step so you fully understand each concept.

---

## **1. Tree Traversals (DFS & BFS)**
### **Depth-First Search (DFS)**
DFS explores as deep as possible before backtracking. It has three common types:

1. **Preorder (Root → Left → Right)**
2. **Inorder (Left → Root → Right)**
3. **Postorder (Left → Right → Root)**

### **Example Tree**
```
        1
       / \
      2   3
     / \   \
    4   5   6
```

#### **Preorder Traversal (Root → Left → Right)**
**Output:** `1 → 2 → 4 → 5 → 3 → 6`

```javascript
function preorder(root) {
  if (!root) return;
  console.log(root.val);
  preorder(root.left);
  preorder(root.right);
}
```

---

#### **Inorder Traversal (Left → Root → Right)**
**Output:** `4 → 2 → 5 → 1 → 3 → 6`

```javascript
function inorder(root) {
  if (!root) return;
  inorder(root.left);
  console.log(root.val);
  inorder(root.right);
}
```

---

#### **Postorder Traversal (Left → Right → Root)**
**Output:** `4 → 5 → 2 → 6 → 3 → 1`

```javascript
function postorder(root) {
  if (!root) return;
  postorder(root.left);
  postorder(root.right);
  console.log(root.val);
}
```

---

### **Breadth-First Search (BFS) (Level Order Traversal)**
Instead of going deep, BFS explores the tree **level by level**.

**Output:** `1 → 2 → 3 → 4 → 5 → 6`

```javascript
function levelOrder(root) {
  if (!root) return;
  
  let queue = [root];
  
  while (queue.length > 0) {
    let node = queue.shift();
    console.log(node.val);
    
    if (node.left) queue.push(node.left);
    if (node.right) queue.push(node.right);
  }
}
```

---

## **2. Maximum Depth (Height) of a Tree**
The height of a tree is the **number of edges in the longest path** from the root to a leaf.

### **Example**
```
        1
       / \
      2   3
     / \    
    4   5  
```
- Height = `3`

```javascript
function maxDepth(root) {
  if (!root) return 0;
  return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}
```

---

## **3. Check if a Tree is Balanced**
A tree is **balanced** if the left and right subtree heights differ by at most 1.

### **Example**
✅ **Balanced**
```
        1
       / \
      2   3
     /
    4
```
❌ **Not Balanced**
```
        1
       /
      2
     /
    3
```

```javascript
function isBalanced(root) {
  function height(node) {
    if (!node) return 0;
    
    let leftHeight = height(node.left);
    let rightHeight = height(node.right);
    
    if (Math.abs(leftHeight - rightHeight) > 1) return -1; // Unbalanced
    
    return 1 + Math.max(leftHeight, rightHeight);
  }

  return height(root) !== -1;
}
```

---

## **4. Lowest Common Ancestor (LCA)**
Find the lowest node that is an ancestor of both **p** and **q**.

### **Example**
```
        3
       / \
      5   1
     / \   \
    6   2   8
```
- LCA of `6` and `2` = `5`
- LCA of `5` and `1` = `3`

```javascript
function lowestCommonAncestor(root, p, q) {
  if (!root || root === p || root === q) return root;
  
  let left = lowestCommonAncestor(root.left, p, q);
  let right = lowestCommonAncestor(root.right, p, q);
  
  if (left && right) return root;
  
  return left ? left : right;
}
```

---

## **5. Diameter of a Binary Tree**
The **diameter** of a tree is the **longest path between any two nodes**.

### **Example**
```
        1
       / \
      2   3
     / \    
    4   5  
```
- Diameter = `4 → 2 → 1 → 3`

```javascript
function diameterOfBinaryTree(root) {
  let diameter = 0;

  function dfs(node) {
    if (!node) return 0;
    let left = dfs(node.left);
    let right = dfs(node.right);
    diameter = Math.max(diameter, left + right);
    return 1 + Math.max(left, right);
  }

  dfs(root);
  return diameter;
}
```

---

## **6. Validate a Binary Search Tree (BST)**
A **Binary Search Tree (BST)** follows the property:
- Left subtree values `< root`
- Right subtree values `> root`

### **Example**
✅ **Valid BST**
```
        5
       / \
      3   8
     / \
    2   4
```
❌ **Invalid BST**
```
        5
       / \
      3   8
     / \
    6   4  ❌ (6 is in the wrong place)
```

```javascript
function isValidBST(root, min = null, max = null) {
  if (!root) return true;
  if ((min !== null && root.val <= min) || (max !== null && root.val >= max)) return false;
  
  return isValidBST(root.left, min, root.val) && isValidBST(root.right, root.val, max);
}
```

---

## **7. Serialize and Deserialize a Tree**
Convert a tree into a string and back.

```javascript
function serialize(root) {
  if (!root) return "X";
  return root.val + "," + serialize(root.left) + "," + serialize(root.right);
}

function deserialize(data) {
  let nodes = data.split(",");
  
  function buildTree() {
    let val = nodes.shift();
    if (val === "X") return null;
    
    let node = new TreeNode(parseInt(val));
    node.left = buildTree();
    node.right = buildTree();
    
    return node;
  }
  
  return buildTree();
}
```

---

## **8. Convert Sorted Array to a Balanced BST**
Given a sorted array, construct a **Balanced BST**.

```javascript
function sortedArrayToBST(nums, left = 0, right = nums.length - 1) {
  if (left > right) return null;
  
  let mid = Math.floor((left + right) / 2);
  let node = new TreeNode(nums[mid]);
  
  node.left = sortedArrayToBST(nums, left, mid - 1);
  node.right = sortedArrayToBST(nums, mid + 1, right);
  
  return node;
}
```

---

## **Summary Table**
| Algorithm | Concept |
|-----------|---------|
| DFS, BFS | Tree Traversals |
| Max Depth | Height of Tree |
| Balanced Tree | Check Height Balance |
| LCA | Find Lowest Common Ancestor |
| Diameter | Longest Path Between Nodes |
| Validate BST | Check BST Property |
| Serialize/Deserialize | Convert Tree to String |
| Sorted Array to BST | Build Balanced BST |
