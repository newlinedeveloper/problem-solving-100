## Searching & Sorting

### **1. Binary Search**

#### **Problem Explanation**
Binary Search is used to efficiently find the position of a target value in a sorted array. It works by repeatedly dividing the search interval in half.

#### **Pseudocode**
1. Set two pointers: `left = 0` and `right = len(array) - 1`.
2. While `left <= right`:
   - Calculate the middle index: `mid = left + (right - left) / 2`.
   - If `array[mid] == target`, return `mid`.
   - If `array[mid] < target`, adjust `left = mid + 1`.
   - Else, adjust `right = mid - 1`.
3. If the loop ends, return `-1` (target not found).

---

#### **Go Solution**
```go
func binarySearch(nums []int, target int) int {
    left, right := 0, len(nums)-1
    for left <= right {
        mid := left + (right-left)/2
        if nums[mid] == target {
            return mid
        } else if nums[mid] < target {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }
    return -1
}

// Sample Input/Output
// nums := []int{1, 3, 5, 7, 9}
// target := 5
// Output: 2
```

---

#### **Python Solution**
```python
def binary_search(nums, target):
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = left + (right - left) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1

# Sample Input/Output
# nums = [1, 3, 5, 7, 9]
# target = 5
# Output: 2
```

---

### **2. Kth Smallest Element in a Sorted Matrix**

#### **Problem Explanation**
Given a sorted matrix (rows and columns are sorted), find the k-th smallest element.

---

#### **Pseudocode**
1. Treat the matrix as a single sorted list.
2. Use Binary Search to find the k-th smallest number.
3. Use a helper function `countLessOrEqual` to count how many numbers are less than or equal to the current mid.

---

#### **Go Solution**
```go
func kthSmallest(matrix [][]int, k int) int {
    n := len(matrix)
    left, right := matrix[0][0], matrix[n-1][n-1]

    for left < right {
        mid := left + (right-left)/2
        count := countLessOrEqual(matrix, mid)
        if count < k {
            left = mid + 1
        } else {
            right = mid
        }
    }
    return left
}

func countLessOrEqual(matrix [][]int, x int) int {
    count, n := 0, len(matrix)
    row, col := n-1, 0
    for row >= 0 && col < n {
        if matrix[row][col] <= x {
            count += row + 1
            col++
        } else {
            row--
        }
    }
    return count
}

// Sample Input/Output
// matrix := [][]int{
//     {1, 5, 9},
//     {10, 11, 13},
//     {12, 13, 15},
// }
// k := 8
// Output: 13
```

---

#### **Python Solution**
```python
def kthSmallest(matrix, k):
    def countLessOrEqual(x):
        count, n = 0, len(matrix)
        row, col = n - 1, 0
        while row >= 0 and col < n:
            if matrix[row][col] <= x:
                count += row + 1
                col += 1
            else:
                row -= 1
        return count

    n = len(matrix)
    left, right = matrix[0][0], matrix[n - 1][n - 1]
    while left < right:
        mid = left + (right - left) // 2
        if countLessOrEqual(mid) < k:
            left = mid + 1
        else:
            right = mid
    return left

# Sample Input/Output
# matrix = [
#     [1, 5, 9],
#     [10, 11, 13],
#     [12, 13, 15]
# ]
# k = 8
# Output: 13
```

---

### **3. Median of Two Sorted Arrays**

#### **Problem Explanation**
Find the median of two sorted arrays. The runtime complexity should be O(log(min(len(nums1), len(nums2)))).

---

#### **Pseudocode**
1. Ensure `nums1` is the smaller array.
2. Perform binary search on `nums1`:
   - Partition both arrays at the current middle.
   - Adjust the search range until the partition satisfies median properties.

---

#### **Go Solution**
```go
func findMedianSortedArrays(nums1, nums2 []int) float64 {
    if len(nums1) > len(nums2) {
        return findMedianSortedArrays(nums2, nums1)
    }
    x, y := len(nums1), len(nums2)
    low, high := 0, x

    for low <= high {
        partitionX := (low + high) / 2
        partitionY := (x + y + 1) / 2 - partitionX

        maxX := -1 << 31
        if partitionX != 0 {
            maxX = nums1[partitionX-1]
        }

        minX := 1 << 31 - 1
        if partitionX != x {
            minX = nums1[partitionX]
        }

        maxY := -1 << 31
        if partitionY != 0 {
            maxY = nums2[partitionY-1]
        }

        minY := 1 << 31 - 1
        if partitionY != y {
            minY = nums2[partitionY]
        }

        if maxX <= minY && maxY <= minX {
            if (x+y)%2 == 0 {
                return float64(max(maxX, maxY)+min(minX, minY)) / 2
            }
            return float64(max(maxX, maxY))
        } else if maxX > minY {
            high = partitionX - 1
        } else {
            low = partitionX + 1
        }
    }
    return 0
}

// Sample Input/Output
// nums1 := []int{1, 3}
// nums2 := []int{2}
// Output: 2.0
```

---

#### **Python Solution**
```python
def findMedianSortedArrays(nums1, nums2):
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1

    x, y = len(nums1), len(nums2)
    low, high = 0, x

    while low <= high:
        partitionX = (low + high) // 2
        partitionY = (x + y + 1) // 2 - partitionX

        maxX = float('-inf') if partitionX == 0 else nums1[partitionX - 1]
        minX = float('inf') if partitionX == x else nums1[partitionX]

        maxY = float('-inf') if partitionY == 0 else nums2[partitionY - 1]
        minY = float('inf') if partitionY == y else nums2[partitionY]

        if maxX <= minY and maxY <= minX:
            if (x + y) % 2 == 0:
                return (max(maxX, maxY) + min(minX, minY)) / 2
            return max(maxX, maxY)
        elif maxX > minY:
            high = partitionX - 1
        else:
            low = partitionX + 1

# Sample Input/Output
# nums1 = [1, 3]
# nums2 = [2]
# Output: 2.0
```

---

### **4. Kth Largest Element in an Array**

#### **Problem Explanation**
Find the k-th largest element in an unsorted array.

---

#### **Pseudocode**
1. Use a heap or quick-select algorithm to find the k-th largest number efficiently.

---

#### **Python Solution**
```python
import heapq

def findKthLargest(nums, k):
    return heapq.nlargest(k, nums)[-1]

# Sample Input/Output
# nums = [3, 2, 1, 5, 6, 4]
# k = 2
# Output: 5
```

#### **Go Solution**
```go
import (
    "container/heap"
    "sort"
)

func findKthLargest(nums []int, k int) int {
    sort.Sort(sort.Reverse(sort.IntSlice(nums)))
    return nums[k-1]
}

// Sample Input/Output
// nums := []int{3, 2, 1, 5, 6, 4}
// k := 2
// Output: 5
```
