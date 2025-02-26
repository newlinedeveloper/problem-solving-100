### **Problem: Single Number**  
You are given a **non-empty** array of integers, where every element appears **twice** except for one. Find that single number.  

**Constraints:**  
- **Time Complexity:** Must run in **O(n)**  
- **Space Complexity:** Must use **O(1) extra space**  

---

### **Solution Approach:**
Since all numbers except one appear **twice**, we can use **XOR** (`^`) to find the unique number.

#### **Why XOR Works?**
1. `x ^ x = 0` (any number XORed with itself is 0)
2. `x ^ 0 = x` (any number XORed with 0 remains unchanged)
3. XOR is **commutative** and **associative**, meaning order does not matter.

Thus, XORing all numbers together will **cancel out** the duplicate numbers, leaving only the unique number.

---

### **Code Implementation:**
```python
def singleNumber(nums):
    result = 0
    for num in nums:
        result ^= num  # XOR all numbers
    return result
```

---

### **Example Runs:**
#### **Example 1:**
```python
print(singleNumber([2, 2, 1]))  
```
✅ **Output:**  
```plaintext
1
```

#### **Example 2:**
```python
print(singleNumber([4, 1, 2, 1, 2]))  
```
✅ **Output:**  
```plaintext
4
```

#### **Example 3:**
```python
print(singleNumber([7, 3, 5, 3, 7]))  
```
✅ **Output:**  
```plaintext
5
```

---

### **Time & Space Complexity Analysis:**
- **Time Complexity:** **O(N)** → We iterate through the array once.  
- **Space Complexity:** **O(1)** → We use a single variable (`result`).  

🔹 **Optimized & Efficient** since it avoids sorting, extra memory, or additional data structures. 🚀  

Let me know if you need further explanations! 😊
