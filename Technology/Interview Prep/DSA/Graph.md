## Graph Representation

Before diving into the patterns, let's briefly touch upon the two most common ways to represent a graph:

1. **Adjacency Matrix**:
    - A 2D array (list of lists in Python) where `matrix[u][v]` represents the weight of the edge between nodes `u` and `v`.
    - For an unweighted graph, you can use a boolean matrix, where `matrix[u][v]` is `True` if there is an edge and `False` otherwise.
2. **Adjacency List**:
    - A dictionary (in Python) where each key is a node, and the value is a list of its neighbors.
    - More space-efficient for sparse graphs.

Choose the representation that best suits your problem.

---

## Graph Class in Python

Below is a sample Graph class implemented in Python using an adjacency list. The dictionary key is a node, and the value is a list of its neighbors.

```python
class Graph:
    def __init__(self, is_bidirectional=True):
        self.adj_list = {}
        self.is_bidirectional = is_bidirectional

    def add_vertex(self, vertex):
        if vertex not in self.adj_list:
            self.adj_list[vertex] = []

    def add_edge(self, source, destination):
        if source not in self.adj_list:
            self.add_vertex(source)
        if destination not in self.adj_list:
            self.add_vertex(destination)

        self.adj_list[source].append(destination)

        if self.is_bidirectional:
            self.adj_list[destination].append(source)

    def get_adj_list(self):
        return self.adj_list
```

---

## Graph Traversal

### 1. Depth-First Search (DFS)

#### **DFS (Iterative)**

```python
def dfs_iterative(graph, source):
    """
    Iterative DFS that prints the nodes as they are visited.
    :param graph: Graph object.
    :param source: Starting node for DFS.
    """
    stack = [source]
    visited = set()

    while stack:
        current_node = stack.pop()
        if current_node not in visited:
            visited.add(current_node)
            print(current_node)

            # Push all neighbors onto the stack
            for neighbor in graph.get_adj_list()[current_node]:
                if neighbor not in visited:
                    stack.append(neighbor)
```

#### **DFS (Recursive)**

```python
def dfs_recursive(graph, source, visited=None):
    """
    Recursive DFS that prints the nodes as they are visited.
    :param graph: Graph object.
    :param source: Starting node for DFS.
    :param visited: Set to keep track of visited nodes.
    """
    if visited is None:
        visited = set()

    visited.add(source)
    print(source)

    for neighbor in graph.get_adj_list()[source]:
        if neighbor not in visited:
            dfs_recursive(graph, neighbor, visited)
```

### 2. Breadth-First Search (BFS)

```python
from collections import deque

def bfs(graph, source):
    """
    BFS that prints the nodes level by level.
    :param graph: Graph object.
    :param source: Starting node for BFS.
    """
    visited = set()
    queue = deque([source])
    visited.add(source)
    level = 0

    while queue:
        level_size = len(queue)
        print(f"Level {level}: ", end="")

        for _ in range(level_size):
            current_node = queue.popleft()
            print(current_node, end=" ")

            for neighbor in graph.get_adj_list()[current_node]:
                if neighbor not in visited:
                    visited.add(neighbor)
                    queue.append(neighbor)
        print()
        level += 1
```

---

## Traversal Pattern Questions

### 1. Number of Connected Components (for an Undirected Graph)

```python
def count_connected_components(graph):
    """
    Counts how many connected components are in the graph.
    :param graph: Graph object (undirected).
    :return: Number of connected components.
    """
    visited = set()
    component_count = 0

    for node in graph.get_adj_list().keys():
        if node not in visited:
            if explore_dfs(graph, node, visited):
                component_count += 1

    return component_count

def explore_dfs(graph, current_node, visited):
    if current_node in visited:
        return False

    visited.add(current_node)
    for neighbor in graph.get_adj_list()[current_node]:
        explore_dfs(graph, neighbor, visited)
    return True
```

### 2. Detect Cycle in an Undirected Graph

