### **Problem 1: Contains Duplicate II**  
📌 **Problem Statement:**  
Given an integer array `nums` and an integer `k`, return `True` if there are **two distinct indices** `i` and `j` in the array such that:  
- `nums[i] == nums[j]`  
- `abs(i - j) <= k`  

🔹 **Constraints:**  
- **Time Complexity:** `O(N)`  
- **Space Complexity:** `O(min(N, k))`  

---

### **Solution Approach:**
We use a **HashSet (Sliding Window)** to track elements within the last `k` indices.  
1. Iterate through the array while maintaining a **window of size `k`** using a **set**.  
2. If the current number already exists in the set, return `True`.  
3. Otherwise, add the number to the set.  
4. If the set size exceeds `k`, remove the oldest element (sliding window).  

---

### **Code Implementation:**
```python
def containsNearbyDuplicate(nums, k):
    num_set = set()
    
    for i in range(len(nums)):
        if nums[i] in num_set:
            return True
        num_set.add(nums[i])

        if len(num_set) > k:
            num_set.remove(nums[i - k])  # Remove the oldest element
    
    return False
```

---

### **Example Runs:**
#### **Example 1:**
```python
print(containsNearbyDuplicate([1, 2, 3, 1], 3))
```
✅ **Output:** `True`

#### **Example 2:**
```python
print(containsNearbyDuplicate([1, 0, 1, 1], 1))
```
✅ **Output:** `True`

#### **Example 3:**
```python
print(containsNearbyDuplicate([1, 2, 3, 4, 5], 2))
```
✅ **Output:** `False`

---

### **Time & Space Complexity Analysis:**
- **Time Complexity:** **O(N)** (Each element is added and removed once)
- **Space Complexity:** **O(min(N, k))** (Set holds at most `k` elements)

---

## **Problem 2: Minimum Absolute Difference**
📌 **Problem Statement:**  
Given an array of **distinct integers**, find all pairs `(a, b)` where the absolute difference `|a - b|` is **minimum**.  

🔹 **Constraints:**  
- The answer should be returned in **ascending order**.  
- **Time Complexity:** `O(N log N)`

---

### **Solution Approach:**
1. **Sort the array** (so the closest numbers are adjacent).  
2. Iterate through the sorted array and compute absolute differences.  
3. Keep track of the **minimum difference** found.  
4. Collect all pairs `(a, b)` that have this minimum difference.

---

### **Code Implementation:**
```python
def minimumAbsDifference(arr):
    arr.sort()  # Step 1: Sort the array
    min_diff = float('inf')
    result = []

    for i in range(len(arr) - 1):
        diff = arr[i + 1] - arr[i]
        if diff < min_diff:
            min_diff = diff
            result = [[arr[i], arr[i + 1]]]
        elif diff == min_diff:
            result.append([arr[i], arr[i + 1]])

    return result
```

---

### **Example Runs:**
#### **Example 1:**
```python
print(minimumAbsDifference([4, 2, 1, 3]))
```
✅ **Output:** `[[1, 2], [2, 3], [3, 4]]`

#### **Example 2:**
```python
print(minimumAbsDifference([1, 3, 6, 10, 15]))
```
✅ **Output:** `[[1, 3]]`

#### **Example 3:**
```python
print(minimumAbsDifference([3, 8, -10, 23, 19, -4, -14, 27]))
```
✅ **Output:** `[[-14, -10], [19, 23], [23, 27]]`

---

### **Time & Space Complexity Analysis:**
- **Time Complexity:** **O(N log N)** (Sorting dominates)
- **Space Complexity:** **O(N)** (Storing pairs)

---

## **Problem 3: Minimum Size Subarray Sum**
📌 **Problem Statement:**  
Given an array of **positive integers** `nums` and a positive integer `target`, return **the length of the smallest contiguous subarray** whose sum is **greater than or equal to target**. If no such subarray exists, return `0`.  

🔹 **Constraints:**  
- **Time Complexity:** `O(N)`

---

### **Solution Approach (Sliding Window)**
1. Use **two pointers (`left`, `right`)** to track a sliding window.  
2. Expand `right` until the sum **≥ target**.  
3. Shrink `left` while sum **≥ target** to minimize the window size.  
4. Update the **minimum window size**.  

---

### **Code Implementation:**
```python
def minSubArrayLen(target, nums):
    left, sum_window = 0, 0
    min_length = float('inf')

    for right in range(len(nums)):
        sum_window += nums[right]

        while sum_window >= target:
            min_length = min(min_length, right - left + 1)
            sum_window -= nums[left]
            left += 1

    return min_length if min_length != float('inf') else 0
```

---

### **Example Runs:**
#### **Example 1:**
```python
print(minSubArrayLen(7, [2, 3, 1, 2, 4, 3]))
```
✅ **Output:** `2`  
🔹 **Explanation:** `[4, 3]` is the smallest subarray with sum ≥ `7`.

#### **Example 2:**
```python
print(minSubArrayLen(4, [1, 4, 4]))
```
✅ **Output:** `1`  
🔹 **Explanation:** `[4]` is the smallest subarray.

#### **Example 3:**
```python
print(minSubArrayLen(11, [1, 1, 1, 1, 1, 1, 1, 1]))
```
✅ **Output:** `0`  
🔹 **Explanation:** No subarray has sum ≥ `11`.

---

### **Time & Space Complexity Analysis:**
- **Time Complexity:** **O(N)** (Each element is processed at most twice)
- **Space Complexity:** **O(1)** (No extra space used)

---

### ✅ **Summary of Solutions:**
| Problem | Approach | Time Complexity | Space Complexity |
|---------|----------|----------------|----------------|
| **Contains Duplicate II** | Sliding Window + HashSet | **O(N)** | **O(k)** |
| **Minimum Absolute Difference** | Sort + One Pass | **O(N log N)** | **O(N)** |
| **Minimum Size Subarray Sum** | Sliding Window | **O(N)** | **O(1)** |

---
