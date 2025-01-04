## Recursion

### 1. Generate Parentheses

#### Problem Description
Given `n` pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

#### Pseudocode
1. Use backtracking to generate all possible strings.
2. Keep track of the number of open and close parentheses used.
3. If the number of open parentheses is less than `n`, add an open parenthesis.
4. If the number of close parentheses is less than the number of open parentheses, add a close parenthesis.
5. Continue until the string is well-formed with `n` pairs of parentheses.

---

#### Golang Solution
```go
func generateParenthesis(n int) []string {
    var result []string
    var backtrack func(s string, open, close int)
    backtrack = func(s string, open, close int) {
        if len(s) == 2*n {
            result = append(result, s)
            return
        }
        if open < n {
            backtrack(s+"(", open+1, close)
        }
        if close < open {
            backtrack(s+")", open, close+1)
        }
    }
    backtrack("", 0, 0)
    return result
}

func main() {
    fmt.Println(generateParenthesis(3)) // Output: ["((()))","(()())","(())()","()(())","()()()"]
}
```

---

#### Python Solution
```python
def generateParenthesis(n):
    result = []
    
    def backtrack(s, open, close):
        if len(s) == 2 * n:
            result.append(s)
            return
        if open < n:
            backtrack(s + '(', open + 1, close)
        if close < open:
            backtrack(s + ')', open, close + 1)

    backtrack('', 0, 0)
    return result

# Sample usage
print(generateParenthesis(3))  # Output: ["((()))","(()())","(())()","()(())","()()()"]
```

---

### Explanation
- The function uses backtracking to explore all combinations by adding open or close parentheses based on the current state.
- It ensures that at any point, the number of close parentheses does not exceed the number of open parentheses to maintain valid sequences.

---

### 2. Subsets

#### Problem Description
Given an integer array `nums` of unique elements, return all possible subsets (the power set).

#### Pseudocode
1. Use backtracking to explore all subsets.
2. For each element, decide whether to include it in the current subset or not.
3. Append the current subset to the result list.
4. Recursively generate subsets by including or excluding the next element.

---

#### Golang Solution
```go
func subsets(nums []int) [][]int {
    var result [][]int
    var backtrack func(start int, current []int)
    backtrack = func(start int, current []int) {
        result = append(result, append([]int{}, current...))
        for i := start; i < len(nums); i++ {
            current = append(current, nums[i])
            backtrack(i+1, current)
            current = current[:len(current)-1]
        }
    }
    backtrack(0, []int{})
    return result
}

func main() {
    nums := []int{1, 2, 3}
    fmt.Println(subsets(nums)) // Output: [[], [1], [1, 2], [1, 2, 3], [1, 3], [2], [2, 3], [3]]
}
```

---

#### Python Solution
```python
def subsets(nums):
    result = []
    
    def backtrack(start, current):
        result.append(current[:])
        for i in range(start, len(nums)):
            current.append(nums[i])
            backtrack(i + 1, current)
            current.pop()

    backtrack(0, [])
    return result

# Sample usage
print(subsets([1, 2, 3]))  # Output: [[], [1], [1, 2], [1, 2, 3], [1, 3], [2], [2, 3], [3]]
```

---

### Explanation
- The backtracking function explores all combinations of including or excluding each element.
- It generates subsets by recursively building up the list of elements included in each subset.

---

### 3. Permutations

#### Problem Description
Given a list of distinct integers, return all possible permutations.

#### Pseudocode
1. Use backtracking to explore all permutations.
2. For each number, try placing it in each position.
3. Swap elements to create permutations.
4. Revert the swap after exploring the permutation.

---

#### Golang Solution
```go
func permute(nums []int) [][]int {
    var result [][]int
    var backtrack func(first int)
    backtrack = func(first int) {
        if first == len(nums) {
            result = append(result, append([]int{}, nums...))
            return
        }
        for i := first; i < len(nums); i++ {
            nums[first], nums[i] = nums[i], nums[first]
            backtrack(first + 1)
            nums[first], nums[i] = nums[i], nums[first]
        }
    }
    backtrack(0)
    return result
}

func main() {
    nums := []int{1, 2, 3}
    fmt.Println(permute(nums)) // Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
}
```