```python
def detect_cycle_undirected(num_vertices, adjacency_list):
    """
    Detect if a cycle exists in an undirected graph.
    :param num_vertices: Number of vertices in the graph.
    :param adjacency_list: A list of lists where adjacency_list[u] contains neighbors of u.
    :return: True if cycle is found, False otherwise.
    """
    visited = set()

    for vertex in range(num_vertices):
        if vertex not in visited:
            if cycle_dfs_undirected(adjacency_list, vertex, -1, visited):
                return True
    return False

def cycle_dfs_undirected(adjacency_list, current_vertex, parent_vertex, visited):
    visited.add(current_vertex)

    for neighbor in adjacency_list[current_vertex]:
        if neighbor not in visited:
            if cycle_dfs_undirected(adjacency_list, neighbor, current_vertex, visited):
                return True
        # If neighbor is visited and not the parent, it's a cycle
        elif neighbor != parent_vertex:
            return True
    return False
```

### 3. Detect Cycle in a Directed Graph

```python
def detect_cycle_directed(num_vertices, adjacency_list):
    """
    Detect cycle in a directed graph using DFS and recursion stack.
    :param num_vertices: Number of vertices in the graph.
    :param adjacency_list: adjacency_list[u] = list of neighbors of u.
    :return: True if the directed graph has a cycle, False otherwise.
    """
    visited = [False] * num_vertices
    recursion_stack = [False] * num_vertices

    for vertex in range(num_vertices):
        if not visited[vertex]:
            if cycle_dfs_directed(adjacency_list, vertex, visited, recursion_stack):
                return True
    return False

def cycle_dfs_directed(adjacency_list, vertex, visited, recursion_stack):
    if recursion_stack[vertex]:
        return True
    if visited[vertex]:
        return False

    visited[vertex] = True
    recursion_stack[vertex] = True

    for neighbor in adjacency_list[vertex]:
        if cycle_dfs_directed(adjacency_list, neighbor, visited, recursion_stack):
            return True

    recursion_stack[vertex] = False
    return False
```

---

## Topological Sort

### 1. Topological Sort (DFS)

```python
def topological_sort_dfs(num_vertices, adjacency_list):
    """
    Performs topological sort using DFS.
    :param num_vertices: Number of vertices in the graph.
    :param adjacency_list: adjacency_list[u] = list of neighbors of u.
    :return: A list of vertices in topologically sorted order.
    """
    visited = set()
    result_stack = []

    def dfs_topo(vertex):
        if vertex in visited:
            return
        visited.add(vertex)

        for neighbor in adjacency_list[vertex]:
            if neighbor not in visited:
                dfs_topo(neighbor)

        # After visiting all neighbors of 'vertex', push it to the stack
        result_stack.append(vertex)

    for node_index in range(num_vertices):
        if node_index not in visited:
            dfs_topo(node_index)

    # The stack has the topological order in reverse
    return result_stack[::-1]
```

### 2. Topological Sort (Kahn’s Algorithm / BFS)

```python
from collections import deque

def topological_sort_kahn(num_vertices, adjacency_list):
    """
    Performs topological sort using Kahn's Algorithm (BFS approach).
    :param num_vertices: Number of vertices.
    :param adjacency_list: adjacency_list[u] = list of neighbors of u.
    :return: List of vertices in topologically sorted order.
    """
    in_degree = [0] * num_vertices
    # Calculate in-degrees of all vertices
    for u in range(num_vertices):
        for v in adjacency_list[u]:
            in_degree[v] += 1

    # Queue for all vertices with in-degree 0
    queue = deque([i for i in range(num_vertices) if in_degree[i] == 0])
    topo_order = []

    while queue:
        current_vertex = queue.popleft()
        topo_order.append(current_vertex)

        for neighbor in adjacency_list[current_vertex]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)

    return topo_order
```

---

## Grid Pattern

### 1. Number of Islands (DFS in a grid)

