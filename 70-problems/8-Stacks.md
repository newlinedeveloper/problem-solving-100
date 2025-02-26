Here are the solutions for the **Stacks Section** problems with explanations and sample I/O.

---

## **1. Min Stack**
### **Problem:**
Design a **stack** that supports:
- `push(x)`: Push element `x` onto the stack.
- `pop()`: Remove top element.
- `top()`: Get the top element.
- `getMin()`: Retrieve the minimum element in **O(1)**.

### **Solution:**
- Maintain two stacks:
  - **Main stack (`stack`)** to store elements.
  - **Min stack (`min_stack`)** to track the minimum.

### **Code:**
```python
class MinStack:
    def __init__(self):
        self.stack = []
        self.min_stack = []

    def push(self, x: int) -> None:
        self.stack.append(x)
        if not self.min_stack or x <= self.min_stack[-1]:
            self.min_stack.append(x)

    def pop(self) -> None:
        if self.stack.pop() == self.min_stack[-1]:
            self.min_stack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return self.min_stack[-1]
```

### **Example:**
```python
minStack = MinStack()
minStack.push(-2)
minStack.push(0)
minStack.push(-3)
print(minStack.getMin())  # Output: -3
minStack.pop()
print(minStack.top())     # Output: 0
print(minStack.getMin())  # Output: -2
```

### **Time Complexity:**
- **O(1)** for `push`, `pop`, `top`, and `getMin`.

---

## **2. Valid Parentheses**
### **Problem:**
Given a string **containing only `()[]{}`**, determine if it is **valid**.

### **Solution:**
- Use a **stack**.
- Push **opening brackets** into the stack.
- For **closing brackets**, check if they match the last opening bracket.

### **Code:**
```python
def isValid(s):
    stack = []
    mapping = {')': '(', '}': '{', ']': '['}

    for char in s:
        if char in mapping:
            top_element = stack.pop() if stack else '#'
            if mapping[char] != top_element:
                return False
        else:
            stack.append(char)

    return not stack
```

### **Example:**
```python
print(isValid("()[]{}"))  # Output: True
print(isValid("(]"))      # Output: False
print(isValid("{[]}"))    # Output: True
```

### **Time Complexity:**
- **O(N)**

---

## **3. Evaluate Reverse Polish Notation (RPN)**
### **Problem:**
Evaluate an expression in **Reverse Polish Notation (Postfix Notation)**.

### **Solution:**
- Use a **stack**.
- Push **numbers** onto the stack.
- When encountering an **operator**, pop the last **two numbers**, evaluate, and push result.

### **Code:**
```python
def evalRPN(tokens):
    stack = []
    operators = {'+', '-', '*', '/'}

    for token in tokens:
        if token in operators:
            b = stack.pop()
            a = stack.pop()
            if token == '+':
                stack.append(a + b)
            elif token == '-':
                stack.append(a - b)
            elif token == '*':
                stack.append(a * b)
            elif token == '/':
                stack.append(int(a / b))  # Ensure truncation towards zero
        else:
            stack.append(int(token))

    return stack[0]
```

### **Example:**
```python
print(evalRPN(["2", "1", "+", "3", "*"]))  # Output: 9
print(evalRPN(["4", "13", "5", "/", "+"]))  # Output: 6
```

### **Time Complexity:**
- **O(N)**

---

## **4. Stack Sorting**
### **Problem:**
Sort a **stack** in **ascending order** using **another stack**.

### **Solution:**
- Use a temporary stack to store sorted elements.

### **Code:**
```python
def sortStack(stack):
    tempStack = []

    while stack:
        curr = stack.pop()
        while tempStack and tempStack[-1] > curr:
            stack.append(tempStack.pop())
        tempStack.append(curr)

    while tempStack:
        stack.append(tempStack.pop())

    return stack
```

### **Example:**
```python
stack = [34, 3, 31, 98, 92, 23]
print(sortStack(stack))  # Output: [3, 23, 31, 34, 92, 98]
```

### **Time Complexity:**
- **O(N²)** (due to nested loops)

---