---

#### Python Solution
```python
def permute(nums):
    result = []
    
    def backtrack(start):
        if start == len(nums):
            result.append(nums[:])
            return
        for i in range(start, len(nums)):
            nums[start], nums[i] = nums[i], nums[start]
            backtrack(start + 1)
            nums[start], nums[i] = nums[i], nums[start]

    backtrack(0)
    return result

# Sample usage
print(permute([1, 2, 3]))  # Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

---

### Explanation
- The algorithm uses backtracking to swap each element with the current position to generate all permutations.
- It swaps back after exploring each permutation to reset the array for the next iteration.

---

### 4. Palindrome Partitioning

#### Problem Description
Given a string, partition it such that every substring is a palindrome. Return all possible palindrome partitioning.

#### Pseudocode
1. Use backtracking to find all partitions.
2. Check if the current substring is a palindrome.
3. If yes, continue exploring the rest of the string.
4. Append the current partition to the result.

---

#### Golang Solution
```go
func partition(s string) [][]string {
    var result [][]string
    var backtrack func(start int, path []string)
    backtrack = func(start int, path []string) {
        if start == len(s) {
            result = append(result, append([]string{}, path...))
            return
        }
        for end := start + 1; end <= len(s); end++ {
            if isPalindrome(s[start:end]) {
                backtrack(end, append(path, s[start:end]))
            }
        }
    }
    backtrack(0, []string{})
    return result
}

func isPalindrome(s string) bool {
    for i := 0; i < len(s)/2; i++ {
        if s[i] != s[len(s)-1-i] {
            return false
        }
    }
    return true
}

func main() {
    s := "aab"
    fmt.Println(partition(s)) // Output: [["a","a","b"],["aa","b"]]
}
```

---

#### Python Solution
```python
def partition(s):
    result = []
    
    def backtrack(start, path):
        if start == len(s):
            result.append(path[:])
            return
        for end in range(start + 1, len(s) + 1):
            if is_palindrome(s[start:end]):
                backtrack(end, path + [s[start:end]])
    
    def is_palindrome(sub):
        return sub == sub[::-1]

    backtrack(0, [])
    return result

# Sample usage
print(partition("aab"))  # Output: [["a","a","b"],["aa","b"]]
```

---

### Explanation
- The function uses backtracking to explore partitions of the string.
- It checks if each substring is a palindrome before adding it to the current path.

---

### 5. Combination Sum II

#### Problem Description
Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in candidates where the candidate numbers sum to the target. Each number in candidates may only be used once in the combination.

#### Pseudocode
1. Sort the candidates to handle duplicates.
2. Use backtracking to explore all combinations.
3. Skip duplicates to ensure unique combinations.
4. Stop exploring further if the current sum exceeds the target.

---

#### Golang Solution
```go
func combinationSum2(candidates []int, target int) [][]int {
    sort.Ints(candidates)
    var result [][]int
    var backtrack func(start int, target int, path []int)
    backtrack = func(start int, target int, path []int) {
        if target == 0 {
            result = append(result, append([]int{}, path...))
            return
        }
        for i := start; i < len(candidates); i++ {
            if i > start && candidates[i] == candidates[i-1] {
                continue
            }
            if candidates[i] > target {
                break
            }
            backtrack(i+1, target-candidates[i], append(path, candidates[i]))
        }
    }
    backtrack(0, target, []int{})
    return result
}

func main() {
    candidates := []int{10, 1, 2, 7, 6, 1, 5}
    target := 8
    fmt.Println(combinationSum2(candidates, target)) // Output: [[1,1,6],[1,2,5],[1,7],[2,6]]
}
```

---

#### Python Solution
```python
def combinationSum2(candidates, target):
    candidates.sort()
    result = []

    def backtrack(start, target, path):
        if target == 0:
            result.append(path[:])
            return
        for i in range(start, len(candidates)):
            if i > start and candidates[i] == candidates[i - 1]:
                continue
            if candidates[i] > target:
                break
            backtrack(i + 1, target - candidates[i], path + [candidates[i]])

    backtrack(0, target, [])
    return result

