### Stacks and Queues

Let's go through each problem one by one:

---

### 1. Implement Queue using Stacks

#### Problem Description
Implement a queue using two stacks. The queue should support `push`, `pop`, `peek`, and `empty` operations.

#### Pseudocode
1. Use two stacks, `stack1` for push operations and `stack2` for pop operations.
2. For `push`, push the element onto `stack1`.
3. For `pop`, if `stack2` is empty, transfer all elements from `stack1` to `stack2`. Then pop from `stack2`.
4. For `peek`, if `stack2` is empty, transfer all elements from `stack1` to `stack2`. Then return the top of `stack2`.
5. For `empty`, check if both `stack1` and `stack2` are empty.

---

#### Golang Solution
```go
type MyQueue struct {
    stack1 []int
    stack2 []int
}

func Constructor() MyQueue {
    return MyQueue{}
}

func (this *MyQueue) Push(x int) {
    this.stack1 = append(this.stack1, x)
}

func (this *MyQueue) Pop() int {
    if len(this.stack2) == 0 {
        for len(this.stack1) > 0 {
            this.stack2 = append(this.stack2, this.stack1[len(this.stack1)-1])
            this.stack1 = this.stack1[:len(this.stack1)-1]
        }
    }
    top := this.stack2[len(this.stack2)-1]
    this.stack2 = this.stack2[:len(this.stack2)-1]
    return top
}

func (this *MyQueue) Peek() int {
    if len(this.stack2) == 0 {
        for len(this.stack1) > 0 {
            this.stack2 = append(this.stack2, this.stack1[len(this.stack1)-1])
            this.stack1 = this.stack1[:len(this.stack1)-1]
        }
    }
    return this.stack2[len(this.stack2)-1]
}

func (this *MyQueue) Empty() bool {
    return len(this.stack1) == 0 && len(this.stack2) == 0
}

func main() {
    q := Constructor()
    q.Push(1)
    q.Push(2)
    fmt.Println(q.Peek())  // Output: 1
    fmt.Println(q.Pop())   // Output: 1
    fmt.Println(q.Empty()) // Output: false
}
```

---

#### Python Solution
```python
class MyQueue:

    def __init__(self):
        self.stack1 = []
        self.stack2 = []

    def push(self, x: int) -> None:
        self.stack1.append(x)

    def pop(self) -> int:
        if not self.stack2:
            while self.stack1:
                self.stack2.append(self.stack1.pop())
        return self.stack2.pop()

    def peek(self) -> int:
        if not self.stack2:
            while self.stack1:
                self.stack2.append(self.stack1.pop())
        return self.stack2[-1]

    def empty(self) -> bool:
        return not self.stack1 and not self.stack2

# Sample usage
q = MyQueue()
q.push(1)
q.push(2)
print(q.peek())  # Output: 1
print(q.pop())   # Output: 1
print(q.empty()) # Output: False
```

---

### Explanation
- `stack1` handles push operations, and `stack2` handles pop operations.
- Elements are transferred from `stack1` to `stack2` when needed, maintaining the queue order.

---

### 2. Implement Stack using Queues

#### Problem Description
Implement a stack using two queues. The stack should support `push`, `pop`, `top`, and `empty` operations.

#### Pseudocode
1. Use two queues, `queue1` for storing the elements and `queue2` as a temporary queue.
2. For `push`, add the element to `queue2` and then transfer all elements from `queue1` to `queue2`. Swap the names of `queue1` and `queue2`.
3. For `pop`, remove the element from `queue1`.
4. For `top`, return the first element of `queue1`.
5. For `empty`, check if `queue1` is empty.

---

#### Golang Solution
```go
type MyStack struct {
    queue1 []int
    queue2 []int
}

func Constructor() MyStack {
    return MyStack{}
}

func (this *MyStack) Push(x int) {
    this.queue2 = append(this.queue2, x)
    for len(this.queue1) > 0 {
        this.queue2 = append(this.queue2, this.queue1[0])
        this.queue1 = this.queue1[1:]
    }
    this.queue1, this.queue2 = this.queue2, this.queue1
}

func (this *MyStack) Pop() int {
    top := this.queue1[0]
    this.queue1 = this.queue1[1:]
    return top
}

func (this *MyStack) Top() int {
    return this.queue1[0]
}

func (this *MyStack) Empty() bool {
    return len(this.queue1) == 0
}

func main() {
    stack := Constructor()
    stack.Push(1)
    stack.Push(2)
    fmt.Println(stack.Top())   // Output: 2
    fmt.Println(stack.Pop())   // Output: 2
    fmt.Println(stack.Empty()) // Output: false
}
```

