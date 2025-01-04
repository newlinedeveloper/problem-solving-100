### Matrix

### 1. Spiral Matrix

#### Problem Description
Given an `m x n` matrix, return all elements of the matrix in spiral order.

#### Pseudocode
1. Initialize four pointers to the boundary of the matrix: `top`, `bottom`, `left`, `right`.
2. Traverse the matrix:
   - From left to right across the top.
   - From top to bottom along the right.
   - From right to left across the bottom (if still valid).
   - From bottom to top along the left (if still valid).
3. Adjust the pointers inward and repeat until all elements are traversed.

---

#### Golang Solution
```go
func spiralOrder(matrix [][]int) []int {
    if len(matrix) == 0 {
        return []int{}
    }
    top, bottom, left, right := 0, len(matrix)-1, 0, len(matrix[0])-1
    var res []int
    for top <= bottom && left <= right {
        for i := left; i <= right; i++ {
            res = append(res, matrix[top][i])
        }
        top++
        for i := top; i <= bottom; i++ {
            res = append(res, matrix[i][right])
        }
        right--
        if top <= bottom {
            for i := right; i >= left; i-- {
                res = append(res, matrix[bottom][i])
            }
            bottom--
        }
        if left <= right {
            for i := bottom; i >= top; i-- {
                res = append(res, matrix[i][left])
            }
            left++
        }
    }
    return res
}

func main() {
    matrix := [][]int{
        {1, 2, 3},
        {4, 5, 6},
        {7, 8, 9},
    }
    fmt.Println(spiralOrder(matrix)) // Output: [1 2 3 6 9 8 7 4 5]
}
```

---

#### Python Solution
```python
def spiralOrder(matrix):
    if not matrix:
        return []
    top, bottom, left, right = 0, len(matrix) - 1, 0, len(matrix[0]) - 1
    res = []
    while top <= bottom and left <= right:
        for i in range(left, right + 1):
            res.append(matrix[top][i])
        top += 1
        for i in range(top, bottom + 1):
            res.append(matrix[i][right])
        right -= 1
        if top <= bottom:
            for i in range(right, left - 1, -1):
                res.append(matrix[bottom][i])
            bottom -= 1
        if left <= right:
            for i in range(bottom, top - 1, -1):
                res.append(matrix[i][left])
            left += 1
    return res

# Sample usage
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
print(spiralOrder(matrix))  # Output: [1, 2, 3, 6, 9, 8, 7, 4, 5]
```

---

### Explanation
- We maintain four pointers representing the current bounds of the unvisited part of the matrix.
- We traverse the matrix layer by layer in a spiral order, updating the pointers accordingly after each pass.
- The process continues until all elements have been visited.

---

### 2. Set Matrix Zeroes

#### Problem Description
Given an `m x n` matrix, if an element is `0`, set its entire row and column to `0`.

#### Pseudocode
1. Use two sets to store the rows and columns that need to be zeroed.
2. Traverse the matrix to identify the rows and columns to be zeroed.
3. Traverse the matrix again to set the identified rows and columns to zero.

---

#### Golang Solution
```go
func setZeroes(matrix [][]int) {
    rows, cols := len(matrix), len(matrix[0])
    zeroRows, zeroCols := map[int]bool{}, map[int]bool{}
    for i := 0; i < rows; i++ {
        for j := 0; j < cols; j++ {
            if matrix[i][j] == 0 {
                zeroRows[i] = true
                zeroCols[j] = true
            }
        }
    }
    for i := 0; i < rows; i++ {
        for j := 0; j < cols; j++ {
            if zeroRows[i] || zeroCols[j] {
                matrix[i][j] = 0
            }
        }
    }
}

func main() {
    matrix := [][]int{
        {1, 2, 3},
        {4, 0, 6},
        {7, 8, 9},
    }
    setZeroes(matrix)
    fmt.Println(matrix) // Output: [[1 0 3] [0 0 0] [7 0 9]]
}
```

---

#### Python Solution
```python
def setZeroes(matrix):
    rows, cols = len(matrix), len(matrix[0])
    zero_rows, zero_cols = set(), set()
    for i in range(rows):
        for j in range(cols):
            if matrix[i][j] == 0:
                zero_rows.add(i)
                zero_cols.add(j)
    for i in range(rows):
        for j in range(cols):
            if i in zero_rows or j in zero_cols:
                matrix[i][j] = 0

# Sample usage
matrix = [
    [1, 2, 3],
    [4, 0, 6],
    [7, 8, 9]
]
setZeroes(matrix)
print(matrix)  # Output: [[1, 0, 3], [0, 0, 0], [7, 0, 9]]
```