# Sample usage
print(combinationSum2([10, 1, 2, 7, 6, 1, 5], 8))  # Output: [[1,1,6],[1,2,5],[1,7],[2,6]]
```

---

### Explanation
- The algorithm sorts the candidates to handle duplicates and ensure that each combination is unique.
- It uses backtracking to explore each combination, skipping over duplicates to avoid repeated sets.

---

### 6. Remove Linked List Elements

#### Problem Description
Remove all elements from a linked list of integers that have a value equal to the given value.

#### Pseudocode
1. Initialize a dummy node to simplify edge cases.
2. Traverse the list while checking the value of each node.
3. If a node matches the target value, remove it by updating the pointers.

---

#### Golang Solution
```go
func removeElements(head *ListNode, val int) *ListNode {
    dummy := &ListNode{Next: head}
    current := dummy
    for current.Next != nil {
        if current.Next.Val == val {
            current.Next = current.Next.Next
        } else {
            current = current.Next
        }
    }
    return dummy.Next
}

func main() {
    head := &ListNode{1, &ListNode{2, &ListNode{6, &ListNode{3, &ListNode{4, &ListNode{5, &ListNode{6, nil}}}}}}}
    result := removeElements(head, 6)
    for result != nil {
        fmt.Print(result.Val, " ")
        result = result.Next
    }
    // Output: 1 2 3 4 5
}
```

---

#### Python Solution
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def removeElements(head, val):
    dummy = ListNode(0, head)
    current = dummy
    while current.next:
        if current.next.val == val:
            current.next = current.next.next
        else:
            current = current.next
    return dummy.next

# Sample usage
head = ListNode(1, ListNode(2, ListNode(6, ListNode(3, ListNode(4, ListNode(5, ListNode(6)))))))
result = removeElements(head, 6)
while result:
    print(result.val, end=" ")  # Output: 1 2 3 4 5
    result = result.next
```

---

### Explanation
- The algorithm uses a dummy node to handle cases where the head of the list itself needs to be removed.
- It traverses the list, removing nodes that match the target value.

---

### 7. Different Ways to Add Parentheses

#### Problem Description
Given a string expression of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. You may return the answer in any order.

#### Pseudocode
1. Define a recursive function `compute` that takes a substring of the expression.
2. If the substring is a number, return it as the only result.
3. Iterate through the substring, and for each operator, split the expression into two parts.
4. Recursively compute results for each part, and combine the results based on the operator.
5. Return the combined results.

---

#### Golang Solution
```go
func diffWaysToCompute(expression string) []int {
    if val, err := strconv.Atoi(expression); err == nil {
        return []int{val}
    }
    results := []int{}
    for i := 0; i < len(expression); i++ {
        if expression[i] == '+' || expression[i] == '-' || expression[i] == '*' {
            leftResults := diffWaysToCompute(expression[:i])
            rightResults := diffWaysToCompute(expression[i+1:])
            for _, left := range leftResults {
                for _, right := range rightResults {
                    switch expression[i] {
                    case '+':
                        results = append(results, left+right)
                    case '-':
                        results = append(results, left-right)
                    case '*':
                        results = append(results, left*right)
                    }
                }
            }
        }
    }
    return results
}

func main() {
    expression := "2*3-4*5"
    fmt.Println(diffWaysToCompute(expression)) // Output: [-34, -14, -10, -10, 10]
}
```

---

#### Python Solution
```python
def diffWaysToCompute(expression):
    if expression.isdigit():
        return [int(expression)]
    
    results = []
    for i, char in enumerate(expression):
        if char in "+-*":
            leftResults = diffWaysToCompute(expression[:i])
            rightResults = diffWaysToCompute(expression[i+1:])
            for left in leftResults:
                for right in rightResults:
                    if char == '+':
                        results.append(left + right)
                    elif char == '-':
                        results.append(left - right)
                    elif char == '*':
                        results.append(left * right)
    return results

# Sample usage
expression = "2*3-4*5"
print(diffWaysToCompute(expression))  # Output: [-34, -14, -10, -10, 10]
```

---

### Explanation
- The algorithm uses recursion to split the expression at each operator and computes all possible results for each combination of left and right parts.
- This allows exploring all different ways to add parentheses and compute the results.

---





