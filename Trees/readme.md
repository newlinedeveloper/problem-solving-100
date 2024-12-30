### Trees

### **1. Maximum Depth of Binary Tree**

#### Explanation:
Find the maximum depth of a binary tree. The depth is the number of nodes along the longest path from the root to a leaf.

#### Pseudocode:
1. If the root is `nil`, return 0.
2. Recursively calculate the depth of the left and right subtrees.
3. The depth of the tree is `1 + max(leftDepth, rightDepth)`.

#### Solution in Golang:
```go
package main

import "fmt"

type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

func maxDepth(root *TreeNode) int {
    if root == nil {
        return 0
    }
    left := maxDepth(root.Left)
    right := maxDepth(root.Right)
    return 1 + max(left, right)
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func main() {
    root := &TreeNode{1, 
        &TreeNode{2, &TreeNode{4, nil, nil}, &TreeNode{5, nil, nil}}, 
        &TreeNode{3, nil, nil}}
    fmt.Println(maxDepth(root)) // Output: 3
}
```

---

### **2. Invert Binary Tree**

#### Explanation:
Invert a binary tree by swapping the left and right children of all nodes.

#### Pseudocode:
1. If the root is `nil`, return.
2. Swap the left and right subtrees.
3. Recursively call the function for left and right subtrees.

#### Solution in Golang:
```go
func invertTree(root *TreeNode) *TreeNode {
    if root == nil {
        return nil
    }
    root.Left, root.Right = root.Right, root.Left
    invertTree(root.Left)
    invertTree(root.Right)
    return root
}

func main() {
    root := &TreeNode{1, &TreeNode{2, nil, nil}, &TreeNode{3, nil, nil}}
    fmt.Println(invertTree(root)) // Output: [1, 3, 2]
}
```

---

### **3. Binary Tree Maximum Path Sum**

#### Explanation:
Find the maximum path sum in a binary tree. A path can start and end at any node.

#### Pseudocode:
1. Use a helper function to compute the maximum path sum through each node.
2. Track the global maximum.

#### Solution in Golang:
```go
func maxPathSum(root *TreeNode) int {
    maxSum := -1 << 31
    var dfs func(node *TreeNode) int
    dfs = func(node *TreeNode) int {
        if node == nil {
            return 0
        }
        left := max(0, dfs(node.Left))
        right := max(0, dfs(node.Right))
        maxSum = max(maxSum, left+right+node.Val)
        return max(left, right) + node.Val
    }
    dfs(root)
    return maxSum
}

func main() {
    root := &TreeNode{1, &TreeNode{2, nil, nil}, &TreeNode{3, nil, nil}}
    fmt.Println(maxPathSum(root)) // Output: 6
}
```

---

### **4. Same Tree**

#### Explanation:
Check if two binary trees are structurally identical and have the same values.

#### Pseudocode:
1. If both nodes are `nil`, return `true`.
2. If one node is `nil`, return `false`.
3. Check if values match and recurse for left and right subtrees.

#### Solution in Golang:
```go
func isSameTree(p *TreeNode, q *TreeNode) bool {
    if p == nil && q == nil {
        return true
    }
    if p == nil || q == nil || p.Val != q.Val {
        return false
    }
    return isSameTree(p.Left, q.Left) && isSameTree(p.Right, q.Right)
}

func main() {
    p := &TreeNode{1, &TreeNode{2, nil, nil}, &TreeNode{3, nil, nil}}
    q := &TreeNode{1, &TreeNode{2, nil, nil}, &TreeNode{3, nil, nil}}
    fmt.Println(isSameTree(p, q)) // Output: true
}
```

---

### **5. Binary Tree Level Order Traversal**

#### Explanation:
Perform a level-order traversal (BFS) of a binary tree.

#### Pseudocode:
1. Use a queue to store nodes.
2. For each level, dequeue nodes and enqueue their children.

#### Solution in Golang:
```go
func levelOrder(root *TreeNode) [][]int {
    var result [][]int
    if root == nil {
        return result
    }
    queue := []*TreeNode{root}
    for len(queue) > 0 {
        level := []int{}
        n := len(queue)
        for i := 0; i < n; i++ {
            node := queue[0]
            queue = queue[1:]
            level = append(level, node.Val)
            if node.Left != nil {
                queue = append(queue, node.Left)
            }
            if node.Right != nil {
                queue = append(queue, node.Right)
            }
        }
        result = append(result, level)
    }
    return result
}

func main() {
    root := &TreeNode{1, &TreeNode{2, nil, nil}, &TreeNode{3, nil, nil}}
    fmt.Println(levelOrder(root)) // Output: [[1], [2, 3]]
}
```

---

### **6. Lowest Common Ancestor of a Binary Tree**

