Here are the solutions for the **Backtracking Section** problems with explanations and sample I/O.

---

## **1. Letter Case Permutation**
### **Problem:**
Given a string `s`, generate **all possible** strings by changing **letter case**.

### **Solution:**
- Use **backtracking** to explore all **combinations**.
- If a **letter**, change case.
- If a **digit**, keep it the same.

### **Code:**
```python
def letterCasePermutation(s):
    res = []

    def backtrack(i, path):
        if i == len(s):
            res.append("".join(path))
            return
        if s[i].isalpha():
            path.append(s[i].lower())
            backtrack(i + 1, path)
            path.pop()
            path.append(s[i].upper())
            backtrack(i + 1, path)
            path.pop()
        else:
            path.append(s[i])
            backtrack(i + 1, path)
            path.pop()

    backtrack(0, [])
    return res
```

### **Example:**
```python
print(letterCasePermutation("a1b2"))  
# Output: ["a1b2", "a1B2", "A1b2", "A1B2"]
```

### **Time Complexity:** 
- **O(2^N)**

---

## **2. Subsets**
### **Problem:**
Given an array `nums`, return **all possible subsets (power set).**

### **Solution:**
- Use **backtracking**.
- At **each index**, either:
  - **Include** the element.
  - **Exclude** the element.

### **Code:**
```python
def subsets(nums):
    res = []

    def backtrack(i, path):
        if i == len(nums):
            res.append(path[:])
            return
        path.append(nums[i])
        backtrack(i + 1, path)
        path.pop()
        backtrack(i + 1, path)

    backtrack(0, [])
    return res
```

### **Example:**
```python
print(subsets([1,2,3]))  
# Output: [[], [1], [2], [3], [1,2], [1,3], [2,3], [1,2,3]]
```

### **Time Complexity:** 
- **O(2^N)**

---

## **3. Combinations**
### **Problem:**
Given `n` and `k`, return all possible **k-sized** combinations from `[1, 2, ..., n]`.

### **Solution:**
- Use **backtracking**.
- Stop recursion when we reach **k elements**.

### **Code:**
```python
def combine(n, k):
    res = []

    def backtrack(start, path):
        if len(path) == k:
            res.append(path[:])
            return
        for i in range(start, n + 1):
            path.append(i)
            backtrack(i + 1, path)
            path.pop()

    backtrack(1, [])
    return res
```

### **Example:**
```python
print(combine(4, 2))  
# Output: [[1,2], [1,3], [1,4], [2,3], [2,4], [3,4]]
```

### **Time Complexity:** 
- **O(C(n, k)) ≈ O(n^k)**

---

## **4. Permutations**
### **Problem:**
Given an array `nums`, return all possible **permutations**.

### **Solution:**
- Use **backtracking**.
- Swap elements to generate permutations.

### **Code:**
```python
def permute(nums):
    res = []

    def backtrack(start):
        if start == len(nums):
            res.append(nums[:])
            return
        for i in range(start, len(nums)):
            nums[start], nums[i] = nums[i], nums[start]
            backtrack(start + 1)
            nums[start], nums[i] = nums[i], nums[start]

    backtrack(0)
    return res
```

### **Example:**
```python
print(permute([1,2,3]))  
# Output: [[1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]]
```

### **Time Complexity:** 
- **O(N!)**

---

Let me know if you need more explanations! 🚀
