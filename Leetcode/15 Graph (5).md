### 200. Number of Islands
```python
class Solution:
	def numIslands(self, grid: List[List[str]]) -> int:
		count = 0
		for row in range(len(grid)):
			for col in range(len(grid[0])):
				if grid[row][col] == '1':
					self.dfs(grid, row, col)
					count += 1
		return count
	
	def dfs(self, grid, row, col):
		grid[row][col] = 0
		for dr, dc in (1, 0), (0, -1), (-1, 0), (0, 1):
			r = row + dr
			c = col + dc
			if 0 <= r < len(grid) and 0 <= c < len(grid[0]) and grid[r][c]=='1':
				self.dfs(grid,r,c)
```
### 130. Surrounded Regions
```python
class Solution:
	def solve(self, board: List[List[str]]) -> None:
	"""
	Do not return anything, modify board in-place instead.
	"""
	if not board:
		return
	  
	rows, cols = len(board), len(board[0])
	  
	def dfs(board, row, col):
		if row < 0 or col < 0 
			or row >= rows or col >= cols 
			or board[row][col] != 'O':
				return
		board[row][col] = 'T' # Temporarily mark the 'O' to avoid revisiting
		for dr, dc in [(1, 0), (0, -1), (-1, 0), (0, 1)]:
			dfs(board, row + dr, col + dc)
	  
	# Step 1: Mark all 'O's connected to the borders
	for row in range(rows):
		if board[row][0] == 'O':
			dfs(board, row, 0)
		if board[row][cols - 1] == 'O':
			dfs(board, row, cols - 1)
	
	for col in range(cols):
		if board[0][col] == 'O':
			dfs(board, 0, col)
		if board[rows - 1][col] == 'O':
			dfs(board, rows - 1, col)
	
	# Step 2: Convert all remaining 'O's to 'X'
	for row in range(rows):
		for col in range(cols):
			if board[row][col] == 'O':
				board[row][col] = 'X'
		
	# Step 3: Convert temporary marker 'T' back to 'O'
	for row in range(rows):
		for col in range(cols):
			if board[row][col] == 'T':
				board[row][col] = 'O'
```
### 133. Clone Graph
```python
"""
# Definition for a Node.
class Node:
def __init__(self, val = 0, neighbors = None):
self.val = val
self.neighbors = neighbors if neighbors is not None else []
"""
  
from typing import Optional
class Solution:
	def __init__(self):
		self.visited = {}
	  
	def cloneGraph(self, node: Optional['Node']) -> Optional['Node']:
		if not node: return node
		if node in self.visited: return self.visited[node]
		clone_node = Node(node.val, [])
		self.visited[node] = clone_node
		if node.neighbors:
			for n in node.neighbors:
				clone_node.neighbors.append(self.cloneGraph(n))
		return clone_node
```
### Evaluate Division

### 207. Course Schedule
```python
class Solution:
	def dfs(self, node, adj, visit, inStack):
		if inStack[node]: return True
		if visit[node]: return False
		  
		visit[node] = True
		inStack[node] = True
		for neighbor in adj[node]:
			if self.dfs(neighbor, adj, visit, inStack):
				return True
		inStack[node] = False
		return False
	  
	def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
		adj = [[] for _ in range(numCourses)]
		for prereq in prerequisites:
			adj[prereq[1]].append(prereq[0])
		visit = [False] * numCourses
		inStack = [False] * numCourses
		for i in range(numCourses):
			if self.dfs(i, adj, visit, inStack):
				return False
		return True
```
### 210. Course Schedule II
```python
class Solution:
	def dfs(self, node, adj, visit, inStack, result):
		if inStack[node]:
			return True
		if visit[node]:
			return False
		  
		visit[node] = True
		inStack[node] = True
		for neighbor in adj[node]:
			if self.dfs(neighbor, adj, visit, inStack, result):
				return True
		inStack[node] = False
		result.append(node)
		return False
	  
	def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
		adj = [[] for _ in range(numCourses)]
		for prereq in prerequisites:
			adj[prereq[1]].append(prereq[0])
		visit = [False] * numCourses
		inStack = [False] * numCourses
		result = []
		
		for i in range(numCourses):
		if not visit[i]:
			if self.dfs(i, adj, visit, inStack, result):
				return []
		  
		return result[::-1]
```
### Pacific Atlantic Water Flow 

### Longest Consecutive Sequence 

### Alien Dictionary

### Graph Valid Tree 

### Number of Connected Components in an Undirected Graph 
