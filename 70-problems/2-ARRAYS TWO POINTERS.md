Here are the solutions for the given **Arrays - Two Pointers** problems with explanations and sample I/O.

---

## **1. Best Time to Buy and Sell Stock**
### **Problem:**  
You are given an array `prices` where `prices[i]` is the stock price on day `i`. Find the maximum profit you can achieve. You may buy and sell **only once**.

### **Solution:**  
Use a **two-pointer** approach:
- Keep track of the **minimum price** (`min_price`).
- Calculate profit for each day and update **max_profit**.

### **Code:**
```python
def maxProfit(prices):
    min_price = float('inf')
    max_profit = 0

    for price in prices:
        min_price = min(min_price, price)  # Track min price
        max_profit = max(max_profit, price - min_price)  # Maximize profit

    return max_profit
```

### **Sample I/O:**
```python
print(maxProfit([7,1,5,3,6,4])) # Output: 5
print(maxProfit([7,6,4,3,1]))   # Output: 0
```

---

## **2. Squares of a Sorted Array**
### **Problem:**  
Given a sorted array of integers (including negatives), return an array of squares sorted in non-decreasing order.

### **Solution:**  
Use **two pointers** (`left` and `right`) from the ends of the array:
- Compare squares of `left` and `right`, adding the **largest** to the result.

### **Code:**
```python
def sortedSquares(nums):
    left, right = 0, len(nums) - 1
    result = [0] * len(nums)
    index = len(nums) - 1

    while left <= right:
        left_sq, right_sq = nums[left]**2, nums[right]**2
        if left_sq > right_sq:
            result[index] = left_sq
            left += 1
        else:
            result[index] = right_sq
            right -= 1
        index -= 1

    return result
```

### **Sample I/O:**
```python
print(sortedSquares([-4,-1,0,3,10])) # Output: [0,1,9,16,100]
print(sortedSquares([-7,-3,2,3,11])) # Output: [4,9,9,49,121]
```

---

## **3. 3Sum**
### **Problem:**  
Find all unique triplets `(nums[i], nums[j], nums[k])` such that `nums[i] + nums[j] + nums[k] = 0`.

### **Solution:**  
1. **Sort the array** to easily skip duplicates.
2. Use a **fixed pointer (`i`)** and apply a **two-pointer** approach for `j` and `k`.
3. **Skip duplicate values** to ensure unique triplets.

### **Code:**
```python
def threeSum(nums):
    nums.sort()  # Sorting helps in avoiding duplicates easily
    res = []

    for i in range(len(nums) - 2):
        if i > 0 and nums[i] == nums[i - 1]:  # Skip duplicate values
            continue

        left, right = i + 1, len(nums) - 1
        while left < right:
            s = nums[i] + nums[left] + nums[right]

            if s == 0:
                res.append([nums[i], nums[left], nums[right]])
                left += 1
                right -= 1
                while left < right and nums[left] == nums[left - 1]:  # Skip duplicates
                    left += 1
                while left < right and nums[right] == nums[right + 1]:  # Skip duplicates
                    right -= 1
            elif s < 0:
                left += 1
            else:
                right -= 1

    return res
```

### **Sample I/O:**
```python
print(threeSum([-1,0,1,2,-1,-4])) # Output: [[-1, -1, 2], [-1, 0, 1]]
print(threeSum([0,0,0,0]))        # Output: [[0,0,0]]
```

---

## **4. Longest Mountain in Array**
### **Problem:**  
Find the longest **mountain** in an array. A mountain is a sequence where:
- `A[i] < A[i+1]` (increasing part)
- `A[i] > A[i+1]` (decreasing part)

### **Solution:**  
1. Iterate over the array and find **peaks** (`A[i-1] < A[i] > A[i+1]`).
2. Expand **left** and **right** to find the full mountain.
3. Track the **maximum mountain length**.

### **Code:**
```python
def longestMountain(arr):
    n = len(arr)
    max_length = 0

    for i in range(1, n - 1):
        if arr[i - 1] < arr[i] > arr[i + 1]:  # Found a peak
            left, right = i, i

            while left > 0 and arr[left - 1] < arr[left]:  # Expand left
                left -= 1
            while right < n - 1 and arr[right] > arr[right + 1]:  # Expand right
                right += 1

            max_length = max(max_length, right - left + 1)

    return max_length if max_length >= 3 else 0
```

### **Sample I/O:**
```python
print(longestMountain([2,1,4,7,3,2,5])) # Output: 5
print(longestMountain([2,2,2]))        # Output: 0
```

---

### **Summary**
| **Problem** | **Approach** | **Time Complexity** |
|-------------|-------------|----------------------|
| Best Time to Buy and Sell Stock | Track `min_price`, update `max_profit` | O(N) |
| Squares of a Sorted Array | Two pointers from both ends | O(N) |
| 3Sum | Sort + Two Pointers | O(N²) |
| Longest Mountain in Array | Find peak, expand left & right | O(N) |