```python
def count_islands(grid):
    """
    grid: 2D list of characters 'L' (Land) or 'W' (Water).
    Return: Number of islands.
    """
    rows = len(grid)
    cols = len(grid[0]) if rows > 0 else 0
    visited = set()
    island_count = 0

    def explore(r, c):
        # Check boundaries
        if r < 0 or r >= rows or c < 0 or c >= cols:
            return False
        if grid[r][c] == 'W':
            return False

        pos_key = (r, c)
        if pos_key in visited:
            return False

        visited.add(pos_key)
        # Explore neighbors (up, down, left, right)
        explore(r - 1, c)
        explore(r + 1, c)
        explore(r, c - 1)
        explore(r, c + 1)
        return True

    for row_index in range(rows):
        for col_index in range(cols):
            if explore(row_index, col_index):
                island_count += 1

    return island_count
```

### 2. Rotting Oranges (BFS in a grid)

```python
from collections import deque

def oranges_rotting(grid):
    """
    grid: 2D list of integers:
          0 - Empty cell
          1 - Fresh orange
          2 - Rotten orange
    Return: Minimum time needed to rot all oranges, or -1 if it's impossible.
    """
    rows = len(grid)
    cols = len(grid[0]) if rows > 0 else 0
    queue = deque()
    fresh_oranges = 0
    minutes_elapsed = 0

    # Collect initial rotten oranges and count fresh ones
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 2:
                queue.append((r, c))
            elif grid[r][c] == 1:
                fresh_oranges += 1

    directions = [(0, 1), (0, -1), (1, 0), (-1, 0)]

    while queue and fresh_oranges > 0:
        for _ in range(len(queue)):
            row_index, col_index = queue.popleft()

            for dr, dc in directions:
                new_r = row_index + dr
                new_c = col_index + dc
                # Check boundaries
                if 0 <= new_r < rows and 0 <= new_c < cols and grid[new_r][new_c] == 1:
                    grid[new_r][new_c] = 2
                    queue.append((new_r, new_c))
                    fresh_oranges -= 1

        minutes_elapsed += 1

    return minutes_elapsed if fresh_oranges == 0 else -1
```

---

## Shortest Path

### 1. Shortest Path in an Unweighted Graph (BFS)

```python
def shortest_path_unweighted(graph, start, end):
    """
    Find the shortest distance between start and end in an unweighted graph using BFS.
    :param graph: Graph object.
    :param start: Start node.
    :param end: Destination node.
    :return: Integer distance from start to end, -1 if not found.
    """
    from collections import deque
    queue = deque([(start, 0)])
    visited = set([start])

    while queue:
        current_node, distance = queue.popleft()

        if current_node == end:
            return distance

        for neighbor in graph.get_adj_list()[current_node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append((neighbor, distance + 1))

    return -1
```

### 2. Shortest Path in a Weighted DAG (via Topological Sort)

```python
def shortest_path_weighted_dag(num_vertices, edges, source):
    """
    Finds the shortest path in a weighted DAG from a single source using Topological Sort.
    :param num_vertices: Number of vertices.
    :param edges: List of edges [u, v, w] meaning there is an edge u->v with weight w.
    :param source: Source vertex.
    :return: List of shortest distances from source to each vertex; None for unreachable vertices.
    """
    # Build adjacency list: graph[u] = [(v, w), (v2, w2), ...]
    graph_adj = {i: [] for i in range(num_vertices)}
    for u, v, w in edges:
        graph_adj[u].append((v, w))

    # Topological Sort using DFS
    visited = set()
    topo_stack = []

    def dfs_topo_dag(node):
        visited.add(node)
        for neighbor, weight in graph_adj[node]:
            if neighbor not in visited:
                dfs_topo_dag(neighbor)
        topo_stack.append(node)

    for vertex in range(num_vertices):
        if vertex not in visited:
            dfs_topo_dag(vertex)

    topo_order = topo_stack[::-1]  # Reverse to get proper topological order

    # Initialize distances
    distances = [None] * num_vertices
    distances[source] = 0

    for node in topo_order:
        if distances[node] is not None:
            for neighbor, weight in graph_adj[node]:
                new_distance = distances[node] + weight
                if distances[neighbor] is None:
                    distances[neighbor] = new_distance
                else:
                    distances[neighbor] = min(distances[neighbor], new_distance)

    return distances
```

### 3. Dijkstra’s Algorithm

