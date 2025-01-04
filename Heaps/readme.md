### Heaps

### **1. Merge k Sorted Lists**

#### **Problem Description**
Given an array of `k` linked lists, each sorted in ascending order, merge all the lists into one sorted linked list.

---

#### **Pseudocode**
1. Use a priority queue (or a min-heap) to store the nodes of the lists.
2. Push the first node of each list into the heap.
3. Extract the smallest node from the heap, add it to the result list, and push its next node into the heap if it exists.
4. Continue until the heap is empty.
5. Return the merged linked list.

---

#### **Golang Solution**
```go
package main

import (
	"container/heap"
	"fmt"
)

type ListNode struct {
	Val  int
	Next *ListNode
}

type MinHeap []*ListNode

func (h MinHeap) Len() int            { return len(h) }
func (h MinHeap) Less(i, j int) bool  { return h[i].Val < h[j].Val }
func (h MinHeap) Swap(i, j int)       { h[i], h[j] = h[j], h[i] }
func (h *MinHeap) Push(x interface{}) { *h = append(*h, x.(*ListNode)) }
func (h *MinHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}

func mergeKLists(lists []*ListNode) *ListNode {
	h := &MinHeap{}
	heap.Init(h)
	for _, l := range lists {
		if l != nil {
			heap.Push(h, l)
		}
	}

	dummy := &ListNode{}
	current := dummy
	for h.Len() > 0 {
		smallest := heap.Pop(h).(*ListNode)
		current.Next = smallest
		current = current.Next
		if smallest.Next != nil {
			heap.Push(h, smallest.Next)
		}
	}
	return dummy.Next
}

func main() {
	// Sample Input: [[1->4->5], [1->3->4], [2->6]]
	l1 := &ListNode{1, &ListNode{4, &ListNode{5, nil}}}
	l2 := &ListNode{1, &ListNode{3, &ListNode{4, nil}}}
	l3 := &ListNode{2, &ListNode{6, nil}}
	lists := []*ListNode{l1, l2, l3}

	merged := mergeKLists(lists)
	for merged != nil {
		fmt.Print(merged.Val, " ")
		merged = merged.Next
	}
	// Output: 1 1 2 3 4 4 5 6
}
```

---

#### **Python Solution**
```python
import heapq

class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

    def __lt__(self, other):
        return self.val < other.val

def mergeKLists(lists):
    heap = []
    for l in lists:
        if l:
            heapq.heappush(heap, l)
    
    dummy = ListNode(0)
    current = dummy
    while heap:
        smallest = heapq.heappop(heap)
        current.next = smallest
        current = current.next
        if smallest.next:
            heapq.heappush(heap, smallest.next)
    
    return dummy.next

# Sample Input: [[1->4->5], [1->3->4], [2->6]]
l1 = ListNode(1, ListNode(4, ListNode(5)))
l2 = ListNode(1, ListNode(3, ListNode(4)))
l3 = ListNode(2, ListNode(6))
lists = [l1, l2, l3]

merged = mergeKLists(lists)
while merged:
    print(merged.val, end=" ")
    merged = merged.next
# Output: 1 1 2 3 4 4 5 6
```

---

### **2. Top K Frequent Elements**

#### **Problem Description**
Given an integer array `nums` and an integer `k`, return the `k` most frequent elements.

---

#### **Pseudocode**
1. Count the frequency of each number using a hashmap.
2. Use a max-heap (or a min-heap for the top `k` elements) to store numbers by frequency.
3. Extract the top `k` elements from the heap.
4. Return the result.

---