#### Explanation:
The Lowest Common Ancestor (LCA) of two nodes \( p \) and \( q \) in a binary tree is the lowest node that has both \( p \) and \( q \) as descendants.

#### Pseudocode:
1. If the root is `nil`, return `nil`.
2. If the root matches \( p \) or \( q \), return the root.
3. Recursively find LCA in the left and right subtrees.
4. If both subtrees return non-nil values, the root is the LCA.
5. Otherwise, return the non-nil subtree.

#### Solution in Golang:
```go
func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
    if root == nil || root == p || root == q {
        return root
    }
    left := lowestCommonAncestor(root.Left, p, q)
    right := lowestCommonAncestor(root.Right, p, q)
    if left != nil && right != nil {
        return root
    }
    if left != nil {
        return left
    }
    return right
}

func main() {
    root := &TreeNode{3, 
        &TreeNode{5, &TreeNode{6, nil, nil}, &TreeNode{2, nil, nil}}, 
        &TreeNode{1, nil, nil}}
    p := root.Left         // Node 5
    q := root.Right        // Node 1
    fmt.Println(lowestCommonAncestor(root, p, q).Val) // Output: 3
}
```

---

### **7. Construct Binary Tree from Preorder and Inorder Traversal**

#### Explanation:
Use preorder and inorder traversal to reconstruct a binary tree.

#### Pseudocode:
1. The first element of the preorder array is the root.
2. Find the root index in the inorder array to divide the left and right subtrees.
3. Recursively build the left and right subtrees.

#### Solution in Golang:
```go
func buildTree(preorder []int, inorder []int) *TreeNode {
    if len(preorder) == 0 || len(inorder) == 0 {
        return nil
    }
    root := &TreeNode{Val: preorder[0]}
    mid := 0
    for i, v := range inorder {
        if v == root.Val {
            mid = i
            break
        }
    }
    root.Left = buildTree(preorder[1:mid+1], inorder[:mid])
    root.Right = buildTree(preorder[mid+1:], inorder[mid+1:])
    return root
}

func main() {
    preorder := []int{3, 9, 20, 15, 7}
    inorder := []int{9, 3, 15, 20, 7}
    root := buildTree(preorder, inorder)
    fmt.Println(root.Val) // Output: 3 (root node value)
}
```

---

### **8. Binary Tree Right Side View**

#### Explanation:
Return the values of the nodes visible from the right side of a binary tree.

#### Pseudocode:
1. Perform a level-order traversal.
2. Add the last node value of each level to the result.

#### Solution in Golang:
```go
func rightSideView(root *TreeNode) []int {
    var result []int
    if root == nil {
        return result
    }
    queue := []*TreeNode{root}
    for len(queue) > 0 {
        n := len(queue)
        for i := 0; i < n; i++ {
            node := queue[0]
            queue = queue[1:]
            if i == n-1 {
                result = append(result, node.Val)
            }
            if node.Left != nil {
                queue = append(queue, node.Left)
            }
            if node.Right != nil {
                queue = append(queue, node.Right)
            }
        }
    }
    return result
}

func main() {
    root := &TreeNode{1, &TreeNode{2, nil, &TreeNode{5, nil, nil}}, &TreeNode{3, nil, &TreeNode{4, nil, nil}}}
    fmt.Println(rightSideView(root)) // Output: [1, 3, 4]
}
```

---

### **9. Validate Binary Search Tree**

#### Explanation:
Check if a binary tree is a valid binary search tree (BST).

#### Pseudocode:
1. Use a helper function with bounds (`min` and `max`).
2. For each node, ensure its value is between the bounds.
3. Recursively check left and right subtrees.

#### Solution in Golang:
```go
func isValidBST(root *TreeNode) bool {
    return validate(root, nil, nil)
}

func validate(node *TreeNode, min, max *int) bool {
    if node == nil {
        return true
    }
    if (min != nil && node.Val <= *min) || (max != nil && node.Val >= *max) {
        return false
    }
    return validate(node.Left, min, &node.Val) && validate(node.Right, &node.Val, max)
}

func main() {
    root := &TreeNode{2, &TreeNode{1, nil, nil}, &TreeNode{3, nil, nil}}
    fmt.Println(isValidBST(root)) // Output: true
}
```

---

### **10. Kth Smallest Element in a BST**

#### Explanation:
Find the kth smallest element in a BST using an in-order traversal.

#### Pseudocode:
1. Perform an in-order traversal.
2. Keep track of the count of visited nodes.
3. Return the kth element.

