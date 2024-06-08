## Arrays

### Product of Array Except Self
```
def product_except_self(nums):
    n = len(nums)
    
    # Initialize the result array with 1s
    result = [1] * n
    
    # Calculate prefix products and store them in result
    prefix = 1
    for i in range(n):
        result[i] = prefix
        prefix *= nums[i]
    
    # Calculate suffix products and multiply with the existing prefix products in result
    suffix = 1
    for i in range(n-1, -1, -1):
        result[i] *= suffix
        suffix *= nums[i]
    
    return result

# Example usage:
nums = [1, 2, 3, 4]
output = product_except_self(nums)
print(output)  # Output: [24, 12, 8, 6]


```


### Move Zeroes
```
def move_zeroes(nums):
    non_zero_index = 0

    # Move all non-zero elements to the beginning of the array
    for current in range(len(nums)):
        if nums[current] != 0:
            nums[non_zero_index] = nums[current]
            non_zero_index += 1

    # Fill the remaining array with zeros
    for i in range(non_zero_index, len(nums)):
        nums[i] = 0

# Example usage:
nums = [0, 1, 0, 3, 12]
move_zeroes(nums)
print(nums)  # Output: [1, 3, 12, 0, 0]


```

### Best Time to Buy and Sell Stock
```
def maxProfit(prices):
    if not prices:
        return 0
    
    min_price = float('inf')
    max_profit = 0

    for price in prices:
        if price < min_price:
            min_price = price
        elif price - min_price > max_profit:
            max_profit = price - min_price
    
    return max_profit

# Example usage:
prices = [7, 1, 5, 3, 6, 4]
print(maxProfit(prices))  # Output: 5

```


### Next Permutation
```
def nextPermutation(nums):
    n = len(nums)
    if n <= 1:
        return
    
    # Step 1: Find the rightmost ascent
    i = n - 2
    while i >= 0 and nums[i] >= nums[i + 1]:
        i -= 1
    
    # Step 2: Find the smallest element greater than the pivot and swap
    if i >= 0:
        j = n - 1
        while nums[j] <= nums[i]:
            j -= 1
        nums[i], nums[j] = nums[j], nums[i]
    
    # Step 3: Reverse the suffix
    nums[i + 1:] = reversed(nums[i + 1:])
    
# Example usage:
nums = [1, 2, 3]
nextPermutation(nums)
print(nums)  # Output: [1, 3, 2]


```

### Two Sum
```
def twoSum(nums, target):
    # Create a dictionary to store the value and its index
    num_to_index = {}
    
    # Iterate over the list
    for index, num in enumerate(nums):
        # Calculate the complement of the current number
        complement = target - num
        
        # Check if the complement is already in the dictionary
        if complement in num_to_index:
            # If found, return the indices
            return [num_to_index[complement], index]
        
        # Otherwise, add the number and its index to the dictionary
        num_to_index[num] = index

# Example usage:
nums = [2, 7, 11, 15]
target = 9
print(twoSum(nums, target))  # Output: [0, 1]


```

### Container With Most Water
```

def maxArea(height):
    # Initialize two pointers
    left, right = 0, len(height) - 1
    max_area = 0
    
    # Loop until the two pointers meet
    while left < right:
        # Calculate the current area
        current_height = min(height[left], height[right])
        current_width = right - left
        current_area = current_height * current_width
        
        # Update the maximum area if the current area is greater
        max_area = max(max_area, current_area)
        
        # Move the pointer pointing to the shorter line
        if height[left] < height[right]:
            left += 1
        else:
            right -= 1
    
    return max_area

# Example usage:
height = [1,8,6,2,5,4,8,3,7]
print(maxArea(height))  # Output: 49


```


### Sort Colors

```
def sortColors(nums):
    low, mid, high = 0, 0, len(nums) - 1

    while mid <= high:
        if nums[mid] == 0:
            nums[low], nums[mid] = nums[mid], nums[low]
            low += 1
            mid += 1
        elif nums[mid] == 1:
            mid += 1
        else:
            nums[high], nums[mid] = nums[mid], nums[high]
            high -= 1

# Example usage:
nums = [2,0,2,1,1,0]
sortColors(nums)
print(nums)  # Output: [0,0,1,1,2,2]


```
### Approach

