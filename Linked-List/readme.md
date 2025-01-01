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