```python
import heapq

def dijkstra(graph_adj, source, num_vertices):
    """
    Dijkstra's Algorithm to find the shortest path in a weighted graph without negative weights.
    :param graph_adj: Dictionary {u: [(v, w), (v2, w2)]} for edges u->v with weight w.
    :param source: Source vertex.
    :param num_vertices: Number of vertices.
    :return: List of min distances from source to every vertex.
    """
    distances = [float('inf')] * num_vertices
    distances[source] = 0

    # Min-heap (priority queue) of tuples (distance, node)
    pq = [(0, source)]

    while pq:
        current_distance, current_node = heapq.heappop(pq)

        # If this distance is outdated, skip it
        if current_distance > distances[current_node]:
            continue

        for neighbor, weight in graph_adj[current_node]:
            distance_through_current = distances[current_node] + weight
            if distance_through_current < distances[neighbor]:
                distances[neighbor] = distance_through_current
                heapq.heappush(pq, (distance_through_current, neighbor))

    return distances
```

### 4. Bellman-Ford Algorithm

```python
def bellman_ford(num_vertices, edges, source):
    """
    Bellman-Ford Algorithm to detect negative cycles and find shortest paths.
    :param num_vertices: Number of vertices.
    :param edges: List of edges [u, v, weight].
    :param source: Source vertex.
    :return: List of distances; [-1] if a negative cycle is detected.
    """
    INF = 10**9
    distances = [INF] * num_vertices
    distances[source] = 0

    # Relax edges (num_vertices - 1) times
    for _ in range(num_vertices - 1):
        for u, v, w in edges:
            if distances[u] != INF and distances[u] + w < distances[v]:
                distances[v] = distances[u] + w

    # Check for negative-weight cycles
    for u, v, w in edges:
        if distances[u] != INF and distances[u] + w < distances[v]:
            return [-1]  # Indicates negative cycle

    return distances
```

### 5. Floyd-Warshall Algorithm

```python
def floyd_warshall(matrix):
    """
    Floyd Warshall Algorithm for all-pairs shortest paths.
    matrix: 2D list (matrix[u][v]) with distances; -1 indicates no direct edge.
    Updates the matrix in-place with shortest distances, -1 if unreachable.
    """
    n = len(matrix)
    INF = 10**9

    # Replace -1 with INF, and 0 on the diagonal
    for i in range(n):
        for j in range(n):
            if matrix[i][j] == -1 and i != j:
                matrix[i][j] = INF
            if i == j:
                matrix[i][j] = 0

    for k in range(n):
        for i in range(n):
            for j in range(n):
                if matrix[i][k] + matrix[k][j] < matrix[i][j]:
                    matrix[i][j] = matrix[i][k] + matrix[k][j]

    # Convert INF back to -1
    for i in range(n):
        for j in range(n):
            if matrix[i][j] == INF:
                matrix[i][j] = -1
```

---

## Union-Find (Disjoint Set Union - DSU)

```python
class DSU:
    def __init__(self, n):
        """
        :param n: Number of elements (0 to n-1).
        """
        self.parent = [i for i in range(n)]
        self.rank = [0] * n
        self.size = [1] * n

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union_by_rank(self, x, y):
        """
        Union two sets by rank.
        :return: True if merged, False if already in the same set.
        """
        root_x = self.find(x)
        root_y = self.find(y)

        if root_x == root_y:
            return False

        if self.rank[root_x] < self.rank[root_y]:
            self.parent[root_x] = root_y
        elif self.rank[root_x] > self.rank[root_y]:
            self.parent[root_y] = root_x
        else:
            self.parent[root_y] = root_x
            self.rank[root_x] += 1

        return True

    def union_by_size(self, x, y):
        """
        Union two sets by size.
        :return: True if merged, False if already in the same set.
        """
        root_x = self.find(x)
        root_y = self.find(y)

        if root_x == root_y:
            return False

        if self.size[root_x] < self.size[root_y]:
            self.parent[root_x] = root_y
            self.size[root_y] += self.size[root_x]
        else:
            self.parent[root_y] = root_x
            self.size[root_x] += self.size[root_y]

        return True
```