---

### Explanation
- We identify rows and columns that need to be zeroed by scanning the matrix.
- We store these rows and columns in sets.
- Finally, we set the corresponding rows and columns to zero in a second pass.

---

### 3. Valid Sudoku

#### Problem Description
Determine if a `9 x 9` Sudoku board is valid. Only the filled cells need to be validated according to the rules:
1. Each row must contain the digits `1-9` without repetition.
2. Each column must contain the digits `1-9` without repetition.
3. Each of the nine `3 x 3` sub-boxes must contain the digits `1-9` without repetition.

#### Pseudocode
1. Use three sets: one for rows, one for columns, and one for sub-boxes.
2. Iterate over each cell in the board.
3. For each filled cell, check:
   - If the number already exists in the corresponding row, column, or sub-box.
   - If not, add it to the respective sets.
4. If any number repeats in a row, column, or sub-box, the board is invalid.

---

#### Golang Solution
```go
func isValidSudoku(board [][]byte) bool {
    rows := make([]map[byte]bool, 9)
    cols := make([]map[byte]bool, 9)
    boxes := make([]map[byte]bool, 9)
    
    for i := 0; i < 9; i++ {
        rows[i] = make(map[byte]bool)
        cols[i] = make(map[byte]bool)
        boxes[i] = make(map[byte]bool)
    }
    
    for i := 0; i < 9; i++ {
        for j := 0; j < 9; j++ {
            if board[i][j] == '.' {
                continue
            }
            num := board[i][j]
            boxIndex := (i/3)*3 + j/3
            if rows[i][num] || cols[j][num] || boxes[boxIndex][num] {
                return false
            }
            rows[i][num] = true
            cols[j][num] = true
            boxes[boxIndex][num] = true
        }
    }
    return true
}

func main() {
    board := [][]byte{
        {'5', '3', '.', '.', '7', '.', '.', '.', '.'},
        {'6', '.', '.', '1', '9', '5', '.', '.', '.'},
        {'.', '9', '8', '.', '.', '.', '.', '6', '.'},
        {'8', '.', '.', '.', '6', '.', '.', '.', '3'},
        {'4', '.', '.', '8', '.', '3', '.', '.', '1'},
        {'7', '.', '.', '.', '2', '.', '.', '.', '6'},
        {'.', '6', '.', '.', '.', '.', '2', '8', '.'},
        {'.', '.', '.', '4', '1', '9', '.', '.', '5'},
        {'.', '.', '.', '.', '8', '.', '.', '7', '9'},
    }
    fmt.Println(isValidSudoku(board)) // Output: true
}
```

---

#### Python Solution
```python
def isValidSudoku(board):
    rows = [set() for _ in range(9)]
    cols = [set() for _ in range(9)]
    boxes = [set() for _ in range(9)]

    for i in range(9):
        for j in range(9):
            if board[i][j] == '.':
                continue
            num = board[i][j]
            box_index = (i // 3) * 3 + j // 3
            if num in rows[i] or num in cols[j] or num in boxes[box_index]:
                return False
            rows[i].add(num)
            cols[j].add(num)
            boxes[box_index].add(num)

    return True

# Sample usage
board = [
    ['5', '3', '.', '.', '7', '.', '.', '.', '.'],
    ['6', '.', '.', '1', '9', '5', '.', '.', '.'],
    ['.', '9', '8', '.', '.', '.', '.', '6', '.'],
    ['8', '.', '.', '.', '6', '.', '.', '.', '3'],
    ['4', '.', '.', '8', '.', '3', '.', '.', '1'],
    ['7', '.', '.', '.', '2', '.', '.', '.', '6'],
    ['.', '6', '.', '.', '.', '.', '2', '8', '.'],
    ['.', '.', '.', '4', '1', '9', '.', '.', '5'],
    ['.', '.', '.', '.', '8', '.', '.', '7', '9'],
]
print(isValidSudoku(board))  # Output: True
```

---

### Explanation
- We use three sets to track the presence of each number in rows, columns, and sub-boxes.
- We calculate the sub-box index using integer division.
- If any duplication is found in the sets during iteration, the board is invalid.

---

### 4. Rotate Image

#### Problem Description
You are given an `n x n` 2D matrix representing an image. Rotate the image by 90 degrees (clockwise).

You have to rotate the image **in-place**, which means you have to modify the input 2D matrix directly.

#### Pseudocode
1. **Transpose the matrix**:
   - Swap `matrix[i][j]` with `matrix[j][i]`.
