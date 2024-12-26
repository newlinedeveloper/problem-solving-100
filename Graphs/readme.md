### Graphs

### 1. **Number of Islands**

#### **Problem Explanation**
You are given a 2D grid of `1s` (land) and `0s` (water). An island is a group of `1s` connected horizontally or vertically. Return the number of distinct islands.

#### **Approach**
1. Iterate through each cell in the grid.
2. When a `1` is found, perform a Depth-First Search (DFS) or Breadth-First Search (BFS) to mark all connected `1s` as visited.
3. Count the number of DFS/BFS calls, as each represents a distinct island.

#### **Pseudocode**
```plaintext
function numIslands(grid):
    count = 0
    for i in range(rows of grid):
        for j in range(cols of grid):
            if grid[i][j] == '1':
                dfs(grid, i, j)
                count += 1
    return count

function dfs(grid, i, j):
    if i < 0 or j < 0 or i >= rows of grid or j >= cols of grid or grid[i][j] == '0':
        return
    grid[i][j] = '0'
    dfs(grid, i-1, j)
    dfs(grid, i+1, j)
    dfs(grid, i, j-1)
    dfs(grid, i, j+1)
```

#### **Golang Solution**
```go
func numIslands(grid [][]byte) int {
    rows, cols := len(grid), len(grid[0])
    count := 0

    var dfs func(int, int)
    dfs = func(i, j int) {
        if i < 0 || j < 0 || i >= rows || j >= cols || grid[i][j] == '0' {
            return
        }
        grid[i][j] = '0'
        dfs(i-1, j)
        dfs(i+1, j)
        dfs(i, j-1)
        dfs(i, j+1)
    }

    for i := 0; i < rows; i++ {
        for j := 0; j < cols; j++ {
            if grid[i][j] == '1' {
                count++
                dfs(i, j)
            }
        }
    }
    return count
}

grid := [][]byte{
    {'1', '1', '0', '0', '0'},
    {'1', '1', '0', '0', '0'},
    {'0', '0', '1', '0', '0'},
    {'0', '0', '0', '1', '1'},
}

o/p : 3
```
Explanation: There are three islands in the grid. The first two rows form one island, the third row has one island, and the last two rows have one island.

---

### 2. **Is Graph Bipartite?**

#### **Problem Explanation**
A graph is bipartite if the nodes can be divided into two sets such that no two nodes within the same set are connected.

#### **Approach**
1. Use BFS or DFS to color the graph.
2. If a conflict in coloring arises, the graph is not bipartite.

#### **Pseudocode**
```plaintext
function isBipartite(graph):
    colors = array of size graph initialized to -1
    for each node in graph:
        if colors[node] == -1:
            if not bfs(graph, node, colors):
                return false
    return true

function bfs(graph, node, colors):
    queue = [node]
    colors[node] = 0
    while queue is not empty:
        current = dequeue(queue)
        for neighbor in graph[current]:
            if colors[neighbor] == -1:
                colors[neighbor] = 1 - colors[current]
                enqueue(queue, neighbor)
            elif colors[neighbor] == colors[current]:
                return false
    return true
```

#### **Golang Solution**
```go
func isBipartite(graph [][]int) bool {
    colors := make([]int, len(graph))
    for i := range colors {
        colors[i] = -1
    }

    var bfs func(int) bool
    bfs = func(start int) bool {
        queue := []int{start}
        colors[start] = 0

        for len(queue) > 0 {
            current := queue[0]
            queue = queue[1:]

            for _, neighbor := range graph[current] {
                if colors[neighbor] == -1 {
                    colors[neighbor] = 1 - colors[current]
                    queue = append(queue, neighbor)
                } else if colors[neighbor] == colors[current] {
                    return false
                }
            }
        }
        return true
    }

    for i := 0; i < len(graph); i++ {
        if colors[i] == -1 && !bfs(i) {
            return false
        }
    }
    return true
}

graph := [][]int{
    {1, 3},
    {0, 2},
    {1, 3},
    {0, 2},
}

o/p: true

```
Explanation: The graph is bipartite. Nodes {0, 2} belong to one set, and nodes {1, 3} belong to the other set.

---

### 3. **Flood Fill**

#### **Problem Explanation**
Given an image represented by a 2D grid, change the color of a region starting from a given pixel.

