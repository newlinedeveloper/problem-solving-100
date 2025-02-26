Here are the solutions for the **Heaps Section** problems with explanations and sample I/O.

---

## **1. Kth Largest Element in an Array**
### **Problem:**  
Find the **Kth largest element** in an unsorted array.

### **Solution:**  
- Use a **Min Heap** of size `K`.
- Maintain only `K` largest elements in the heap.
- The top element in the heap will be the **Kth largest**.

### **Code (Using Min Heap):**
```python
import heapq

def findKthLargest(nums, k):
    min_heap = []
    
    for num in nums:
        heapq.heappush(min_heap, num)
        if len(min_heap) > k:
            heapq.heappop(min_heap)
    
    return min_heap[0]
```

### **Sample I/O:**
```python
print(findKthLargest([3,2,1,5,6,4], 2)) # Output: 5
print(findKthLargest([3,2,3,1,2,4,5,5,6], 4)) # Output: 4
```

### **Time Complexity:**  
- **O(N log K)** → Heap operations (push & pop) run in **log K** time.

---

## **2. K Closest Points to Origin**
### **Problem:**  
Given a list of points `(x, y)`, find the `K` closest points to the **origin (0,0)**.

### **Solution:**  
- Compute **Euclidean Distance**:  
  \[
  d = x^2 + y^2
  \]
- Use a **Max Heap** of size `K`.
- If the heap exceeds size `K`, remove the farthest point.

### **Code (Using Max Heap):**
```python
import heapq

def kClosest(points, k):
    max_heap = []
    
    for (x, y) in points:
        dist = -(x*x + y*y)  # Use negative for max heap behavior
        heapq.heappush(max_heap, (dist, x, y))
        if len(max_heap) > k:
            heapq.heappop(max_heap)

    return [[x, y] for (_, x, y) in max_heap]
```

### **Sample I/O:**
```python
print(kClosest([[1,3],[-2,2],[5,8],[0,1]], 2)) # Output: [[-2,2], [0,1]]
```

### **Time Complexity:**  
- **O(N log K)** → Maintaining the heap.

---

## **3. Top K Frequent Elements**
### **Problem:**  
Given a list of numbers, find the `K` most **frequent elements**.

### **Solution:**  
- Count the frequency using a **hash map**.
- Use a **Min Heap** of size `K` to track top frequent elements.
- If heap size exceeds `K`, remove the least frequent element.

### **Code (Using Min Heap):**
```python
import heapq
from collections import Counter

def topKFrequent(nums, k):
    freq_map = Counter(nums)
    min_heap = []
    
    for num, freq in freq_map.items():
        heapq.heappush(min_heap, (freq, num))
        if len(min_heap) > k:
            heapq.heappop(min_heap)
    
    return [num for (freq, num) in min_heap]
```

### **Sample I/O:**
```python
print(topKFrequent([1,1,1,2,2,3], 2)) # Output: [1,2]
print(topKFrequent([1], 1)) # Output: [1]
```

### **Time Complexity:**  
- **O(N log K)** → Heap operations.

---

## **4. Task Scheduler**
### **Problem:**  
Given tasks represented by **characters** and a cooling interval `n`, schedule tasks to minimize CPU idle time.

### **Solution:**  
- Use a **Max Heap** to prioritize frequent tasks.
- Use a **Queue** to track cooldown periods.
- Process tasks using **Greedy Scheduling**.

### **Code (Using Max Heap + Queue):**
```python
import heapq
from collections import Counter, deque

def leastInterval(tasks, n):
    freq_map = Counter(tasks)
    max_heap = [-cnt for cnt in freq_map.values()]
    heapq.heapify(max_heap)
    
    time = 0
    queue = deque()  # Stores (release_time, count)
    
    while max_heap or queue:
        time += 1
        
        if max_heap:
            cnt = 1 + heapq.heappop(max_heap)  # Execute task
            if cnt:
                queue.append((time + n, cnt))  # Add back after cooldown
        
        if queue and queue[0][0] == time:
            heapq.heappush(max_heap, queue.popleft()[1])  # Reinsert task
    
    return time
```

### **Sample I/O:**
```python
print(leastInterval(["A","A","A","B","B","B"], 2)) # Output: 8
print(leastInterval(["A","C","A","B","D","B"], 2)) # Output: 6
```

### **Time Complexity:**  
- **O(N log N)** → Heap operations.

---

### **Summary**
| **Problem** | **Approach** | **Time Complexity** |
|-------------|-------------|----------------------|
| Kth Largest Element | Min Heap | O(N log K) |
| K Closest Points to Origin | Max Heap | O(N log K) |
| Top K Frequent Elements | Min Heap | O(N log K) |
| Task Scheduler | Max Heap + Queue | O(N log N) |

---