---

#### Python Solution
```python
class MyStack:

    def __init__(self):
        self.queue1 = []
        self.queue2 = []

    def push(self, x: int) -> None:
        self.queue2.append(x)
        while self.queue1:
            self.queue2.append(self.queue1.pop(0))
        self.queue1, self.queue2 = self.queue2, self.queue1

    def pop(self) -> int:
        return self.queue1.pop(0)

    def top(self) -> int:
        return self.queue1[0]

    def empty(self) -> bool:
        return not self.queue1

# Sample usage
stack = MyStack()
stack.push(1)
stack.push(2)
print(stack.top())   # Output: 2
print(stack.pop())   # Output: 2
print(stack.empty()) # Output: False
```

---

### Explanation
- `queue2` is used to reverse the order of the elements every time a new element is pushed.
- After pushing, we swap `queue1` and `queue2` to maintain the stack order in `queue1`.

---

### 3. Daily Temperatures

#### Problem Description
Given a list of daily temperatures, return a list such that, for each day in the input, tells you how many days you would have to wait until a warmer temperature. If there is no future day for which this is possible, put `0` instead.

#### Pseudocode
1. Initialize an empty stack `stack` and an array `result` of the same length as `temperatures` filled with zeros.
2. Iterate through the list of temperatures with their indices.
3. For each temperature, while the stack is not empty and the current temperature is greater than the temperature corresponding to the index at the top of the stack, calculate the number of days until the next warmer day and update `result`.
4. Push the current index onto the stack.
5. Continue until all temperatures are processed.

---

#### Golang Solution
```go
func dailyTemperatures(T []int) []int {
    stack := []int{}
    result := make([]int, len(T))

    for i := 0; i < len(T); i++ {
        for len(stack) > 0 && T[i] > T[stack[len(stack)-1]] {
            idx := stack[len(stack)-1]
            stack = stack[:len(stack)-1]
            result[idx] = i - idx
        }
        stack = append(stack, i)
    }
    return result
}

func main() {
    temperatures := []int{73, 74, 75, 71, 69, 72, 76, 73}
    fmt.Println(dailyTemperatures(temperatures)) // Output: [1, 1, 4, 2, 1, 1, 0, 0]
}
```

---

#### Python Solution
```python
def dailyTemperatures(T):
    stack = []
    result = [0] * len(T)
    
    for i, temp in enumerate(T):
        while stack and temp > T[stack[-1]]:
            idx = stack.pop()
            result[idx] = i - idx
        stack.append(i)
    
    return result

# Sample usage
temperatures = [73, 74, 75, 71, 69, 72, 76, 73]
print(dailyTemperatures(temperatures)) # Output: [1, 1, 4, 2, 1, 1, 0, 0]
```

---

### Explanation
- The stack keeps track of the indices of temperatures that have not found a warmer day yet.
- As soon as we find a warmer temperature, we calculate the days waited and update the `result` array accordingly.
- This approach ensures we only iterate through the list once, making it efficient with a time complexity of \(O(n)\).

---

### 4. Largest Rectangle in Histogram

#### Problem Description
Given an array of integers representing the heights of histogram bars, find the area of the largest rectangle that can be formed within the bounds of the histogram.

#### Pseudocode
1. Initialize an empty stack to store indices and a variable `maxArea` to store the maximum area found.
2. Iterate through the histogram heights.
3. For each height, while the stack is not empty and the current height is less than the height at the top index of the stack, pop the stack and calculate the area with the popped height as the smallest (or minimum height) of the rectangle.
4. Calculate the area as height \* (current index - stack top index - 1) if the stack is not empty, otherwise use the full width up to the current index.
5. Push the current index onto the stack.
6. After finishing the iteration, process any remaining indices in the stack similarly to step 3.
7. Return `maxArea`.

