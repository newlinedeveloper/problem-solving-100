Here are the solutions for the **Graphs - Breadth and Depth First Traversal** problems with explanations and sample I/O.

---

## **1. Clone Graph**
### **Problem:**  
Given a reference to a node in a connected **undirected graph**, return a **deep copy** of the graph.

### **Solution:**  
- Use a **DFS** or **BFS** approach to traverse the graph.
- Maintain a **hash map** to store cloned nodes.
- If a node is visited, reuse the cloned node; otherwise, create a new node.

### **Code (DFS Approach):**
```python
class Node:
    def __init__(self, val=0, neighbors=None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []

def cloneGraph(node, visited={}):
    if not node:
        return None
    if node in visited:
        return visited[node]

    clone = Node(node.val)
    visited[node] = clone

    for neighbor in node.neighbors:
        clone.neighbors.append(cloneGraph(neighbor, visited))

    return clone
```

### **Sample I/O:**
```python
# Example: 
# 1 -- 2
# |    |
# 4 -- 3
# Cloned graph should maintain the same structure

original = Node(1, [Node(2), Node(4)])
cloned = cloneGraph(original)
print(cloned.val)  # Output: 1
```

---

## **2. Core Graph Operations**
### **Problem:**  
Implement common graph operations such as **adding edges, BFS, and DFS traversal**.

### **Solution:**  
- Use an **adjacency list** representation for the graph.
- Implement **BFS and DFS** traversal methods.

### **Code:**
```python
from collections import deque

class Graph:
    def __init__(self):
        self.adj_list = {}

    def add_edge(self, u, v):
        if u not in self.adj_list:
            self.adj_list[u] = []
        if v not in self.adj_list:
            self.adj_list[v] = []
        self.adj_list[u].append(v)
        self.adj_list[v].append(u)

    def bfs(self, start):
        visited = set()
        queue = deque([start])
        result = []

        while queue:
            node = queue.popleft()
            if node not in visited:
                visited.add(node)
                result.append(node)
                queue.extend(self.adj_list[node])

        return result

    def dfs(self, start, visited=None):
        if visited is None:
            visited = set()
        visited.add(start)
        result = [start]

        for neighbor in self.adj_list[start]:
            if neighbor not in visited:
                result.extend(self.dfs(neighbor, visited))

        return result
```

### **Sample I/O:**
```python
g = Graph()
g.add_edge(1, 2)
g.add_edge(1, 3)
g.add_edge(2, 4)

print(g.bfs(1)) # Output: [1, 2, 3, 4]
print(g.dfs(1)) # Output: [1, 2, 4, 3]
```

---

## **3. Cheapest Flights Within K Stops**
### **Problem:**  
Find the **cheapest** price from city `src` to `dst` within **at most K stops**.

### **Solution:**  
- Use **Dijkstra's Algorithm** with a modified priority queue to track stops.
- Instead of the shortest distance, prioritize the **minimum cost** while tracking stops.

### **Code (BFS + Priority Queue - Dijkstra's Style):**
```python
from heapq import heappop, heappush
from collections import defaultdict

def findCheapestPrice(n, flights, src, dst, K):
    graph = defaultdict(list)
    for u, v, w in flights:
        graph[u].append((v, w))

    pq = [(0, src, K + 1)]  # (cost, node, remaining stops)
    
    while pq:
        cost, node, stops = heappop(pq)
        if node == dst:
            return cost
        if stops > 0:
            for neighbor, price in graph[node]:
                heappush(pq, (cost + price, neighbor, stops - 1))

    return -1
```

### **Sample I/O:**
```python
flights = [[0,1,100],[1,2,100],[2,3,100],[0,2,500]]
print(findCheapestPrice(4, flights, 0, 3, 1)) # Output: 200
```

---

## **4. Course Schedule**
### **Problem:**  
Determine if it is possible to finish all courses given prerequisites as a list of **directed edges**.

### **Solution:**  
- This is a **cycle detection problem** in a **Directed Graph**.
- Use **Topological Sorting** with **Kahn’s Algorithm (BFS)** or **DFS**.

### **Code (Topological Sort using BFS - Kahn’s Algorithm):**
```python
from collections import deque, defaultdict

def canFinish(numCourses, prerequisites):
    graph = defaultdict(list)
    in_degree = {i: 0 for i in range(numCourses)}

    for course, prereq in prerequisites:
        graph[prereq].append(course)
        in_degree[course] += 1

    queue = deque([course for course in in_degree if in_degree[course] == 0])
    count = 0

    while queue:
        course = queue.popleft()
        count += 1
        for neighbor in graph[course]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)

    return count == numCourses
```

### **Sample I/O:**
```python
print(canFinish(2, [[1,0]]))  # Output: True
print(canFinish(2, [[1,0],[0,1]]))  # Output: False
```

---

### **Summary**
| **Problem** | **Approach** | **Time Complexity** |
|-------------|-------------|----------------------|
| Clone Graph | DFS + Hash Map | O(N) |
| Core Graph Operations | BFS & DFS using adjacency list | O(V + E) |
| Cheapest Flights Within K Stops | Dijkstra’s Algorithm (BFS with Priority Queue) | O(E + K log V) |
| Course Schedule | Topological Sorting (BFS) | O(V + E) |

---