---

## Minimum Spanning Tree (MST)

### 1. Prim’s Algorithm

```python
import heapq

def mst_prim(adjacency_dict, num_vertices):
    """
    Prim's Algorithm to find the cost of MST in a weighted undirected graph.
    :param adjacency_dict: {node: [(neighbor, weight), ...], ...}
    :param num_vertices: Total number of vertices.
    :return: The total cost of the MST.
    """
    visited = [False] * num_vertices
    min_heap = [(0, 0)]  # (cost, node) - start from node 0
    total_cost = 0

    edges_used = 0

    while min_heap and edges_used < num_vertices:
        cost, node = heapq.heappop(min_heap)
        if visited[node]:
            continue

        visited[node] = True
        total_cost += cost
        edges_used += 1

        for neighbor, weight in adjacency_dict[node]:
            if not visited[neighbor]:
                heapq.heappush(min_heap, (weight, neighbor))

    return total_cost
```

### 2. Kruskal’s Algorithm

```python
def mst_kruskal(num_vertices, edges):
    """
    Kruskal's Algorithm to find the MST cost in a weighted undirected graph.
    :param num_vertices: Number of vertices.
    :param edges: List of edges [u, v, weight].
    :return: The total cost of the MST.
    """
    # Sort edges by weight
    edges.sort(key=lambda x: x[2])

    dsu = DSU(num_vertices)
    mst_cost = 0
    edges_used = 0

    for u, v, weight in edges:
        if dsu.union_by_rank(u, v):
            mst_cost += weight
            edges_used += 1
            if edges_used == num_vertices - 1:
                break

    return mst_cost
```

---

## Kosaraju’s Algorithm

### Number of Strongly Connected Components (SCCs)

```python
def kosaraju_scc(num_vertices, adjacency_list):
    """
    Kosaraju's Algorithm to find the number of strongly connected components.
    :param num_vertices: Number of vertices.
    :param adjacency_list: adjacency_list[u] = list of neighbors of u.
    :return: The count of strongly connected components.
    """
    visited = [False] * num_vertices
    stack = []

    # 1. Fill vertices in stack by finish time
    def dfs_fill(vertex):
        visited[vertex] = True
        for neighbor in adjacency_list[vertex]:
            if not visited[neighbor]:
                dfs_fill(neighbor)
        stack.append(vertex)

    for vertex in range(num_vertices):
        if not visited[vertex]:
            dfs_fill(vertex)

    # 2. Create a reversed graph
    reversed_adj = [[] for _ in range(num_vertices)]
    for u in range(num_vertices):
        for v in adjacency_list[u]:
            reversed_adj[v].append(u)

    # 3. Pop from stack, do DFS on reversed graph
    visited = [False] * num_vertices
    scc_count = 0

    def dfs_reverse(vertex):
        visited[vertex] = True
        for neighbor in reversed_adj[vertex]:
            if not visited[neighbor]:
                dfs_reverse(neighbor)

    while stack:
        node = stack.pop()
        if not visited[node]:
            dfs_reverse(node)
            scc_count += 1

    return scc_count
```

---

# Summary

This Python Cheatsheet covers a variety of graph algorithms and patterns:

- **Traversal**: DFS (iterative & recursive), BFS
- **Connectivity**: Counting connected components
- **Cycle Detection**: Directed & Undirected Graphs
- **Topological Sort**: DFS method & Kahn’s Algorithm
- **Grid-based BFS/DFS**: Common patterns like “Number of Islands” and “Rotting Oranges”
- **Shortest Path**: BFS (unweighted), Dijkstra’s, Bellman-Ford, Floyd-Warshall, and Weighted DAG via Topological Sort
- **Disjoint Set (Union-Find)**: For advanced operations like Kruskal's MST
- **Minimum Spanning Tree**: Prim’s and Kruskal’s
- **Kosaraju’s Algorithm**: Strongly Connected Components

**Note**: The complexity and specific implementations can vary based on your use case and constraints. Understanding the fundamentals and customizing them to fit your needs is key.

**Happy Coding!**