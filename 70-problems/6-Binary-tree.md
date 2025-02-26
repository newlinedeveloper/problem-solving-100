Here are the solutions for the **Binary Trees Section** problems with explanations and sample I/O.

---

## **1. Average of Levels in Binary Tree**
### **Problem:**
Find the **average value of nodes** at each level in a binary tree.

### **Solution:**
- Use **Level Order Traversal** (BFS).
- For each level, calculate the average.

### **Code (BFS - Queue):**
```python
from collections import deque

def averageOfLevels(root):
    if not root:
        return []
    
    res = []
    queue = deque([root])

    while queue:
        level_sum = 0
        level_size = len(queue)
        
        for _ in range(level_size):
            node = queue.popleft()
            level_sum += node.val
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)

        res.append(level_sum / level_size)

    return res
```

### **Time Complexity:**  
- **O(N)**

---

## **2. Minimum Depth of Binary Tree**
### **Problem:**
Find the **minimum depth** (distance to the nearest leaf).

### **Solution:**
- Use **BFS** (First leaf encountered is the answer).

### **Code (BFS - Queue):**
```python
def minDepth(root):
    if not root:
        return 0
    
    queue = deque([(root, 1)])

    while queue:
        node, depth = queue.popleft()
        if not node.left and not node.right:
            return depth
        if node.left:
            queue.append((node.left, depth + 1))
        if node.right:
            queue.append((node.right, depth + 1))
```

### **Time Complexity:**  
- **O(N)**

---

## **3. Maximum Depth of Binary Tree**
### **Problem:**
Find the **maximum depth** (longest path to a leaf).

### **Solution:**
- Use **DFS (Recursion)**.

### **Code (DFS - Recursive):**
```python
def maxDepth(root):
    if not root:
        return 0
    return 1 + max(maxDepth(root.left), maxDepth(root.right))
```

### **Time Complexity:**  
- **O(N)**

---

## **4. Min/Max Value in Binary Tree**
### **Solution (DFS - Recursive):**
```python
def findMin(root):
    if not root:
        return float('inf')
    return min(root.val, findMin(root.left), findMin(root.right))

def findMax(root):
    if not root:
        return float('-inf')
    return max(root.val, findMax(root.left), findMax(root.right))
```

### **Time Complexity:**  
- **O(N)**

---

## **5. Binary Tree Level Order Traversal**
### **Problem:**
Return a **list of node values at each level**.

### **Solution:**
- Use **BFS (Queue)**.

### **Code (BFS - Queue):**
```python
def levelOrder(root):
    if not root:
        return []
    
    res, queue = [], deque([root])

    while queue:
        level, size = [], len(queue)
        
        for _ in range(size):
            node = queue.popleft()
            level.append(node.val)
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)

        res.append(level)

    return res
```

### **Time Complexity:**  
- **O(N)**

---

## **6. Same Tree**
### **Problem:**
Check if two trees are **identical**.

### **Solution:**
- Use **Recursion**.

### **Code (DFS - Recursive):**
```python
def isSameTree(p, q):
    if not p and not q:
        return True
    if not p or not q or p.val != q.val:
        return False
    return isSameTree(p.left, q.left) and isSameTree(p.right, q.right)
```

### **Time Complexity:**  
- **O(N)**

---

## **7. Path Sum**
### **Problem:**
Check if there exists a **root-to-leaf path** whose sum equals `targetSum`.

### **Solution:**
- Use **DFS** (Subtract `targetSum` as we traverse).

### **Code (DFS - Recursive):**
```python
def hasPathSum(root, targetSum):
    if not root:
        return False
    if not root.left and not root.right and targetSum == root.val:
        return True
    return hasPathSum(root.left, targetSum - root.val) or hasPathSum(root.right, targetSum - root.val)
```

### **Time Complexity:**  
- **O(N)**

---

## **8. Diameter of a Binary Tree**
### **Problem:**
Find the **diameter** (longest path between any two nodes).

### **Solution:**
- Compute `leftHeight + rightHeight` at each node.
- Track the maximum diameter.

### **Code (DFS - Recursive):**
```python
def diameterOfBinaryTree(root):
    max_diameter = 0

    def depth(node):
        nonlocal max_diameter
        if not node:
            return 0
        left = depth(node.left)
        right = depth(node.right)
        max_diameter = max(max_diameter, left + right)
        return 1 + max(left, right)

    depth(root)
    return max_diameter
```

### **Time Complexity:**  
- **O(N)**

---

## **9. Invert Binary Tree**
### **Problem:**
Invert (mirror) a binary tree.

### **Solution:**
- Swap `left` and `right` for each node.

### **Code (DFS - Recursive):**
```python
def invertTree(root):
    if not root:
        return None
    root.left, root.right = invertTree(root.right), invertTree(root.left)
    return root
```

### **Time Complexity:**  
- **O(N)**

---

## **10. Lowest Common Ancestor of a Binary Tree**
### **Problem:**
Find the **Lowest Common Ancestor (LCA)** of two nodes.

### **Solution:**
- If one node is in the left subtree and the other in the right, return `root`.
- Otherwise, recurse in the subtree that contains both nodes.

### **Code (DFS - Recursive):**
```python
def lowestCommonAncestor(root, p, q):
    if not root or root == p or root == q:
        return root
    
    left = lowestCommonAncestor(root.left, p, q)
    right = lowestCommonAncestor(root.right, p, q)

    if left and right:
        return root
    return left if left else right
```

### **Time Complexity:**  
- **O(N)**

---