#### Solution in Golang:
```go
func kthSmallest(root *TreeNode, k int) int {
    stack := []*TreeNode{}
    for {
        for root != nil {
            stack = append(stack, root)
            root = root.Left
        }
        root = stack[len(stack)-1]
        stack = stack[:len(stack)-1]
        k--
        if k == 0 {
            return root.Val
        }
        root = root.Right
    }
}

func main() {
    root := &TreeNode{3, &TreeNode{1, nil, &TreeNode{2, nil, nil}}, &TreeNode{4, nil, nil}}
    fmt.Println(kthSmallest(root, 1)) // Output: 1
}
```

---

### **11. Binary Tree Zigzag Level Order Traversal**

#### Explanation:
Perform a level-order traversal where the order alternates between left-to-right and right-to-left.

#### Pseudocode:
1. Use a queue to traverse level by level.
2. Reverse the order of nodes for alternate levels.

#### Solution in Golang:
```go
func zigzagLevelOrder(root *TreeNode) [][]int {
    var result [][]int
    if root == nil {
        return result
    }
    queue := []*TreeNode{root}
    leftToRight := true
    for len(queue) > 0 {
        n := len(queue)
        level := []int{}
        for i := 0; i < n; i++ {
            node := queue[0]
            queue = queue[1:]
            level = append(level, node.Val)
            if node.Left != nil {
                queue = append(queue, node.Left)
            }
            if node.Right != nil {
                queue = append(queue, node.Right)
            }
        }
        if !leftToRight {
            reverse(level)
        }
        leftToRight = !leftToRight
        result = append(result, level)
    }
    return result
}

func reverse(nums []int) {
    for i, j := 0, len(nums)-1; i < j; i, j = i+1, j-1 {
        nums[i], nums[j] = nums[j], nums[i]
    }
}

func main() {
    root := &TreeNode{1, &TreeNode{2, &TreeNode{4, nil, nil}, nil}, &TreeNode{3, nil, &TreeNode{5, nil, nil}}}
    fmt.Println(zigzagLevelOrder(root)) // Output: [[1], [3, 2], [4, 5]]
}
```
### **12. Balanced Binary Tree**

#### Explanation:
A binary tree is balanced if the height of its two subtrees never differs by more than one for any node.

#### Pseudocode:
1. Write a helper function to calculate the height of a subtree.
2. Check if the height difference between left and right subtrees is greater than 1.
3. Recursively check the balance condition for all nodes.

#### Solution in Golang:
```go
func isBalanced(root *TreeNode) bool {
    var height func(*TreeNode) int
    height = func(node *TreeNode) int {
        if node == nil {
            return 0
        }
        left := height(node.Left)
        right := height(node.Right)
        if left == -1 || right == -1 || abs(left-right) > 1 {
            return -1
        }
        return 1 + max(left, right)
    }
    return height(root) != -1
}

func abs(x int) int {
    if x < 0 {
        return -x
    }
    return x
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func main() {
    root := &TreeNode{3, &TreeNode{9, nil, nil}, &TreeNode{20, &TreeNode{15, nil, nil}, &TreeNode{7, nil, nil}}}
    fmt.Println(isBalanced(root)) // Output: true
}
```

#### Input:
```
          3
        /   \
       9     20
            /  \
           15   7
```

#### Output:
```
true
```

---

### **Sample Binary Tree Structure for Problems**

Let’s define a helper function to create a binary tree and traverse it. You can use this as the foundation for running the algorithms:

```go
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}

// Helper function to create a binary tree from a list of values
func createBinaryTree(values []interface{}) *TreeNode {
    if len(values) == 0 {
        return nil
    }
    root := &TreeNode{Val: values[0].(int)}
    queue := []*TreeNode{root}
    i := 1
    for i < len(values) {
        node := queue[0]
        queue = queue[1:]
        if values[i] != nil {
            node.Left = &TreeNode{Val: values[i].(int)}
            queue = append(queue, node.Left)
        }
        i++
        if i < len(values) && values[i] != nil {
            node.Right = &TreeNode{Val: values[i].(int)}
            queue = append(queue, node.Right)
        }
        i++
    }
    return root
}

// Helper function for level-order traversal (BFS)
func levelOrderTraversal(root *TreeNode) []int {
    if root == nil {
        return []int{}
    }
    var result []int
    queue := []*TreeNode{root}
    for len(queue) > 0 {
        node := queue[0]
        queue = queue[1:]
        result = append(result, node.Val)
        if node.Left != nil {
            queue = append(queue, node.Left)
        }
        if node.Right != nil {
            queue = append(queue, node.Right)
        }
    }
    return result
}

func main() {
    // Example: Create and traverse a binary tree
    values := []interface{}{1, 2, 3, nil, 4, nil, 5}
    root := createBinaryTree(values)
    fmt.Println(levelOrderTraversal(root)) // Output: [1, 2, 3, 4, 5]
}
```

---