---

#### Golang Solution
```go
func largestRectangleArea(heights []int) int {
    stack := []int{}
    maxArea := 0
    heights = append(heights, 0) // Append a zero height to flush out remaining bars

    for i, h := range heights {
        for len(stack) > 0 && h < heights[stack[len(stack)-1]] {
            height := heights[stack[len(stack)-1]]
            stack = stack[:len(stack)-1]
            width := i
            if len(stack) > 0 {
                width = i - stack[len(stack)-1] - 1
            }
            maxArea = max(maxArea, height * width)
        }
        stack = append(stack, i)
    }
    return maxArea
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func main() {
    heights := []int{2, 1, 5, 6, 2, 3}
    fmt.Println(largestRectangleArea(heights)) // Output: 10
}
```

---

#### Python Solution
```python
def largestRectangleArea(heights):
    stack = []
    max_area = 0
    heights.append(0) # Append a zero height to flush out remaining bars

    for i, h in enumerate(heights):
        while stack and h < heights[stack[-1]]:
            height = heights[stack.pop()]
            width = i if not stack else i - stack[-1] - 1
            max_area = max(max_area, height * width)
        stack.append(i)

    return max_area

# Sample usage
heights = [2, 1, 5, 6, 2, 3]
print(largestRectangleArea(heights)) # Output: 10
```

---

### Explanation
- The stack keeps track of the indices of heights in an increasing order.
- When we find a height that is less than the height at the top of the stack, we calculate the area for the rectangle with the height of the bar at the stack top as the smallest height.
- The width of the rectangle is determined by the distance between the current index and the index just before the popped index.
- This algorithm efficiently calculates the largest rectangle in \(O(n)\) time.

---

### 5. Trapping Rain Water

#### Problem Description
Given an array of non-negative integers representing the elevation map where the width of each bar is 1, compute how much water it can trap after raining.

#### Pseudocode
1. Initialize two pointers, `left` and `right`, at the beginning and end of the elevation map.
2. Use two variables, `left_max` and `right_max`, to track the maximum heights from the left and right up to the current positions of the pointers.
3. Initialize a variable `trapped_water` to store the total amount of trapped water.
4. While `left` is less than `right`, do the following:
   - If the height at `left` is less than or equal to the height at `right`:
     - If the height at `left` is greater than or equal to `left_max`, update `left_max`.
     - Otherwise, add `left_max - height[left]` to `trapped_water`.
     - Move the `left` pointer one step to the right.
   - Otherwise:
     - If the height at `right` is greater than or equal to `right_max`, update `right_max`.
     - Otherwise, add `right_max - height[right]` to `trapped_water`.
     - Move the `right` pointer one step to the left.
5. Return `trapped_water`.

---

#### Golang Solution
```go
func trap(height []int) int {
    left, right := 0, len(height)-1
    left_max, right_max := 0, 0
    trapped_water := 0

    for left < right {
        if height[left] <= height[right] {
            if height[left] >= left_max {
                left_max = height[left]
            } else {
                trapped_water += left_max - height[left]
            }
            left++
        } else {
            if height[right] >= right_max {
                right_max = height[right]
            } else {
                trapped_water += right_max - height[right]
            }
            right--
        }
    }
    return trapped_water
}

func main() {
    height := []int{0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1}
    fmt.Println(trap(height)) // Output: 6
}
```

---

#### Python Solution
```python
def trap(height):
    left, right = 0, len(height) - 1
    left_max, right_max = 0, 0
    trapped_water = 0

    while left < right:
        if height[left] <= height[right]:
            if height[left] >= left_max:
                left_max = height[left]
            else:
                trapped_water += left_max - height[left]
            left += 1
        else:
            if height[right] >= right_max:
                right_max = height[right]
            else:
                trapped_water += right_max - height[right]
            right -= 1

    return trapped_water

# Sample usage
height = [0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1]
print(trap(height)) # Output: 6
```

---

### Explanation
- The algorithm uses a two-pointer approach to traverse the height array from both ends towards the center.
- By keeping track of the maximum heights seen from both the left and the right, the algorithm can calculate how much water is trapped above each bar.
- This approach ensures that each bar is processed once, resulting in a time complexity of \(O(n)\).

