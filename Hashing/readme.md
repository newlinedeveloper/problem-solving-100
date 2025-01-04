### Hashing

### **1. Two Sum**

#### **Problem Description**
Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

---

#### **Pseudocode**
1. Initialize an empty map `seen` to store numbers and their indices.
2. For each number `num` at index `i` in `nums`:
   - Calculate `complement = target - num`.
   - If `complement` exists in `seen`, return `[seen[complement], i]`.
   - Otherwise, store `num` and its index in `seen`.
3. If no solution is found, return an empty array.

---

#### **Golang Solution**
```go
package main

import "fmt"

func twoSum(nums []int, target int) []int {
	seen := make(map[int]int)
	for i, num := range nums {
		complement := target - num
		if idx, found := seen[complement]; found {
			return []int{idx, i}
		}
		seen[num] = i
	}
	return []int{}
}

func main() {
	nums := []int{2, 7, 11, 15}
	target := 9
	fmt.Println(twoSum(nums, target)) // Output: [0, 1]
}
```

---

#### **Python Solution**
```python
def twoSum(nums, target):
    seen = {}
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
    return []

# Test case
nums = [2, 7, 11, 15]
target = 9
print(twoSum(nums, target))  # Output: [0, 1]
```

---

### **2. First Missing Positive**

#### **Problem Description**
Given an unsorted integer array `nums`, find the smallest missing positive integer.

---

#### **Pseudocode**
1. Iterate through the array and place each number `num` at its correct index `num - 1` if it lies in the range `[1, n]`.
2. Iterate through the array again:
   - If `nums[i] != i + 1`, return `i + 1`.
3. If no missing positive is found, return `n + 1`.

---

#### **Golang Solution**
```go
package main

import "fmt"

func firstMissingPositive(nums []int) int {
	n := len(nums)
	for i := 0; i < n; i++ {
		for nums[i] > 0 && nums[i] <= n && nums[nums[i]-1] != nums[i] {
			nums[nums[i]-1], nums[i] = nums[i], nums[nums[i]-1]
		}
	}
	for i := 0; i < n; i++ {
		if nums[i] != i+1 {
			return i + 1
		}
	}
	return n + 1
}

func main() {
	nums := []int{3, 4, -1, 1}
	fmt.Println(firstMissingPositive(nums)) // Output: 2
}
```

---

#### **Python Solution**
```python
def firstMissingPositive(nums):
    n = len(nums)
    for i in range(n):
        while 1 <= nums[i] <= n and nums[nums[i] - 1] != nums[i]:
            nums[nums[i] - 1], nums[i] = nums[i], nums[nums[i] - 1]
    for i in range(n):
        if nums[i] != i + 1:
            return i + 1
    return n + 1

# Test case
nums = [3, 4, -1, 1]
print(firstMissingPositive(nums))  # Output: 2
```

---

### **3. LRU Cache**

#### **Problem Description**
Design a data structure for Least Recently Used (LRU) cache.

---

#### **Pseudocode**
1. Use a hashmap for quick lookups and a doubly linked list to maintain the order of usage.
2. When a key is accessed or added:
   - If it exists, move it to the head of the list.
   - If it doesn't exist:
     - Add it to the head of the list.
     - If capacity is exceeded, remove the least recently used element from the tail.

---

#### **Golang Solution**
```go
package main

import "fmt"

type Node struct {
	key, value int
	prev, next *Node
}

type LRUCache struct {
	capacity int
	cache    map[int]*Node
	head, tail *Node
}

func Constructor(capacity int) LRUCache {
	head := &Node{}
	tail := &Node{}
	head.next = tail
	tail.prev = head
	return LRUCache{
		capacity: capacity,
		cache:    make(map[int]*Node),
		head:     head,
		tail:     tail,
	}
}

func (lru *LRUCache) Get(key int) int {
	if node, found := lru.cache[key]; found {
		lru.moveToHead(node)
		return node.value
	}
	return -1
}

func (lru *LRUCache) Put(key int, value int) {
	if node, found := lru.cache[key]; found {
		node.value = value
		lru.moveToHead(node)
	} else {
		if len(lru.cache) == lru.capacity {
			tail := lru.popTail()
			delete(lru.cache, tail.key)
		}
		newNode := &Node{key: key, value: value}
		lru.cache[key] = newNode
		lru.addToHead(newNode)
	}
}

func (lru *LRUCache) moveToHead(node *Node) {
	lru.removeNode(node)
	lru.addToHead(node)
}

func (lru *LRUCache) removeNode(node *Node) {
	node.prev.next = node.next
	node.next.prev = node.prev
}

func (lru *LRUCache) addToHead(node *Node) {
	node.next = lru.head.next
	node.prev = lru.head
	lru.head.next.prev = node
	lru.head.next = node
}

func (lru *LRUCache) popTail() *Node {
	tail := lru.tail.prev
	lru.removeNode(tail)
	return tail
}

func main() {
	lru := Constructor(2)
	lru.Put(1, 1)
	lru.Put(2, 2)
	fmt.Println(lru.Get(1)) // Output: 1
	lru.Put(3, 3)
	fmt.Println(lru.Get(2)) // Output: -1
}
```

---

#### **Python Solution**
```python
from collections import OrderedDict

class LRUCache:
    def __init__(self, capacity: int):
        self.cache = OrderedDict()
        self.capacity = capacity

    def get(self, key: int) -> int:
        if key in self.cache:
            self.cache.move_to_end(key)
            return self.cache[key]
        return -1

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            self.cache.move_to_end(key)
        self.cache[key] = value
        if len(self.cache) > self.capacity:
            self.cache.popitem(last=False)

# Test case
lru = LRUCache(2)
lru.put(1, 1)
lru.put(2, 2)
print(lru.get(1))  # Output: 1
lru.put(3, 3)
print(lru.get(2))  # Output: -1
```

---

### **4. Number of Arithmetic Triplets**

#### **Problem Description**
Given an array `nums` and an integer `diff`, count the number of arithmetic triplets `(i, j, k)` such that:
- `nums[j] - nums[i] = diff`
- `nums[k] - nums[j] = diff`.

---

#### **Pseudocode**
1. Use a set to store visited numbers.
2. Iterate through the array:
   - For each `num`, check if `num - diff` and `num - 2 * diff` are in the set.
   - If yes, increment the count.
3. Add `num` to the set.

---

#### **Golang Solution**
```go
package main

import "fmt"

func arithmeticTriplets(nums []int, diff int) int {
	visited := make(map[int]bool)
	count := 0
	for _, num := range nums {
		if visited[num-diff] && visited[num-2*diff] {
			count++
		}
		visited[num] = true
	}
	return count
}

func main() {
	nums := []int{0, 1, 4, 6, 7, 10}
	diff := 3
	fmt.Println(arithmeticTriplets(nums, diff)) // Output: 2
}
```

---

#### **Python Solution**
```python
def arithmeticTriplets(nums, diff):
    visited = set()
    count = 0
    for num in nums:
        if num - diff in visited and num - 2 * diff in visited:
            count += 1
        visited.add(num)
    return count

# Test case
nums = [0, 1, 4, 6, 7, 10]
diff = 3
print(arithmeticTriplets(nums, diff))  # Output: 2
```
