### Linked List

### 1. **Reverse Linked List**

#### Problem
Reverse a singly linked list.

#### Pseudocode
```
function reverseLinkedList(head):
    prev = null
    current = head
    while current is not null:
        temp = current.next
        current.next = prev
        prev = current
        current = temp
    return prev
```

#### Golang Solution
```go
type ListNode struct {
    Val  int
    Next *ListNode
}

func reverseList(head *ListNode) *ListNode {
    var prev *ListNode
    curr := head
    for curr != nil {
        nextTemp := curr.Next
        curr.Next = prev
        prev = curr
        curr = nextTemp
    }
    return prev
}

func main() {
    head := &ListNode{1, &ListNode{2, &ListNode{3, &ListNode{4, nil}}}}
    reversed := reverseList(head)
    for reversed != nil {
        fmt.Print(reversed.Val, " ")
        reversed = reversed.Next
    }
    // Output: 4 3 2 1
}
```

#### Python Solution
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverseList(head: ListNode) -> ListNode:
    prev = None
    current = head
    while current:
        next_temp = current.next
        current.next = prev
        prev = current
        current = next_temp
    return prev

# Example Usage
head = ListNode(1, ListNode(2, ListNode(3, ListNode(4, None))))
reversed = reverseList(head)
while reversed:
    print(reversed.val, end=" ")
    reversed = reversed.next
# Output: 4 3 2 1
```

---

### 2. **Rotate List**

#### Problem
Rotate a linked list to the right by \(k\) places.

#### Pseudocode
```
function rotateRight(head, k):
    if head is null or k == 0:
        return head
    find the length of the list
    connect the tail to head to form a cycle
    break the cycle at the new head position
```

#### Golang Solution
```go
func rotateRight(head *ListNode, k int) *ListNode {
    if head == nil || head.Next == nil || k == 0 {
        return head
    }

    length := 1
    tail := head
    for tail.Next != nil {
        tail = tail.Next
        length++
    }

    k %= length
    if k == 0 {
        return head
    }

    tail.Next = head
    for i := 0; i < length-k; i++ {
        tail = tail.Next
    }

    newHead := tail.Next
    tail.Next = nil
    return newHead
}
```

#### Python Solution
```python
def rotateRight(head, k):
    if not head or not head.next or k == 0:
        return head

    length = 1
    tail = head
    while tail.next:
        tail = tail.next
        length += 1

    k %= length
    if k == 0:
        return head

    tail.next = head
    for _ in range(length - k):
        tail = tail.next

    new_head = tail.next
    tail.next = None
    return new_head
```

---

### 3. **Linked List Cycle**

#### Problem
Determine if a linked list has a cycle.

#### Pseudocode
```
function hasCycle(head):
    slow, fast = head, head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return true
    return false
```

#### Golang Solution
```go
func hasCycle(head *ListNode) bool {
    slow, fast := head, head
    for fast != nil && fast.Next != nil {
        slow = slow.Next
        fast = fast.Next.Next
        if slow == fast {
            return true
        }
    }
    return false
}
```

#### Python Solution
```python
def hasCycle(head):
    slow, fast = head, head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False
```

---

### 4. **Merge Two Sorted Lists**

#### Problem
Merge two sorted linked lists into one sorted list.

#### Pseudocode
```
function mergeTwoLists(l1, l2):
    dummy = new ListNode(-1)
    current = dummy
    while l1 and l2:
        if l1.val < l2.val:
            current.next = l1
            l1 = l1.next
        else:
            current.next = l2
            l2 = l2.next
        current = current.next
    append the remaining nodes
    return dummy.next
```

#### Golang Solution
```go
func mergeTwoLists(l1, l2 *ListNode) *ListNode {
    dummy := &ListNode{}
    current := dummy
    for l1 != nil && l2 != nil {
        if l1.Val < l2.Val {
            current.Next = l1
            l1 = l1.Next
        } else {
            current.Next = l2
            l2 = l2.Next
        }
        current = current.Next
    }
    if l1 != nil {
        current.Next = l1
    }
    if l2 != nil {
        current.Next = l2
    }
    return dummy.Next
}
```

#### Python Solution
```python
def mergeTwoLists(l1, l2):
    dummy = ListNode(-1)
    current = dummy
    while l1 and l2:
        if l1.val < l2.val:
            current.next = l1
            l1 = l1.next
        else:
            current.next = l2
            l2 = l2.next
        current = current.next
    current.next = l1 if l1 else l2
    return dummy.next
```
---
### 5. **Remove Nth Node From End of List**

#### Problem
Remove the \(n^{th}\) node from the end of a linked list.

#### Pseudocode
```
function removeNthFromEnd(head, n):
    dummy = new ListNode(0)
    dummy.next = head
    slow, fast = dummy, dummy
    move fast pointer n steps ahead
    while fast.next:
        move both slow and fast one step
    remove the nth node
    return dummy.next