---
### 5. Trapping Rain Water

#### Problem Description
Given an array of non-negative integers representing the elevation map where the width of each bar is 1, compute how much water it can trap after raining.

#### Pseudocode
1. Initialize two pointers, `left` and `right`, at the beginning and end of the elevation map.
2. Use two variables, `left_max` and `right_max`, to track the maximum heights from the left and right up to the current positions of the pointers.
3. Initialize a variable `trapped_water` to store the total amount of trapped water.
4. While `left` is less than `right`, do the following:
   - If the height at `left` is less than or equal to the height at `right`:
     - If the height at `left` is greater than or equal to `left_max`, update `left_max`.
     - Otherwise, add `left_max - height[left]` to `trapped_water`.
     - Move the `left` pointer one step to the right.
   - Otherwise:
     - If the height at `right` is greater than or equal to `right_max`, update `right_max`.
     - Otherwise, add `right_max - height[right]` to `trapped_water`.
     - Move the `right` pointer one step to the left.
5. Return `trapped_water`.

---

#### Golang Solution
```go
func trap(height []int) int {
    left, right := 0, len(height)-1
    left_max, right_max := 0, 0
    trapped_water := 0

    for left < right {
        if height[left] <= height[right] {
            if height[left] >= left_max {
                left_max = height[left]
            } else {
                trapped_water += left_max - height[left]
            }
            left++
        } else {
            if height[right] >= right_max {
                right_max = height[right]
            } else {
                trapped_water += right_max - height[right]
            }
            right--
        }
    }
    return trapped_water
}

func main() {
    height := []int{0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1}
    fmt.Println(trap(height)) // Output: 6
}
```

---

#### Python Solution
```python
def trap(height):
    left, right = 0, len(height) - 1
    left_max, right_max = 0, 0
    trapped_water = 0

    while left < right:
        if height[left] <= height[right]:
            if height[left] >= left_max:
                left_max = height[left]
            else:
                trapped_water += left_max - height[left]
            left += 1
        else:
            if height[right] >= right_max:
                right_max = height[right]
            else:
                trapped_water += right_max - height[right]
            right -= 1

    return trapped_water

# Sample usage
height = [0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1]
print(trap(height)) # Output: 6
```

---

### Explanation
- The algorithm uses a two-pointer approach to traverse the height array from both ends towards the center.
- By keeping track of the maximum heights seen from both the left and the right, the algorithm can calculate how much water is trapped above each bar.
- This approach ensures that each bar is processed once, resulting in a time complexity of \(O(n)\).

---

### 6. Valid Parentheses

#### Problem Description
Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['`, and `']'`, determine if the input string is valid. An input string is valid if:
1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

#### Pseudocode
1. Create a stack to keep track of opening brackets.
2. Create a mapping of closing to opening brackets.
3. Iterate over each character in the string.
   - If the character is an opening bracket, push it onto the stack.
   - If the character is a closing bracket, check the stack:
     - If the stack is empty or the top of the stack is not the corresponding opening bracket, return `false`.
     - Otherwise, pop the stack.
4. After the iteration, if the stack is empty, return `true`, else return `false`.

---

#### Golang Solution
```go
func isValid(s string) bool {
    stack := []rune{}
    bracketMap := map[rune]rune{
        ')': '(',
        '}': '{',
        ']': '[',
    }

    for _, char := range s {
        if char == '(' || char == '{' || char == '[' {
            stack = append(stack, char)
        } else if len(stack) > 0 && stack[len(stack)-1] == bracketMap[char] {
            stack = stack[:len(stack)-1]
        } else {
            return false
        }
    }

    return len(stack) == 0
}

func main() {
    s := "()[]{}"
    fmt.Println(isValid(s)) // Output: true
}
```

---

#### Python Solution
```python
def isValid(s):
    stack = []
    bracket_map = {')': '(', '}': '{', ']': '['}

    for char in s:
        if char in bracket_map.values():
            stack.append(char)
        elif char in bracket_map.keys():
            if stack == [] or bracket_map[char] != stack.pop():
                return False
        else:
            return False

    return stack == []

# Sample usage
s = "()[]{}"
print(isValid(s)) # Output: true
```