To solve this problem efficiently, we can use the Dutch National Flag algorithm. We maintain three pointers:

- `low` to place 0s
- `mid` to traverse the array
- `high` to place 2s

### Steps

1. Initialize `low` to 0, `mid` to 0, and `high` to `len(nums) - 1`.
2. Traverse the array with `mid` pointer:
   - If `nums[mid]` is 0, swap `nums[mid]` and `nums[low]`, then increment both `low` and `mid`.
   - If `nums[mid]` is 1, just move `mid` to the next position.
   - If `nums[mid]` is 2, swap `nums[mid]` and `nums[high]`, then decrement `high`.

This way, we sort the array in a single pass with O(n) time complexity and O(1) space complexity.

### Explanation

1. **Initialization**:
   - We initialize `low` and `mid` to the start of the array and `high` to the end of the array.

2. **Traversal**:
   - We use a `while` loop to traverse the array with the `mid` pointer until it surpasses the `high` pointer.
   - Depending on the value of `nums[mid]`, we perform different actions:
     - If it's 0, we swap it with the value at `low` and increment both `low` and `mid`.
     - If it's 1, we simply move `mid` to the next position.
     - If it's 2, we swap it with the value at `high` and decrement `high`.


### Minimum Size Subarray Sum

```
def minSubArrayLen(target, nums):
    n = len(nums)
    left = 0
    current_sum = 0
    min_length = float('inf')
    
    for right in range(n):
        current_sum += nums[right]
        
        while current_sum >= target:
            min_length = min(min_length, right - left + 1)
            current_sum -= nums[left]
            left += 1
    
    return min_length if min_length != float('inf') else 0

# Example usage:
target = 7
nums = [2, 3, 1, 2, 4, 3]
print(minSubArrayLen(target, nums))  # Output: 2


```

### Steps

1. Initialize two pointers, `left` and `right`, both at the beginning of the array.
2. Use a variable `current_sum` to keep track of the sum of the current window.
3. Iterate with the `right` pointer to expand the window by adding `nums[right]` to `current_sum`.
4. While `current_sum` is greater than or equal to `target`, update the minimal length and contract the window from the left by subtracting `nums[left]` from `current_sum` and moving the `left` pointer to the right.
5. Continue this process until the `right` pointer has traversed the entire array.
6. If no valid subarray is found, return 0.


#### Merge Sorted Array

```
def merge(nums1, m, nums2, n):
    # Pointers for nums1 and nums2
    p1 = m - 1
    p2 = n - 1
    # Pointer for the last position of merged array
    p = m + n - 1

    # While there are elements to be checked in nums2
    while p1 >= 0 and p2 >= 0:
        if nums1[p1] > nums2[p2]:
            nums1[p] = nums1[p1]
            p1 -= 1
        else:
            nums1[p] = nums2[p2]
            p2 -= 1
        p -= 1

    # If there are still elements in nums2, place them in nums1
    # No need to check for nums1 since they are already in place
    nums1[:p2 + 1] = nums2[:p2 + 1]

# Example usage:
nums1 = [1, 2, 3, 0, 0, 0]
m = 3
nums2 = [2, 5, 6]
n = 3
merge(nums1, m, nums2, n)
print(nums1)  # Output: [1, 2, 2, 3, 5, 6]

```


### Approach

1. Use three pointers to manage the merging process:
   - `p1` pointing to the last element of the actual elements in `nums1` (i.e., `m-1`).
   - `p2` pointing to the last element of `nums2` (i.e., `n-1`).
   - `p` pointing to the last position of `nums1` (i.e., `m+n-1`).

2. Iterate from the back of `nums1` and `nums2` and fill `nums1` from the back:
   - Compare the elements pointed by `p1` and `p2`.
   - Place the larger element at the position `p` and move the respective pointer (`p1` or `p2`) backward.
   - Move the `p` pointer backward.
   
3. If there are remaining elements in `nums2`, fill them in `nums1`.





