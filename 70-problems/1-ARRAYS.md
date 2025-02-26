### Arrays
---

## **1. Contains Duplicate**
### **Problem:**  
Given an integer array `nums`, return `true` if any value appears at least twice in the array, and `false` if every element is distinct.

### **Solution:**
Use a `set` to track seen numbers. If a number appears again, return `true`.

### **Code:**
```python
def containsDuplicate(nums):
    seen = set()
    for num in nums:
        if num in seen:
            return True
        seen.add(num)
    return False
```

### **Sample I/O:**
```python
print(containsDuplicate([1,2,3,1])) # Output: True
print(containsDuplicate([1,2,3,4])) # Output: False
```

---

## **2. Missing Number**
### **Problem:**  
Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return the missing number.

### **Solution:**  
Use the sum formula:  
`Sum of first n natural numbers - sum of given numbers`

### **Code:**
```python
def missingNumber(nums):
    n = len(nums)
    return (n * (n + 1)) // 2 - sum(nums)
```

### **Sample I/O:**
```python
print(missingNumber([3,0,1])) # Output: 2
print(missingNumber([0,1])) # Output: 2
```

---

## **3. Find All Numbers Disappeared in an Array**
### **Problem:**  
Given an array of `n` numbers (1 to n), return all missing numbers.

### **Solution:**  
Mark numbers as negative at their corresponding indices.

### **Code:**
```python
def findDisappearedNumbers(nums):
    for num in nums:
        index = abs(num) - 1
        nums[index] = -abs(nums[index])
    
    return [i + 1 for i in range(len(nums)) if nums[i] > 0]
```

### **Sample I/O:**
```python
print(findDisappearedNumbers([4,3,2,7,8,2,3,1])) # Output: [5, 6]
```

---

## **4. Two Sum**
### **Problem:**  
Find two indices in an array such that their sum equals a given target.

### **Solution:**  
Use a dictionary to store the required complement.

### **Code:**
```python
def twoSum(nums, target):
    seen = {}
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
```

### **Sample I/O:**
```python
print(twoSum([2,7,11,15], 9)) # Output: [0, 1]
```

---

## **5. Final Value After Operations**
### **Problem:**  
Given operations like `X++`, `--X`, return the final value of `X`, starting at `0`.

### **Solution:**  
Iterate and modify `X` accordingly.

### **Code:**
```python
def finalValueAfterOperations(operations):
    X = 0
    for op in operations:
        if "+" in op:
            X += 1
        else:
            X -= 1
    return X
```

### **Sample I/O:**
```python
print(finalValueAfterOperations(["--X","X++","X++"])) # Output: 1
```

---

## **6. How Many Numbers Are Smaller Than the Current Number**
### **Problem:**  
For each number in the array, count how many numbers are smaller.

### **Solution:**  
Sort and use dictionary mapping.

### **Code:**
```python
def smallerNumbersThanCurrent(nums):
    sorted_nums = sorted(nums)
    rank = {num: i for i, num in enumerate(sorted_nums)}
    return [rank[num] for num in nums]
```

### **Sample I/O:**
```python
print(smallerNumbersThanCurrent([8,1,2,2,3])) # Output: [4,0,1,1,3]
```

---

## **7. Minimum Time Visiting All Points**
### **Problem:**  
Find the minimum time to visit all points in a 2D plane, moving in 8 directions.

### **Solution:**  
Use `max(|x2 - x1|, |y2 - y1|)` for each step.

### **Code:**
```python
def minTimeToVisitAllPoints(points):
    time = 0
    for i in range(len(points) - 1):
        x1, y1 = points[i]
        x2, y2 = points[i + 1]
        time += max(abs(x2 - x1), abs(y2 - y1))
    return time
```

### **Sample I/O:**
```python
print(minTimeToVisitAllPoints([[1,1],[3,4],[-1,0]])) # Output: 7
```

---

## **8. Spiral Matrix**
### **Problem:**  
Traverse a matrix in spiral order.

### **Solution:**  
Use four boundaries and iterate.

### **Code:**
```python
def spiralOrder(matrix):
    result = []
    while matrix:
        result += matrix.pop(0)
        matrix = list(zip(*matrix))[::-1] # Rotate remaining matrix counter-clockwise
    return result
```

### **Sample I/O:**
```python
print(spiralOrder([[1,2,3],[4,5,6],[7,8,9]])) # Output: [1,2,3,6,9,8,7,4,5]
```

---

## **9. Number of Islands**
### **Problem:**  
Find the number of islands (connected `1s`) in a binary grid.

### **Solution:**  
Use DFS to mark visited islands.

### **Code:**
```python
def numIslands(grid):
    if not grid:
        return 0
    rows, cols = len(grid), len(grid[0])
    count = 0
    
    def dfs(r, c):
        if 0 <= r < rows and 0 <= c < cols and grid[r][c] == "1":
            grid[r][c] = "0"
            dfs(r+1, c)
            dfs(r-1, c)
            dfs(r, c+1)
            dfs(r, c-1)
    
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == "1":
                count += 1
                dfs(r, c)
    
    return count
```

### **Sample I/O:**
```python
grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
print(numIslands(grid)) # Output: 1
```

---