---

### Explanation
- The algorithm uses a stack to keep track of the last seen opening bracket.
- Each closing bracket must match the type of the last opened bracket. If it doesn't, the string is immediately invalid.
- After processing all characters, the stack should be empty if all opening brackets have been properly closed.

---

### 7. Design Circular Queue

#### Problem Description
Design your implementation of the circular queue. A circular queue is a linear data structure that uses a single, fixed-size buffer as if it were connected end-to-end. Your implementation should support the following operations:
- `enQueue(value)`: Insert an element into the circular queue. Return `true` if the operation is successful.
- `deQueue()`: Delete an element from the circular queue. Return `true` if the operation is successful.
- `Front()`: Get the front item from the queue. If the queue is empty, return `-1`.
- `Rear()`: Get the last item from the queue. If the queue is empty, return `-1`.
- `isEmpty()`: Checks whether the circular queue is empty or not.
- `isFull()`: Checks whether the circular queue is full or not.

#### Pseudocode
1. Initialize the queue with a fixed size, `head`, `tail`, and `count` to keep track of the number of elements.
2. Implement each of the functions to manipulate the circular queue using modular arithmetic to wrap around the indices.

---

#### Golang Solution
```go
type MyCircularQueue struct {
    data  []int
    head  int
    tail  int
    count int
    size  int
}

func Constructor(k int) MyCircularQueue {
    return MyCircularQueue{
        data:  make([]int, k),
        head:  0,
        tail:  -1,
        count: 0,
        size:  k,
    }
}

func (this *MyCircularQueue) EnQueue(value int) bool {
    if this.IsFull() {
        return false
    }
    this.tail = (this.tail + 1) % this.size
    this.data[this.tail] = value
    this.count++
    return true
}

func (this *MyCircularQueue) DeQueue() bool {
    if this.IsEmpty() {
        return false
    }
    this.head = (this.head + 1) % this.size
    this.count--
    return true
}

func (this *MyCircularQueue) Front() int {
    if this.IsEmpty() {
        return -1
    }
    return this.data[this.head]
}

func (this *MyCircularQueue) Rear() int {
    if this.IsEmpty() {
        return -1
    }
    return this.data[this.tail]
}

func (this *MyCircularQueue) IsEmpty() bool {
    return this.count == 0
}

func (this *MyCircularQueue) IsFull() bool {
    return this.count == this.size
}

func main() {
    queue := Constructor(3)
    fmt.Println(queue.EnQueue(1)) // Output: true
    fmt.Println(queue.EnQueue(2)) // Output: true
    fmt.Println(queue.EnQueue(3)) // Output: true
    fmt.Println(queue.EnQueue(4)) // Output: false
    fmt.Println(queue.Rear())     // Output: 3
    fmt.Println(queue.IsFull())   // Output: true
    fmt.Println(queue.DeQueue())  // Output: true
    fmt.Println(queue.EnQueue(4)) // Output: true
    fmt.Println(queue.Rear())     // Output: 4
}
```

---

#### Python Solution
```python
class MyCircularQueue:

    def __init__(self, k: int):
        self.data = [-1] * k
        self.head = 0
        self.tail = -1
        self.count = 0
        self.size = k

    def enQueue(self, value: int) -> bool:
        if self.isFull():
            return False
        self.tail = (self.tail + 1) % self.size
        self.data[self.tail] = value
        self.count += 1
        return True

    def deQueue(self) -> bool:
        if self.isEmpty():
            return False
        self.head = (self.head + 1) % self.size
        self.count -= 1
        return True

    def Front(self) -> int:
        if self.isEmpty():
            return -1
        return self.data[self.head]

    def Rear(self) -> int:
        if self.isEmpty():
            return -1
        return self.data[self.tail]

    def isEmpty(self) -> bool:
        return self.count == 0

    def isFull(self) -> bool:
        return self.count == self.size

# Sample usage
queue = MyCircularQueue(3)
print(queue.enQueue(1))  # Output: True
print(queue.enQueue(2))  # Output: True
print(queue.enQueue(3))  # Output: True
print(queue.enQueue(4))  # Output: False
print(queue.Rear())      # Output: 3
print(queue.isFull())    # Output: True
print(queue.deQueue())   # Output: True
print(queue.enQueue(4))  # Output: True
print(queue.Rear())      # Output: 4
```

