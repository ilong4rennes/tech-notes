### 120. Triangle
```python
class Solution:
	def minimumTotal(self, triangle: List[List[int]]) -> int:
		for row in range(1, len(triangle)):
			for col in range(row + 1):
				if col == 0:
					smallest_above = triangle[row - 1][col]
				elif col == row:
					smallest_above = triangle[row - 1][col - 1]
				else:
					smallest_above = min(triangle[row - 1][col - 1], 
										 triangle[row - 1][col])
				triangle[row][col] += smallest_above
		return min(triangle[-1])
```

### 64. Minimum Path Sum
```python
class Solution:
	def minPathSum(self, grid: List[List[int]]) -> int:
		rows, cols = len(grid), len(grid[0])
		for row in range(rows):
			for col in range(cols):
				if row == 0 and col == 0:
					continue
				elif row == 0:
					minPathSum = grid[row][col - 1]
				elif col == 0:
					minPathSum = grid[row - 1][col]
				else:
					minPathSum = min(grid[row - 1][col], grid[row][col - 1])
				grid[row][col] += minPathSum
		return grid[rows - 1][cols - 1]
```

### 72. Edit Distance
```python
class Solution:
	def minDistance(self, word1: str, word2: str) -> int:
		word1Len, word2Len = len(word1), len(word2)
		memo = [[0] * (word2Len + 1) for _ in range(word1Len + 1)]
		return self.helper(word1, word2, word1Len, word2Len, memo)
		  
	def helper(self, word1, word2, word1Id, word2Id, memo):
		if word1Id == 0: return word2Id
		if word2Id == 0: return word1Id
		if memo[word1Id][word2Id]: return memo[word1Id][word2Id]
		  
		if word1[word1Id - 1] == word2[word2Id - 1]:
			minEditDist = self.helper(word1, word2, word1Id - 1, word2Id - 1, memo)
		else:
			insert = self.helper(word1, word2, word1Id, word2Id - 1, memo)
			delete = self.helper(word1, word2, word1Id - 1, word2Id, memo)
			replace = self.helper(word1, word2, word1Id - 1, word2Id - 1, memo)
			minEditDist = min(insert, delete, replace) + 1
			memo[word1Id][word2Id] = minEditDist
		return minEditDist
```

### 1143. Longest Common Subsequence
```python
class Solution:
	def longestCommonSubsequence(self, text1: str, text2: str) -> int:
		t1Len, t2Len = len(text1), len(text2)
		memo = [[0] * (t2Len + 1) for _ in range(t1Len + 1)]
		for row in range(t1Len - 1, -1, -1):
			for col in range(t2Len - 1, -1, -1):
				if text1[row] == text2[col]:
					memo[row][col] = 1 + memo[row + 1][col + 1]
				else:
					memo[row][col] = max(memo[row + 1][col], memo[row][col + 1])
		return memo[0][0]
```
### 122. Best Time to Buy and Sell Stock II
```python
class Solution:
	def maxProfit(self, prices: List[int]) -> int:
		dp = [[0] * 2 for _ in range(len(prices))]
		dp[0][0] = -prices[0]
		dp[0][1] = 0
		for i in range(1, len(prices)):
			dp[i][0] = max(dp[i - 1][0],
						   dp[i - 1][1] - prices[i])
			dp[i][1] = max(dp[i - 1][1],
						   dp[i - 1][0] + prices[i])
		return dp[-1][1]
```
### 123. Best Time to Buy and Sell Stock III
```python
class Solution:
	def maxProfit(self, prices: List[int]) -> int:
		dp = [[0] * 5 for _ in range(len(prices))]
		dp[0][1] = -prices[0]
		dp[0][3] = -prices[0]
		
		for i in range(1, len(prices)):
			dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] - prices[i])
			dp[i][2] = max(dp[i - 1][2], dp[i - 1][1] + prices[i])
			dp[i][3] = max(dp[i - 1][3], dp[i - 1][2] - prices[i])
			dp[i][4] = max(dp[i - 1][4], dp[i - 1][3] + prices[i])
		return dp[-1][4]
```
### 188. Best Time to Buy and Sell Stock IV
```python
class Solution:
	def maxProfit(self, k: int, prices: List[int]) -> int:
		dp = [[0] * (2*k + 1) for _ in range(len(prices))]
		for i in range(k):
			dp[0][2*i + 1] = -prices[0]
		for i in range(1, len(prices)):
			for j in range(k):
				dp[i][2*j + 1] = max(dp[i - 1][2*j + 1], 
									 dp[i - 1][2 * j] - prices[i])
				dp[i][2*j + 2] = max(dp[i - 1][2*j + 2], 
									 dp[i - 1][2 * j + 1] + prices[i])
		return dp[-1][2*k]
```

### 221. Maximal Square
```python
class Solution:
	def maximalSquare(self, matrix: List[List[str]]) -> int:
		rows, cols = len(matrix), len(matrix[0])
		dp = [[0] * cols for _ in range(rows)]
		max_size = 0
		for row in range(rows):
			for col in range(cols):
				if matrix[row][col] == '1':
					if row == 0 and col == 0:
						dp[row][col] = 1
					else:
						dp[row][col] = min(dp[row-1][col], 
										   dp[row][col-1],
										   dp[row-1][col-1]) + 1
					max_size = max(max_size, dp[row][col])
		return max_size * max_size
```

### 63. Unique Path II
```python
class Solution:
	def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
		if obstacleGrid[0][0] == 1:
			return 0
		
		rows, cols = len(obstacleGrid), len(obstacleGrid[0])
		dp = [[0] * cols for _ in range(rows)]
		dp[0][0] = 1
		
		# Handle the first row
		for col in range(1, cols):
			if ((dp[0][col - 1] == 1) and (obstacleGrid[0][col] == 0)):
				dp[0][col] = 1
			else:
				dp[0][col] = 0
		  
		# Handle the first col
		for row in range(1, rows):
			if ((dp[row - 1][0] == 1) and (obstacleGrid[row][0] == 0)):
				dp[row][0] = 1
			else:
				dp[row][0] = 0
		  
		# Handle the rest
		for row in range(1, rows):
			for col in range(1, cols):
				if obstacleGrid[row][col] == 0:
					dp[row][col] = dp[row - 1][col] + dp[row][col - 1]
				else:
					dp[row][col] = 0
		
		return dp[rows - 1][cols - 1]
```

### 97. Interleaving String
```python
class Solution:
	def isInterleave(self, s1: str, s2: str, s3: str) -> bool:
		if len(s3) != len(s1) + len(s2): return False
		if s1 == '' and s2 == '' and s3 == '': return True
		  
		rows, cols = len(s1) + 1, len(s2) + 1
		dp = [[False] * (cols) for _ in range(rows)]
		dp[0][0] = True
		  
		# Handle first row
		for col in range(1, cols):
			dp[0][col] = ((dp[0][col - 1] == True) and (s2[col - 1] == s3[col - 1]))
		  
		# Handle first col
		for row in range(1, rows):
			dp[row][0] = ((dp[row - 1][0] == True) and (s1[row - 1] == s3[row - 1]))
		  
		# Handle the rest
		for row in range(1, rows):
			for col in range(1, cols):
				if (dp[row - 1][col] == True and s1[row - 1] == s3[row + col - 1]) 
				or (dp[row][col - 1] == True and s2[col - 1] == s3[row + col - 1]):
					dp[row][col] = True
				else: 
					dp[row][col] = False
		  
		return dp[rows - 1][cols - 1]
```
