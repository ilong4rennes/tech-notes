### 909. Snakes and Ladders
```python
class Solution:
	def snakesAndLadders(self, board: List[List[int]]) -> int:
		n = len(board)
		cells = [None] * (n ** 2 + 1)
		label = 1
		columns = list(range(0, n))
		for row in range(n - 1, -1, -1):
			for col in columns:
				cells[label] = (row, col)
				label += 1
			columns.reverse()
		
		dist = [-1] * (n ** 2 + 1)
		q = deque([1])
		dist[1] = 0
		while q:
			curr = q.popleft()
			for next in range(curr + 1, min(curr + 6, n ** 2) + 1):
				row, col = cells[next]
				destination = (board[row][col] if board[row][col] != -1 else next)
				if dist[destination] == -1:
					dist[destination] = dist[curr] + 1
					q.append(destination)
		return dist[n * n]
```

### 433. Minimum Genetic Mutation
```python
class Solution:
	def minMutation(self, startGene: str, endGene: str, bank: List[str]) -> int:
		queue = deque([(startGene, 0)])
		seen = {startGene}
		  
		while queue:
			node, steps = queue.popleft()
			if node == endGene:
				return steps
				
			for char in "ACGT":
				for i in range(len(node)):
					neighbor = node[:i] + char + node[i+1:]
					if neighbor not in seen and neighbor in bank:
						queue.append((neighbor, steps + 1))
						seen.add(neighbor)
				
		return -1
```