---

### Explanation
- This implementation uses a fixed-size array with modular arithmetic to wrap around indices.
- `EnQueue` and `DeQueue` operations ensure that the queue works in a circular manner.
- `Front` and `Rear` provide access to the first and last elements respectively.
- `IsEmpty` and `IsFull` help in checking the state of the queue.

----

### 8. Sliding Window Maximum

#### Problem Description
Given an array `nums` and a sliding window of size `k`, move the window from the left to the right of the array. Return the maximum value in each window.

#### Example
- Input: `nums = [1,3,-1,-3,5,3,6,7]`, `k = 3`
- Output: `[3,3,5,5,6,7]`
- Explanation: 
  - Window position: `[1, 3, -1] -3, 5, 3, 6, 7`, Max: `3`
  - Window position: `1, [3, -1, -3] 5, 3, 6, 7`, Max: `3`
  - Window position: `1, 3, [-1, -3, 5] 3, 6, 7`, Max: `5`
  - And so on...

#### Pseudocode
1. Initialize a deque to store indices.
2. Iterate over the elements in `nums`.
3. Maintain the deque such that it stores indices in a way that their corresponding values in `nums` are in decreasing order.
4. Remove elements from the deque that are out of the current window.
5. Append the current index to the deque.
6. If the current index is greater than or equal to `k - 1`, append the maximum value to the result list, which is the element at the front of the deque.

---

#### Golang Solution
```go
func maxSlidingWindow(nums []int, k int) []int {
    n := len(nums)
    if n == 0 {
        return []int{}
    }

    deque := []int{}
    result := []int{}

    for i := 0; i < n; i++ {
        // Remove indices that are out of the current window
        if len(deque) > 0 && deque[0] < i-k+1 {
            deque = deque[1:]
        }

        // Remove elements from the deque that are less than the current element
        for len(deque) > 0 && nums[deque[len(deque)-1]] < nums[i] {
            deque = deque[:len(deque)-1]
        }

        deque = append(deque, i)

        // Add the maximum value of the current window to the result
        if i >= k-1 {
            result = append(result, nums[deque[0]])
        }
    }

    return result
}

func main() {
    nums := []int{1, 3, -1, -3, 5, 3, 6, 7}
    k := 3
    fmt.Println(maxSlidingWindow(nums, k)) // Output: [3, 3, 5, 5, 6, 7]
}
```

---

#### Python Solution
```python
from collections import deque

def maxSlidingWindow(nums, k):
    n = len(nums)
    if n == 0:
        return []
    
    deque = deque()
    result = []

    for i in range(n):
        # Remove indices that are out of the current window
        if deque and deque[0] < i - k + 1:
            deque.popleft()

        # Remove elements from the deque that are less than the current element
        while deque and nums[deque[-1]] < nums[i]:
            deque.pop()

        deque.append(i)

        # Add the maximum value of the current window to the result
        if i >= k - 1:
            result.append(nums[deque[0]])

    return result

# Sample usage
nums = [1, 3, -1, -3, 5, 3, 6, 7]
k = 3
print(maxSlidingWindow(nums, k))  # Output: [3, 3, 5, 5, 6, 7]
```

---

### Explanation
- The deque is used to store indices of the array `nums`.
- Elements are maintained in the deque in decreasing order of their values in the array.
- The front of the deque always contains the index of the maximum element for the current window.
- The time complexity is `O(n)` since each element is added and removed from the deque at most once.


-----

### 9. Min Stack

#### Problem Description
Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the following operations:
- `push(x)` -- Push element `x` onto the stack.
- `pop()` -- Removes the element on the top of the stack.
- `top()` -- Get the top element.
- `getMin()` -- Retrieve the minimum element in the stack.

#### Example
- Input: `["MinStack","push","push","push","getMin","pop","top","getMin"]`
- Output: `[null,null,null,null,-2,null,-1,-2]`