```

#### Golang Solution
```go
func removeNthFromEnd(head *ListNode, n int) *ListNode {
    dummy := &ListNode{Next: head}
    slow, fast := dummy, dummy

    // Move fast pointer n steps ahead
    for i := 0; i < n; i++ {
        fast = fast.Next
    }

    // Move both pointers until fast reaches the end
    for fast.Next != nil {
        slow = slow.Next
        fast = fast.Next
    }

    // Remove the nth node
    slow.Next = slow.Next.Next
    return dummy.Next
}
```

#### Python Solution
```python
def removeNthFromEnd(head, n):
    dummy = ListNode(0)
    dummy.next = head
    slow, fast = dummy, dummy

    # Move fast pointer n steps ahead
    for _ in range(n):
        fast = fast.next

    # Move both pointers until fast reaches the end
    while fast.next:
        slow = slow.next
        fast = fast.next

    # Remove the nth node
    slow.next = slow.next.next
    return dummy.next
```

---

### 6. **Reorder List**

#### Problem
Reorder a linked list such that it follows the pattern: 
\[L_0, L_n, L_1, L_{n-1}, \dots\]

#### Pseudocode
```
function reorderList(head):
    find the middle of the list
    reverse the second half
    merge the two halves
```

#### Golang Solution
```go
func reorderList(head *ListNode) {
    if head == nil || head.Next == nil {
        return
    }

    // Step 1: Find the middle
    slow, fast := head, head
    for fast != nil && fast.Next != nil {
        slow = slow.Next
        fast = fast.Next.Next
    }

    // Step 2: Reverse the second half
    prev, curr := (*ListNode)(nil), slow.Next
    slow.Next = nil
    for curr != nil {
        nextTemp := curr.Next
        curr.Next = prev
        prev = curr
        curr = nextTemp
    }

    // Step 3: Merge two halves
    first, second := head, prev
    for second != nil {
        temp1, temp2 := first.Next, second.Next
        first.Next = second
        second.Next = temp1
        first, second = temp1, temp2
    }
}
```

#### Python Solution
```python
def reorderList(head):
    if not head or not head.next:
        return

    # Step 1: Find the middle
    slow, fast = head, head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next

    # Step 2: Reverse the second half
    prev, curr = None, slow.next
    slow.next = None
    while curr:
        next_temp = curr.next
        curr.next = prev
        prev = curr
        curr = next_temp

    # Step 3: Merge two halves
    first, second = head, prev
    while second:
        temp1, temp2 = first.next, second.next
        first.next = second
        second.next = temp1
        first, second = temp1, temp2
```

---

### 7. **Intersection of Two Linked Lists**

#### Problem
Find the node where two linked lists intersect. If no intersection exists, return `null`.

#### Pseudocode
```
function getIntersectionNode(headA, headB):
    if either list is null:
        return null
    set pointers to headA and headB
    traverse both lists, resetting to the other head when reaching the end
    return the intersection point
```

#### Golang Solution
```go
func getIntersectionNode(headA, headB *ListNode) *ListNode {
    if headA == nil || headB == nil {
        return nil
    }

    a, b := headA, headB
    for a != b {
        if a == nil {
            a = headB
        } else {
            a = a.Next
        }

        if b == nil {
            b = headA
        } else {
            b = b.Next
        }
    }
    return a
}
```

#### Python Solution
```python
def getIntersectionNode(headA, headB):
    if not headA or not headB:
        return None

    a, b = headA, headB
    while a != b:
        a = a.next if a else headB
        b = b.next if b else headA

    return a
```

---

### 8. **Reverse Nodes in k-Group**

#### Problem
Reverse every \(k\)-group of nodes in a linked list.

#### Pseudocode
```
function reverseKGroup(head, k):
    count nodes in the group
    if count < k:
        return head
    reverse the group of k nodes
    recursively process the next group
    return the new head
```

#### Golang Solution
```go
func reverseKGroup(head *ListNode, k int) *ListNode {
    count := 0
    curr := head
    for curr != nil && count < k {
        curr = curr.Next
        count++
    }

    if count == k {
        prev, curr := (*ListNode)(nil), head
        for i := 0; i < k; i++ {
            nextTemp := curr.Next
            curr.Next = prev
            prev = curr
            curr = nextTemp
        }
        head.Next = reverseKGroup(curr, k)
        return prev
    }
    return head
}
```

#### Python Solution
```python
def reverseKGroup(head, k):
    count = 0
    curr = head
    while curr and count < k:
        curr = curr.next
        count += 1

    if count == k:
        prev, curr = None, head
        for _ in range(k):
            next_temp = curr.next
            curr.next = prev
            prev = curr
            curr = next_temp
        head.next = reverseKGroup(curr, k)
        return prev

    return head
```
---