#### **Golang Solution**
```go
package main

import (
	"container/heap"
	"fmt"
)

type Element struct {
	Val, Freq int
}

type MaxHeap []Element

func (h MaxHeap) Len() int            { return len(h) }
func (h MaxHeap) Less(i, j int) bool  { return h[i].Freq > h[j].Freq }
func (h MaxHeap) Swap(i, j int)       { h[i], h[j] = h[j], h[i] }
func (h *MaxHeap) Push(x interface{}) { *h = append(*h, x.(Element)) }
func (h *MaxHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[:n-1]
	return x
}

func topKFrequent(nums []int, k int) []int {
	freq := make(map[int]int)
	for _, num := range nums {
		freq[num]++
	}

	h := &MaxHeap{}
	heap.Init(h)
	for val, count := range freq {
		heap.Push(h, Element{val, count})
	}

	result := make([]int, k)
	for i := 0; i < k; i++ {
		result[i] = heap.Pop(h).(Element).Val
	}
	return result
}

func main() {
	nums := []int{1, 1, 1, 2, 2, 3}
	k := 2
	fmt.Println(topKFrequent(nums, k)) // Output: [1, 2]
}
```

---

#### **Python Solution**
```python
from collections import Counter
import heapq

def topKFrequent(nums, k):
    freq = Counter(nums)
    return heapq.nlargest(k, freq.keys(), key=freq.get)

# Sample Input: nums = [1,1,1,2,2,3], k = 2
nums = [1, 1, 1, 2, 2, 3]
k = 2
print(topKFrequent(nums, k))  # Output: [1, 2]
```

---

### **3. Reorganize String**

#### **Problem Description**
Given a string `s`, rearrange the characters so that no two adjacent characters are the same. If not possible, return an empty string.

---

#### **Pseudocode**
1. Count the frequency of each character using a hashmap.
2. Use a max-heap to store characters by frequency.
3. Build the result by alternating characters with the highest frequency.
4. Return the result or an empty string if the task is impossible.

---

#### **Golang Solution**
```go
package main

import (
	"container/heap"
	"fmt"
)

type CharFreq struct {
	Char  byte
	Count int
}

type MaxHeap []CharFreq

func (h MaxHeap) Len() int            { return len(h) }
func (h MaxHeap) Less(i, j int) bool  { return h[i].Count > h[j].Count }
func (h MaxHeap) Swap(i, j int)       { h[i], h[j] = h[j], h[i] }
func (h *MaxHeap) Push(x interface{}) { *h = append(*h, x.(CharFreq)) }
func (h *MaxHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[:n-1]
	return x
}

func reorganizeString(s string) string {
	freq := make(map[byte]int)
	for i := 0; i < len(s); i++ {
		freq[s[i]]++
	}

	h := &MaxHeap{}
	heap.Init(h)
	for char, count := range freq {
		if count > (len(s)+1)/2 {
			return ""
		}
		heap.Push(h, CharFreq{char, count})
	}

	result := []byte{}
	prev := CharFreq{}
	for h.Len() > 0 {
		curr := heap.Pop(h).(CharFreq)
		result = append(result, curr.Char)
		curr.Count--
		if prev.Count > 0 {
			heap.Push(h, prev)
		}
		prev = curr
	}
	return string(result)
}

func main() {
	s := "aab"
	fmt.Println(reorganizeString(s)) // Output: "aba"
}
```

---

#### **Python Solution**
```python
from collections import Counter
import heapq

def reorganizeString(s):
    freq = Counter(s)
    max_heap = [(-count, char) for char, count in freq.items()]
    heapq.heapify(max_heap)

    prev_count, prev_char = 0, ''
    result = []
    while max_heap:
        count, char = heapq.heappop(max_heap)
        result.append(char)
        if prev_count < 0:
            heapq.heappush(max_heap, (prev_count, prev_char))
        prev_count, prev_char = count + 1, char

    return "".join(result) if len(result) == len(s) else ""

# Sample Input: s = "aab"
s = "aab"
print(reorganizeString(s))  # Output: "aba"
```

---
### **4. Find Median from Data Stream**

#### **Problem Description**
The task is to implement a data structure that supports the following operations efficiently:
1. Add a number to the data structure.
2. Find the median of the current numbers.

---