#### Explanation:
- `MinStack` is initialized.
- `push(-2)`, `push(0)`, `push(-3)` are executed.
- `getMin()` returns `-3`.
- `pop()` removes the top element, which is `-3`.
- `top()` returns the top element, which is `0`.
- `getMin()` returns `-2`.

---

#### Pseudocode
1. Initialize two stacks:
   - `stack`: to store the elements.
   - `minStack`: to store the current minimum element at each level.
2. In the `push(x)` operation:
   - Push the element `x` to `stack`.
   - Push the smaller of `x` and the current minimum value to `minStack`.
3. In the `pop()` operation:
   - Pop from both `stack` and `minStack`.
4. In the `top()` operation:
   - Return the top element of `stack`.
5. In the `getMin()` operation:
   - Return the top element of `minStack`.

---

#### Golang Solution
```go
package main

import "fmt"

type MinStack struct {
    stack    []int
    minStack []int
}

func Constructor() MinStack {
    return MinStack{stack: []int{}, minStack: []int{}}
}

func (this *MinStack) Push(x int) {
    this.stack = append(this.stack, x)
    if len(this.minStack) == 0 || x <= this.minStack[len(this.minStack)-1] {
        this.minStack = append(this.minStack, x)
    } else {
        this.minStack = append(this.minStack, this.minStack[len(this.minStack)-1])
    }
}

func (this *MinStack) Pop() {
    this.stack = this.stack[:len(this.stack)-1]
    this.minStack = this.minStack[:len(this.minStack)-1]
}

func (this *MinStack) Top() int {
    return this.stack[len(this.stack)-1]
}

func (this *MinStack) GetMin() int {
    return this.minStack[len(this.minStack)-1]
}

func main() {
    minStack := Constructor()
    minStack.Push(-2)
    minStack.Push(0)
    minStack.Push(-3)
    fmt.Println(minStack.GetMin()) // Output: -3
    minStack.Pop()
    fmt.Println(minStack.Top())    // Output: 0
    fmt.Println(minStack.GetMin()) // Output: -2
}
```

---

#### Python Solution
```python
class MinStack:

    def __init__(self):
        self.stack = []
        self.min_stack = []

    def push(self, x: int) -> None:
        self.stack.append(x)
        if not self.min_stack or x <= self.min_stack[-1]:
            self.min_stack.append(x)
        else:
            self.min_stack.append(self.min_stack[-1])

    def pop(self) -> None:
        self.stack.pop()
        self.min_stack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return self.min_stack[-1]


# Sample usage
min_stack = MinStack()
min_stack.push(-2)
min_stack.push(0)
min_stack.push(-3)
print(min_stack.getMin())  # Output: -3
min_stack.pop()
print(min_stack.top())     # Output: 0
print(min_stack.getMin())  # Output: -2
```

---

### Explanation
- Two stacks are used:
  1. `stack`: stores all the elements.
  2. `minStack`: stores the current minimum element at each level of the stack.
- The `getMin()` operation retrieves the minimum element in constant time, as it always refers to the top of `minStack`.
- The `push()` operation ensures that the minimum element at each step is correctly recorded in `minStack`.
- The time complexity of all operations (`push`, `pop`, `top`, and `getMin`) is O(1).

----

### 10. Rotting Oranges

#### Problem Description
You are given a 2D grid representing a box of oranges. Each cell in the grid can have the following values:
- `0`: Empty cell.
- `1`: Fresh orange.
- `2`: Rotten orange.

Every minute, any fresh orange that is adjacent (4-directionally) to a rotten orange becomes rotten. Return the minimum number of minutes that must elapse until no fresh oranges remain. If this is impossible, return `-1`.

#### Example:
- Input:
  ```
  [[2,1,1],
   [1,1,0],
   [0,1,1]]
  ```
- Output: `4`

- Input:
  ```
  [[2,1,1],
   [0,1,1],
   [1,0,1]]
  ```
- Output: `-1` (because some fresh oranges will never rot).

#### Explanation:
- In the first example, after 1 minute, the rotten oranges will spread to adjacent fresh ones. The process continues, and it will take 4 minutes for all fresh oranges to rot.
- In the second example, it's impossible to make all fresh oranges rot because of the isolated fresh oranges.