2. **Reverse each row**:
   - Reverse the elements in each row to achieve the 90-degree clockwise rotation.

---

#### Golang Solution
```go
func rotate(matrix [][]int) {
    n := len(matrix)
    // Transpose the matrix
    for i := 0; i < n; i++ {
        for j := i + 1; j < n; j++ {
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
        }
    }
    // Reverse each row
    for i := 0; i < n; i++ {
        for j := 0; j < n/2; j++ {
            matrix[i][j], matrix[i][n-j-1] = matrix[i][n-j-1], matrix[i][j]
        }
    }
}

func main() {
    matrix := [][]int{
        {1, 2, 3},
        {4, 5, 6},
        {7, 8, 9},
    }
    rotate(matrix)
    fmt.Println(matrix) // Output: [[7, 4, 1], [8, 5, 2], [9, 6, 3]]
}
```

---

#### Python Solution
```python
def rotate(matrix):
    n = len(matrix)
    # Transpose the matrix
    for i in range(n):
        for j in range(i + 1, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    # Reverse each row
    for row in matrix:
        row.reverse()

# Sample usage
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
]
rotate(matrix)
print(matrix)  # Output: [[7, 4, 1], [8, 5, 2], [9, 6, 3]]
```

---

### Explanation
- **Transpose**: The transpose of a matrix is obtained by flipping the matrix over its diagonal.
- **Reverse**: Reversing each row after the transpose gives us the desired 90-degree clockwise rotation.

---

### 5. Search a 2D Matrix

#### Problem Description
Write an efficient algorithm to search for a value in an `m x n` matrix. This matrix has the following properties:
1. Integers in each row are sorted from left to right.
2. The first integer of each row is greater than the last integer of the previous row.

#### Pseudocode
1. Treat the matrix as a sorted list.
2. Use binary search:
   - Convert the mid index into row and column indices.
   - Adjust the search range based on the mid value.

---

#### Golang Solution
```go
func searchMatrix(matrix [][]int, target int) bool {
    if len(matrix) == 0 || len(matrix[0]) == 0 {
        return false
    }
    rows, cols := len(matrix), len(matrix[0])
    left, right := 0, rows*cols-1

    for left <= right {
        mid := left + (right-left)/2
        midValue := matrix[mid/cols][mid%cols]
        if midValue == target {
            return true
        } else if midValue < target {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }
    return false
}

func main() {
    matrix := [][]int{
        {1, 3, 5, 7},
        {10, 11, 16, 20},
        {23, 30, 34, 60},
    }
    fmt.Println(searchMatrix(matrix, 3))  // Output: true
    fmt.Println(searchMatrix(matrix, 13)) // Output: false
}
```

---

#### Python Solution
```python
def searchMatrix(matrix, target):
    if not matrix or not matrix[0]:
        return False
    rows, cols = len(matrix), len(matrix[0])
    left, right = 0, rows * cols - 1

    while left <= right:
        mid = left + (right - left) // 2
        mid_value = matrix[mid // cols][mid % cols]
        if mid_value == target:
            return True
        elif mid_value < target:
            left = mid + 1
        else:
            right = mid - 1
    return False

# Sample usage
matrix = [
    [1, 3, 5, 7],
    [10, 11, 16, 20],
    [23, 30, 34, 60],
]
print(searchMatrix(matrix, 3))  # Output: True
print(searchMatrix(matrix, 13)) # Output: False
```

---

### Explanation
- We treat the matrix as a flattened list and apply binary search.
- The index calculations are adjusted to map the 1D index back to the 2D matrix.

---

### 6. Word Search

#### Problem Description
Given an `m x n` board and a word, find if the word exists in the grid. The word can be constructed from letters of sequentially adjacent cells, where "adjacent" cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

#### Pseudocode
1. Loop through each cell in the grid.
2. For each cell, start a DFS if the character matches the first character of the word.
3. During DFS, check if the current path matches the word.
4. Mark cells as visited to avoid revisiting during the same path.
5. If a path is found, return `true`.
6. If the loop finishes without finding the word, return `false`.

---