#### **Approach**
1. Perform DFS or BFS to traverse the connected region of the starting pixel.
2. Replace the original color with the new color.

#### **Pseudocode**
```plaintext
function floodFill(image, sr, sc, newColor):
    originalColor = image[sr][sc]
    if originalColor == newColor:
        return image
    dfs(image, sr, sc, originalColor, newColor)
    return image

function dfs(image, i, j, originalColor, newColor):
    if i < 0 or j < 0 or i >= rows of image or j >= cols of image or image[i][j] != originalColor:
        return
    image[i][j] = newColor
    dfs(image, i-1, j, originalColor, newColor)
    dfs(image, i+1, j, originalColor, newColor)
    dfs(image, i, j-1, originalColor, newColor)
    dfs(image, i, j+1, originalColor, newColor)
```

#### **Golang Solution**
```go
func floodFill(image [][]int, sr int, sc int, newColor int) [][]int {
    originalColor := image[sr][sc]
    if originalColor == newColor {
        return image
    }

    var dfs func(int, int)
    dfs = func(i, j int) {
        if i < 0 || j < 0 || i >= len(image) || j >= len(image[0]) || image[i][j] != originalColor {
            return
        }
        image[i][j] = newColor
        dfs(i-1, j)
        dfs(i+1, j)
        dfs(i, j-1)
        dfs(i, j+1)
    }

    dfs(sr, sc)
    return image
}

image := [][]int{
    {1, 1, 1},
    {1, 1, 0},
    {1, 0, 1},
}
sr := 1
sc := 1
newColor := 2


o/p:
[
    [2, 2, 2],
    [2, 2, 0],
    [2, 0, 1],
]

```
Explanation: Starting from pixel (1, 1), change all connected pixels with the same color (1) to the new color (2).

---

### 4. **Longest Increasing Path in a Matrix**

#### **Problem Explanation**
Given an `m x n` matrix, find the length of the longest increasing path.

#### **Approach**
1. Use DFS with memoization to store the length of the longest path starting from each cell.
2. Check all four directions (up, down, left, right).

#### **Pseudocode**
```plaintext
function longestIncreasingPath(matrix):
    memo = matrix of same size initialized to -1
    maxPath = 0
    for i in range(rows of matrix):
        for j in range(cols of matrix):
            maxPath = max(maxPath, dfs(matrix, i, j, memo))
    return maxPath

function dfs(matrix, i, j, memo):
    if memo[i][j] != -1:
        return memo[i][j]
    length = 1
    for each direction (up, down, left, right):
        if neighbor is valid and greater than matrix[i][j]:
            length = max(length, 1 + dfs(matrix, neighbor, memo))
    memo[i][j] = length
    return length
```

#### **Golang Solution**
```go
func longestIncreasingPath(matrix [][]int) int {
    rows, cols := len(matrix), len(matrix[0])
    memo := make([][]int, rows)
    for i := range memo {
        memo[i] = make([]int, cols)
    }

    var dfs func(int, int) int
    dfs = func(i, j int) int {
        if memo[i][j] != 0 {
            return memo[i][j]
        }
        directions := [][]int{{-1, 0}, {1, 0}, {0, -1}, {0, 1}}
        maxLength := 1
        for _, dir := range directions {
            x, y := i+dir[0], j+dir[1]
            if x >= 0 && y >= 0 && x < rows && y < cols && matrix[x][y] > matrix[i][j] {
                maxLength = max(maxLength, 1+dfs(x, y))
            }
        }
        memo[i][j] = maxLength
        return maxLength
    }

    maxPath := 0
    for i := 0; i < rows; i++ {
        for j := 0; j < cols; j++ {
            maxPath = max(maxPath, dfs(i, j))
        }
    }
    return maxPath
}

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

matrix := [][]int{
    {9, 9, 4},
    {6, 6, 8},
    {2, 1, 1},
}

o/p: 4

``` 
Explanation: The longest increasing path is [1 → 2 → 6 → 9], which has a length of 4.

---
### **1. Cheapest Flights Within K Stops**

#### **Problem Explanation**
You are given:
- `n`: The number of cities.
- `flights`: A list of `edges` where `flights[i] = [from, to, price]`.
- `src`: The starting city.
- `dst`: The destination city.
- `k`: The maximum number of stops.