---

#### Pseudocode
1. Initialize a queue to keep track of rotten oranges.
2. Count the number of fresh oranges.
3. Process the queue (breadth-first search):
   - For each rotten orange, attempt to rot its neighboring fresh oranges.
   - Keep track of the minutes that have passed.
4. After processing all possible rot actions, if there are any remaining fresh oranges, return `-1`.
5. If no fresh oranges are left, return the time taken to rot all oranges.

---

#### Golang Solution
```go
package main

import "fmt"

func orangesRotting(grid [][]int) int {
    rows, cols := len(grid), len(grid[0])
    queue := []struct{ r, c int }{}
    freshCount := 0

    // Add all rotten oranges to the queue and count fresh oranges
    for r := 0; r < rows; r++ {
        for c := 0; c < cols; c++ {
            if grid[r][c] == 2 {
                queue = append(queue, struct{ r, c int }{r, c})
            } else if grid[r][c] == 1 {
                freshCount++
            }
        }
    }

    // Directions for 4 adjacent cells
    directions := [][2]int{{-1, 0}, {1, 0}, {0, -1}, {0, 1}}

    minutes := 0
    for len(queue) > 0 && freshCount > 0 {
        nextQueue := []struct{ r, c int }{}
        for _, pos := range queue {
            for _, dir := range directions {
                nr, nc := pos.r+dir[0], pos.c+dir[1]
                if nr >= 0 && nr < rows && nc >= 0 && nc < cols && grid[nr][nc] == 1 {
                    grid[nr][nc] = 2
                    freshCount--
                    nextQueue = append(nextQueue, struct{ r, c int }{nr, nc})
                }
            }
        }
        queue = nextQueue
        if len(queue) > 0 {
            minutes++
        }
    }

    if freshCount > 0 {
        return -1
    }
    return minutes
}

func main() {
    grid1 := [][]int{
        {2, 1, 1},
        {1, 1, 0},
        {0, 1, 1},
    }
    fmt.Println(orangesRotting(grid1)) // Output: 4

    grid2 := [][]int{
        {2, 1, 1},
        {0, 1, 1},
        {1, 0, 1},
    }
    fmt.Println(orangesRotting(grid2)) // Output: -1
}
```

---

#### Python Solution
```python
from collections import deque

def orangesRotting(grid):
    rows, cols = len(grid), len(grid[0])
    queue = deque()
    fresh_count = 0
    
    # Add all rotten oranges to the queue and count fresh oranges
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 2:
                queue.append((r, c))
            elif grid[r][c] == 1:
                fresh_count += 1
    
    # Directions for 4 adjacent cells
    directions = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    
    minutes = 0
    while queue and fresh_count > 0:
        next_queue = deque()
        for r, c in queue:
            for dr, dc in directions:
                nr, nc = r + dr, c + dc
                if 0 <= nr < rows and 0 <= nc < cols and grid[nr][nc] == 1:
                    grid[nr][nc] = 2
                    fresh_count -= 1
                    next_queue.append((nr, nc))
        queue = next_queue
        if queue:
            minutes += 1
    
    return minutes if fresh_count == 0 else -1

# Sample usage
grid1 = [
    [2, 1, 1],
    [1, 1, 0],
    [0, 1, 1]
]
print(orangesRotting(grid1))  # Output: 4

grid2 = [
    [2, 1, 1],
    [0, 1, 1],
    [1, 0, 1]
]
print(orangesRotting(grid2))  # Output: -1
```

---

### Explanation
- **BFS (Breadth-First Search)** is used to simulate the spread of rot. Rotten oranges propagate to adjacent fresh ones, and the process is repeated until no more fresh oranges remain.
- **Queue** is used to store the positions of rotten oranges, and we process the grid by expanding the rotten oranges to adjacent cells.
- After processing all rotten oranges, if there are any fresh oranges left, the result is `-1`. Otherwise, the result is the number of minutes required to rot all oranges.

#### Time Complexity:
- `O(m * n)` where `m` is the number of rows and `n` is the number of columns, as each cell is processed once.

#### Space Complexity:
- `O(m * n)` for the queue used in BFS.

---