#### Golang Solution
```go
func exist(board [][]byte, word string) bool {
    rows, cols := len(board), len(board[0])
    directions := [][]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}

    var dfs func(r, c, index int) bool
    dfs = func(r, c, index int) bool {
        if index == len(word) {
            return true
        }
        if r < 0 || c < 0 || r >= rows || c >= cols || board[r][c] != word[index] {
            return false
        }
        temp := board[r][c]
        board[r][c] = '#'
        for _, d := range directions {
            if dfs(r+d[0], c+d[1], index+1) {
                return true
            }
        }
        board[r][c] = temp
        return false
    }

    for r := 0; r < rows; r++ {
        for c := 0; c < cols; c++ {
            if dfs(r, c, 0) {
                return true
            }
        }
    }
    return false
}

func main() {
    board := [][]byte{
        {'A', 'B', 'C', 'E'},
        {'S', 'F', 'C', 'S'},
        {'A', 'D', 'E', 'E'},
    }
    word := "ABCCED"
    fmt.Println(exist(board, word)) // Output: true
}
```

---

#### Python Solution
```python
def exist(board, word):
    rows, cols = len(board), len(board[0])
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]

    def dfs(r, c, index):
        if index == len(word):
            return True
        if r < 0 or c < 0 or r >= rows or c >= cols or board[r][c] != word[index]:
            return False
        temp = board[r][c]
        board[r][c] = '#'
        for dr, dc in directions:
            if dfs(r + dr, c + dc, index + 1):
                return True
        board[r][c] = temp
        return False

    for r in range(rows):
        for c in range(cols):
            if dfs(r, c, 0):
                return True
    return False

# Sample usage
board = [
    ['A', 'B', 'C', 'E'],
    ['S', 'F', 'C', 'S'],
    ['A', 'D', 'E', 'E'],
]
word = "ABCCED"
print(exist(board, word))  # Output: True
```

---

### Explanation
- The solution uses Depth-First Search (DFS) to explore all possible paths in the grid.
- Each cell is marked as visited by changing its value temporarily.
- If a path is found that matches the word, the search stops and returns `true`.

---

### 7. 01 Matrix

#### Problem Description
Given an `m x n` binary matrix, return the distance of the nearest `0` for each cell. The distance between two adjacent cells is `1`.

#### Pseudocode
1. Initialize a queue with all cells containing `0`.
2. Set cells containing `1` to infinity to signify they need to be processed.
3. Perform a multi-source BFS starting from the `0`s, updating the distances.
4. Return the updated matrix.

---

#### Golang Solution
```go
func updateMatrix(mat [][]int) [][]int {
    rows, cols := len(mat), len(mat[0])
    directions := [][]int{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}
    queue := make([][2]int, 0)

    // Initialize the queue with all 0's and set 1's to a large number
    for r := 0; r < rows; r++ {
        for c := 0; c < cols; c++ {
            if mat[r][c] == 0 {
                queue = append(queue, [2]int{r, c})
            } else {
                mat[r][c] = rows + cols
            }
        }
    }

    // BFS from all 0's
    for len(queue) > 0 {
        cell := queue[0]
        queue = queue[1:]
        r, c := cell[0], cell[1]
        for _, d := range directions {
            nr, nc := r+d[0], c+d[1]
            if nr >= 0 && nc >= 0 && nr < rows && nc < cols && mat[nr][nc] > mat[r][c]+1 {
                mat[nr][nc] = mat[r][c] + 1
                queue = append(queue, [2]int{nr, nc})
            }
        }
    }

    return mat
}

func main() {
    mat := [][]int{
        {0, 0, 0},
        {0, 1, 0},
        {1, 1, 1},
    }
    fmt.Println(updateMatrix(mat)) // Output: [[0 0 0] [0 1 0] [1 2 1]]
}
```

---

#### Python Solution
```python
from collections import deque

def updateMatrix(mat):
    rows, cols = len(mat), len(mat[0])
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
    queue = deque()

    # Initialize the queue with all 0's and set 1's to a large number
    for r in range(rows):
        for c in range(cols):
            if mat[r][c] == 0:
                queue.append((r, c))
            else:
                mat[r][c] = rows + cols

    # BFS from all 0's
    while queue:
        r, c = queue.popleft()
        for dr, dc in directions:
            nr, nc = r + dr, c + dc
            if 0 <= nr < rows and 0 <= nc < cols and mat[nr][nc] > mat[r][c] + 1:
                mat[nr][nc] = mat[r][c] + 1
                queue.append((nr, nc))

    return mat

# Sample usage
mat = [
    [0, 0, 0],
    [0, 1, 0],
    [1, 1, 1],
]
print(updateMatrix(mat))  # Output: [[0, 0, 0], [0, 1, 0], [1, 2, 1]]
```

---

### Explanation
- We use a multi-source Breadth-First Search (BFS) starting from all `0` cells simultaneously.
- Each `1` cell is updated with the shortest distance to the nearest `0` found during the BFS.

---