The task is to find the cheapest price to travel from `src` to `dst` within `k` stops. If it is not possible, return `-1`.

---

#### **Pseudocode**
1. Create an adjacency list for the graph.
2. Use a priority queue or min-heap to store `(cost, city, stops_left)`.
3. Start with `(0, src, k + 1)` in the queue.
4. While the queue is not empty:
   - Pop the current city.
   - If it is the destination, return the cost.
   - If stops are available:
     - Traverse its neighbors and push `(current_cost + price, neighbor, stops - 1)` to the queue.
5. If no path is found, return `-1`.

---

#### **Golang Solution**
```go
package main

import (
	"container/heap"
	"fmt"
	"math"
)

type Flight struct {
	city, cost, stops int
}

type MinHeap []Flight

func (h MinHeap) Len() int           { return len(h) }
func (h MinHeap) Less(i, j int) bool { return h[i].cost < h[j].cost }
func (h MinHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *MinHeap) Push(x interface{}) {
	*h = append(*h, x.(Flight))
}

func (h *MinHeap) Pop() interface{} {
	old := *h
	n := len(old)
	item := old[n-1]
	*h = old[:n-1]
	return item
}

func findCheapestPrice(n int, flights [][]int, src int, dst int, k int) int {
	graph := make(map[int][][]int)

	for _, flight := range flights {
		from, to, price := flight[0], flight[1], flight[2]
		graph[from] = append(graph[from], []int{to, price})
	}

	minHeap := &MinHeap{}
	heap.Push(minHeap, Flight{src, 0, k + 1})

	for minHeap.Len() > 0 {
		curr := heap.Pop(minHeap).(Flight)
		city, cost, stops := curr.city, curr.cost, curr.stops

		if city == dst {
			return cost
		}

		if stops > 0 {
			for _, neighbor := range graph[city] {
				nextCity, price := neighbor[0], neighbor[1]
				heap.Push(minHeap, Flight{nextCity, cost + price, stops - 1})
			}
		}
	}

	return -1
}

func main() {
	n := 4
	flights := [][]int{
		{0, 1, 100},
		{1, 2, 100},
		{2, 3, 100},
		{0, 3, 500},
	}
	src, dst, k := 0, 3, 1
	fmt.Println(findCheapestPrice(n, flights, src, dst, k)) // Output: 200
}
```

---

### **2. Number of Provinces**

#### **Problem Explanation**
- You are given an adjacency matrix `isConnected`, where `isConnected[i][j] = 1` indicates a direct connection between cities `i` and `j`.
- A province is a group of cities that are directly or indirectly connected.

The task is to find the number of provinces.

---

#### **Pseudocode**
1. Create a `visited` array to track visited cities.
2. Iterate through each city:
   - If the city is not visited:
     - Start a DFS or BFS from that city and mark all reachable cities as visited.
     - Increment the province count.
3. Return the province count.

---

#### **Golang Solution**
```go
package main

import "fmt"

func findCircleNum(isConnected [][]int) int {
	n := len(isConnected)
	visited := make([]bool, n)

	var dfs func(city int)
	dfs = func(city int) {
		visited[city] = true
		for neighbor := 0; neighbor < n; neighbor++ {
			if isConnected[city][neighbor] == 1 && !visited[neighbor] {
				dfs(neighbor)
			}
		}
	}

	provinces := 0
	for i := 0; i < n; i++ {
		if !visited[i] {
			provinces++
			dfs(i)
		}
	}

	return provinces
}

func main() {
	isConnected := [][]int{
		{1, 1, 0},
		{1, 1, 0},
		{0, 0, 1},
	}
	fmt.Println(findCircleNum(isConnected)) // Output: 2
}
```

---

### **Sample Input and Output**

#### **Cheapest Flights Within K Stops**
**Input:**
```plaintext
n = 4, flights = [[0,1,100],[1,2,100],[2,3,100],[0,3,500]]
src = 0, dst = 3, k = 1
```
**Output:**
```plaintext
200
```

#### **Number of Provinces**
**Input:**
```plaintext
isConnected = [[1,1,0],[1,1,0],[0,0,1]]
```
**Output:**
```plaintext
2
```

---

