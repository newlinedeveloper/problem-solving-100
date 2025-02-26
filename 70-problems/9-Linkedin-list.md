Here are the solutions for the **Linked Lists Section** problems with explanations and sample I/O.

---

## **1. Middle of Linked List**
### **Problem:**
Given the head of a **singly linked list**, return the **middle node**. If there are two middle nodes, return the **second one**.

### **Solution:**
- Use **two pointers**:
  - **Slow pointer** moves **1 step**.
  - **Fast pointer** moves **2 steps**.
- When **fast reaches the end**, **slow** is at the middle.

### **Code:**
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def middleNode(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow
```

### **Example:**
```python
head = ListNode(1, ListNode(2, ListNode(3, ListNode(4, ListNode(5)))))
print(middleNode(head).val)  # Output: 3
```

### **Time Complexity:** 
- **O(N)** (single traversal)

---

## **2. Linked List Cycle**
### **Problem:**
Detect if a **linked list** contains a cycle.

### **Solution:**
- Use **Floyd’s Cycle Detection Algorithm**:
  - **Fast pointer** moves **two steps**.
  - **Slow pointer** moves **one step**.
  - If they **meet**, a cycle exists.

### **Code:**
```python
def hasCycle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False
```

### **Example:**
```python
head = ListNode(1)
head.next = ListNode(2)
head.next.next = head  # Creating cycle
print(hasCycle(head))  # Output: True
```

### **Time Complexity:**
- **O(N)**

---

## **3. Reverse Linked List**
### **Problem:**
Reverse a **singly linked list**.

### **Solution:**
- Use an **iterative approach**:
  - Maintain **prev, curr** pointers.
  - Reverse links **one by one**.

### **Code:**
```python
def reverseList(head):
    prev = None
    curr = head
    while curr:
        temp = curr.next
        curr.next = prev
        prev = curr
        curr = temp
    return prev
```

### **Example:**
```python
head = ListNode(1, ListNode(2, ListNode(3)))
reversed_head = reverseList(head)
print(reversed_head.val)  # Output: 3
```

### **Time Complexity:**
- **O(N)**

---

## **4. Remove Linked List Elements**
### **Problem:**
Remove **all nodes** with a given value.

### **Solution:**
- Use a **dummy node** to handle edge cases.

### **Code:**
```python
def removeElements(head, val):
    dummy = ListNode(0)
    dummy.next = head
    curr = dummy
    while curr.next:
        if curr.next.val == val:
            curr.next = curr.next.next
        else:
            curr = curr.next
    return dummy.next
```

### **Example:**
```python
head = ListNode(1, ListNode(2, ListNode(6, ListNode(3, ListNode(6)))))
new_head = removeElements(head, 6)
print(new_head.val)  # Output: 1
```

### **Time Complexity:**
- **O(N)**

---

## **5. Reverse Linked List II**
### **Problem:**
Reverse a linked list from position `left` to `right`.

### **Solution:**
- Reverse only the **sublist**.
- Use a **dummy node**.

### **Code:**
```python
def reverseBetween(head, left, right):
    dummy = ListNode(0)
    dummy.next = head
    prev = dummy

    for _ in range(left - 1):
        prev = prev.next

    curr = prev.next
    for _ in range(right - left):
        temp = curr.next
        curr.next = temp.next
        temp.next = prev.next
        prev.next = temp

    return dummy.next
```

### **Example:**
```python
head = ListNode(1, ListNode(2, ListNode(3, ListNode(4, ListNode(5)))))
new_head = reverseBetween(head, 2, 4)
print(new_head.next.val)  # Output: 4
```

### **Time Complexity:**
- **O(N)**

---

## **6. Palindrome Linked List**
### **Problem:**
Check if a linked list is a **palindrome**.

### **Solution:**
- Use **two pointers** to find the **middle**.
- Reverse **second half**.
- Compare with **first half**.

### **Code:**
```python
def isPalindrome(head):
    slow = fast = head
    stack = []

    while fast and fast.next:
        stack.append(slow.val)
        slow = slow.next
        fast = fast.next.next

    if fast:
        slow = slow.next  # Skip middle for odd length

    while slow:
        if slow.val != stack.pop():
            return False
        slow = slow.next

    return True
```

### **Example:**
```python
head = ListNode(1, ListNode(2, ListNode(2, ListNode(1))))
print(isPalindrome(head))  # Output: True
```

### **Time Complexity:**
- **O(N)**

---

## **7. Merge Two Sorted Lists**
### **Problem:**
Merge **two sorted linked lists** into **one sorted list**.

### **Solution:**
- Use a **dummy node** to simplify merging.

### **Code:**
```python
def mergeTwoLists(l1, l2):
    dummy = ListNode(0)
    curr = dummy

    while l1 and l2:
        if l1.val < l2.val:
            curr.next = l1
            l1 = l1.next
        else:
            curr.next = l2
            l2 = l2.next
        curr = curr.next

    curr.next = l1 or l2
    return dummy.next
```

### **Example:**
```python
l1 = ListNode(1, ListNode(3, ListNode(5)))
l2 = ListNode(2, ListNode(4, ListNode(6)))
merged = mergeTwoLists(l1, l2)
print(merged.val)  # Output: 1
```

### **Time Complexity:**
- **O(N + M)**

---
