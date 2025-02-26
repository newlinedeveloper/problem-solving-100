Here are the solutions for the **Dynamic Programming (DP) section** problems with explanations and sample input/output.

---

## **1. Coin Change**
### **Problem:**
Given an array of `coins` of different denominations and an integer `amount`, return the **fewest** number of coins to make up that amount. If it's not possible, return `-1`.

### **Solution:**
- Use **Dynamic Programming (DP)** with a **bottom-up approach**.
- Create a **dp array** where `dp[i]` represents the **minimum number of coins** to make `i`.
- Base case: `dp[0] = 0` (0 coins needed for amount `0`).
- Transition: `dp[i] = min(dp[i], dp[i - coin] + 1) for each coin`.

### **Code:**
```python
def coinChange(coins, amount):
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0

    for coin in coins:
        for i in range(coin, amount + 1):
            dp[i] = min(dp[i], dp[i - coin] + 1)

    return dp[amount] if dp[amount] != float('inf') else -1
```

### **Example:**
```python
print(coinChange([1,2,5], 11))  
# Output: 3  (5+5+1)
```

### **Time Complexity:** 
- **O(N * M)** (N = amount, M = number of coins)

---

## **2. Climbing Stairs**
### **Problem:**
You can climb **1 or 2** steps at a time. Given `n` stairs, find **how many ways** you can reach the top.

### **Solution:**
- **DP relation:** `dp[i] = dp[i-1] + dp[i-2]`
- Base case:
  - `dp[1] = 1`
  - `dp[2] = 2`

### **Code:**
```python
def climbStairs(n):
    if n <= 2:
        return n
    a, b = 1, 2
    for _ in range(3, n + 1):
        a, b = b, a + b
    return b
```

### **Example:**
```python
print(climbStairs(5))  
# Output: 8  (1,1,1,1,1 or 1,1,1,2 or 2,2,1, etc.)
```

### **Time Complexity:** 
- **O(N)**

---

## **3. Maximum Subarray (Kadane's Algorithm)**
### **Problem:**
Find the **largest sum** of a contiguous subarray.

### **Solution:**
- Use **Kadane’s Algorithm**:
  - `curr_sum = max(nums[i], curr_sum + nums[i])`
  - `max_sum = max(max_sum, curr_sum)`

### **Code:**
```python
def maxSubArray(nums):
    max_sum = nums[0]
    curr_sum = nums[0]

    for i in range(1, len(nums)):
        curr_sum = max(nums[i], curr_sum + nums[i])
        max_sum = max(max_sum, curr_sum)

    return max_sum
```

### **Example:**
```python
print(maxSubArray([-2,1,-3,4,-1,2,1,-5,4]))  
# Output: 6  (subarray [4,-1,2,1])
```

### **Time Complexity:** 
- **O(N)**

---

## **4. Counting Bits**
### **Problem:**
Given `n`, return an array where `ans[i]` is the **number of 1s in the binary representation** of `i`.

### **Solution:**
- **DP approach**:
  - `dp[i] = dp[i >> 1] + (i & 1)`

### **Code:**
```python
def countBits(n):
    dp = [0] * (n + 1)
    for i in range(1, n + 1):
        dp[i] = dp[i >> 1] + (i & 1)
    return dp
```

### **Example:**
```python
print(countBits(5))  
# Output: [0,1,1,2,1,2]  (0 -> 0, 1 -> 1, 2 -> 10, 3 -> 11, etc.)
```

### **Time Complexity:** 
- **O(N)**

---

## **5. Range Sum Query - Immutable**
### **Problem:**
Given an array `nums`, implement a function `sumRange(i, j)` that returns the **sum of elements** from `nums[i]` to `nums[j]` (inclusive).

### **Solution:**
- **Precompute prefix sums**:
  - `prefix[i] = prefix[i-1] + nums[i]`
  - `sumRange(i, j) = prefix[j+1] - prefix[i]`

### **Code:**
```python
class NumArray:
    def __init__(self, nums):
        self.prefix = [0] * (len(nums) + 1)
        for i in range(len(nums)):
            self.prefix[i + 1] = self.prefix[i] + nums[i]

    def sumRange(self, i, j):
        return self.prefix[j + 1] - self.prefix[i]
```

### **Example:**
```python
arr = NumArray([-2, 0, 3, -5, 2, -1])
print(arr.sumRange(0,2))  
# Output: 1  (-2 + 0 + 3)
```

### **Time Complexity:** 
- **O(1) for sum query**  
- **O(N) for initialization**

---
