Here are the solutions for the **Binary Search Trees (BST) Section** problems with explanations and sample I/O.

---

## **1. Search in a Binary Search Tree**
### **Problem:**
Given the root of a BST and a value `val`, return the **subtree rooted at that node** if it exists, otherwise return `None`.

### **Solution:**
- If `root.val == val`, return `root`.
- If `val < root.val`, search in the left subtree.
- If `val > root.val`, search in the right subtree.

### **Code (Recursive):**
```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def searchBST(root, val):
    if not root or root.val == val:
        return root
    return searchBST(root.left, val) if val < root.val else searchBST(root.right, val)
```

### **Sample I/O:**
```python
# Tree:      4
#           / \
#          2   7
#         / \
#        1   3

root = TreeNode(4, TreeNode(2, TreeNode(1), TreeNode(3)), TreeNode(7))

print(searchBST(root, 2).val)  # Output: 2
print(searchBST(root, 5))  # Output: None
```

### **Time Complexity:**  
- **O(log N)** (Balanced BST)  
- **O(N)** (Skewed BST)

---

## **2. Insert into a Binary Search Tree**
### **Problem:**
Insert a new node into a BST while maintaining its properties.

### **Solution:**
- If `root` is `None`, create a new node.
- If `val < root.val`, insert in the left subtree.
- If `val > root.val`, insert in the right subtree.

### **Code (Recursive):**
```python
def insertIntoBST(root, val):
    if not root:
        return TreeNode(val)
    
    if val < root.val:
        root.left = insertIntoBST(root.left, val)
    else:
        root.right = insertIntoBST(root.right, val)
    
    return root
```

### **Sample I/O:**
```python
root = TreeNode(4, TreeNode(2, TreeNode(1), TreeNode(3)), TreeNode(7))
insertIntoBST(root, 5)
```

### **Time Complexity:**  
- **O(log N)** (Balanced BST)  
- **O(N)** (Skewed BST)

---

## **3. Convert Sorted Array to Binary Search Tree**
### **Problem:**
Convert a sorted array into a **height-balanced BST**.

### **Solution:**
- Choose the **middle element** as the root.
- Recursively build the left and right subtrees.

### **Code (Recursive):**
```python
def sortedArrayToBST(nums):
    if not nums:
        return None
    
    mid = len(nums) // 2
    root = TreeNode(nums[mid])
    
    root.left = sortedArrayToBST(nums[:mid])
    root.right = sortedArrayToBST(nums[mid+1:])
    
    return root
```

### **Sample I/O:**
```python
sortedArrayToBST([-10,-3,0,5,9])
```

### **Time Complexity:**  
- **O(N)**

---

## **4. Two Sum IV - Input is a BST**
### **Problem:**
Find if there exist two nodes whose sum equals `k`.

### **Solution:**
- Use **Inorder Traversal** (BST → Sorted Array).
- Use **Two-Pointer Approach**.

### **Code:**
```python
def findTarget(root, k):
    def inorder(node, nums):
        if not node:
            return
        inorder(node.left, nums)
        nums.append(node.val)
        inorder(node.right, nums)

    nums = []
    inorder(root, nums)

    l, r = 0, len(nums) - 1
    while l < r:
        total = nums[l] + nums[r]
        if total == k:
            return True
        elif total < k:
            l += 1
        else:
            r -= 1
    
    return False
```

### **Time Complexity:**  
- **O(N)**

---

## **5. Lowest Common Ancestor of a Binary Search Tree**
### **Problem:**
Find the **Lowest Common Ancestor (LCA)** of two given nodes.

### **Solution:**
- If `p` and `q` lie on **different sides**, return `root`.
- If both `p` and `q` are on the left, recurse left.
- If both `p` and `q` are on the right, recurse right.

### **Code:**
```python
def lowestCommonAncestor(root, p, q):
    if p.val < root.val and q.val < root.val:
        return lowestCommonAncestor(root.left, p, q)
    elif p.val > root.val and q.val > root.val:
        return lowestCommonAncestor(root.right, p, q)
    return root
```

### **Time Complexity:**  
- **O(log N)**

---

## **6. Minimum Absolute Difference in BST**
### **Problem:**
Find the **minimum absolute difference** between any two nodes.

### **Solution:**
- Use **Inorder Traversal** (BST → Sorted Array).
- Compute **minimum difference**.

### **Code:**
```python
def getMinimumDifference(root):
    prev = float('-inf')
    min_diff = float('inf')

    def inorder(node):
        nonlocal prev, min_diff
        if not node:
            return
        inorder(node.left)
        min_diff = min(min_diff, node.val - prev)
        prev = node.val
        inorder(node.right)

    inorder(root)
    return min_diff
```

### **Time Complexity:**  
- **O(N)**

---

## **7. Balance a Binary Search Tree**
### **Problem:**
Convert an **unbalanced BST** into a **balanced BST**.

### **Solution:**
1. Convert BST to **sorted array** (inorder traversal).
2. Use **sortedArrayToBST** (previous problem).

### **Code:**
```python
def balanceBST(root):
    nums = []

    def inorder(node):
        if not node:
            return
        inorder(node.left)
        nums.append(node.val)
        inorder(node.right)

    inorder(root)
    return sortedArrayToBST(nums)
```

### **Time Complexity:**  
- **O(N)**

---

## **8. Delete Node in a BST**
### **Problem:**
Delete a node in a BST while maintaining its properties.

### **Solution:**
1. **Find the node**.
2. **If node has one child**, replace it.
3. **If node has two children**, replace it with the **smallest node in the right subtree**.

### **Code:**
```python
def deleteNode(root, key):
    if not root:
        return None

    if key < root.val:
        root.left = deleteNode(root.left, key)
    elif key > root.val:
        root.right = deleteNode(root.right, key)
    else:
        if not root.left:
            return root.right
        if not root.right:
            return root.left

        temp = root.right
        while temp.left:
            temp = temp.left
        root.val = temp.val
        root.right = deleteNode(root.right, temp.val)

    return root
```

### **Time Complexity:**  
- **O(log N)**

---

## **9. Kth Smallest Element in a BST**
### **Problem:**
Find the **Kth smallest** element.

### **Solution:**
- **Inorder Traversal** (BST → Sorted Array).
- Return the `Kth` element.

### **Code:**
```python
def kthSmallest(root, k):
    def inorder(node):
        if not node:
            return []
        return inorder(node.left) + [node.val] + inorder(node.right)

    return inorder(root)[k-1]
```

### **Time Complexity:**  
- **O(N)**

---