#### **Pseudocode**
1. Use two heaps: a max-heap for the lower half of the data and a min-heap for the upper half.
2. When a number is added, decide which heap to insert it into based on the median.
3. Balance the heaps such that the max-heap is allowed to have one more element than the min-heap.
4. The median can be found by checking the top of the heaps:
   - If the max-heap has more elements, the median is the top of the max-heap.
   - If the heaps have equal size, the median is the average of the tops of both heaps.

---

#### **Golang Solution**
```go
package main

import (
	"container/heap"
	"fmt"
)

type MaxHeap []int
func (h MaxHeap) Len() int            { return len(h) }
func (h MaxHeap) Less(i, j int) bool  { return h[i] > h[j] }
func (h MaxHeap) Swap(i, j int)       { h[i], h[j] = h[j], h[i] }
func (h *MaxHeap) Push(x interface{}) { *h = append(*h, x.(int)) }
func (h *MaxHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[:n-1]
	return x
}

type MinHeap []int
func (h MinHeap) Len() int            { return len(h) }
func (h MinHeap) Less(i, j int) bool  { return h[i] < h[j] }
func (h MinHeap) Swap(i, j int)       { h[i], h[j] = h[j], h[i] }
func (h *MinHeap) Push(x interface{}) { *h = append(*h, x.(int)) }
func (h *MinHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[:n-1]
	return x
}

type MedianFinder struct {
	maxHeap *MaxHeap
	minHeap *MinHeap
}

func Constructor() MedianFinder {
	maxH := &MaxHeap{}
	minH := &MinHeap{}
	heap.Init(maxH)
	heap.Init(minH)
	return MedianFinder{maxH, minH}
}

func (mf *MedianFinder) AddNum(num int) {
	heap.Push(mf.maxHeap, num)
	heap.Push(mf.minHeap, heap.Pop(mf.maxHeap).(int))
	if mf.maxHeap.Len() < mf.minHeap.Len() {
		heap.Push(mf.maxHeap, heap.Pop(mf.minHeap).(int))
	}
}

func (mf *MedianFinder) FindMedian() float64 {
	if mf.maxHeap.Len() > mf.minHeap.Len() {
		return float64((*mf.maxHeap)[0])
	}
	return (float64((*mf.maxHeap)[0]) + float64((*mf.minHeap)[0])) / 2
}

func main() {
	mf := Constructor()
	mf.AddNum(1)
	mf.AddNum(2)
	fmt.Println(mf.FindMedian()) // Output: 1.5
	mf.AddNum(3)
	fmt.Println(mf.FindMedian()) // Output: 2
}
```

---

#### **Python Solution**
```python
import heapq

class MedianFinder:
    def __init__(self):
        self.max_heap = []
        self.min_heap = []

    def addNum(self, num: int) -> None:
        heapq.heappush(self.max_heap, -num)
        heapq.heappush(self.min_heap, -heapq.heappop(self.max_heap))
        if len(self.max_heap) < len(self.min_heap):
            heapq.heappush(self.max_heap, -heapq.heappop(self.min_heap))

    def findMedian(self) -> float:
        if len(self.max_heap) > len(self.min_heap):
            return float(-self.max_heap[0])
        return (-self.max_heap[0] + self.min_heap[0]) / 2

# Sample usage:
mf = MedianFinder()
mf.addNum(1)
mf.addNum(2)
print(mf.findMedian())  # Output: 1.5
mf.addNum(3)
print(mf.findMedian())  # Output: 2
```

---

### **Explanation**
1. **Initialization**: We initialize two heaps: a max-heap (using negated values in Python) for the lower half of the numbers and a min-heap for the upper half.
2. **Adding a Number**: When a number is added, we ensure the max-heap always has the smallest half of the data and the min-heap the largest. If the max-heap's size exceeds the min-heap's size by more than one, we balance by moving the top element from the max-heap to the min-heap.
3. **Finding the Median**: The median is the top element of the max-heap if it has more elements, or the average of the tops of both heaps if they are equal in size.

