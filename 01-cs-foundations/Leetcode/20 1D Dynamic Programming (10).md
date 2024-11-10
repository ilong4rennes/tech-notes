### 70. Climbing Stairs
```python
class Solution:
	def climbStairs(self, n: int) -> int:
		memo = {}
		memo[1] = 1
		memo[2] = 2
		return self.climb(n, memo)
	
	def climb(self, n, memo):
		if n in memo:
			return memo[n]
		else:
			memo[n] = self.climb(n-1, memo) + self.climb(n-2, memo)
		return memo[n]
```
### 322. Coin Change
```python
class Solution:
	def coinChange(self, coins: List[int], amount: int) -> int:
		numCoins = len(coins)
		minCoins = [amount + 1] * (amount + 1)
		minCoins[0] = 0
		
		for i in range(amount + 1):
			for coin in coins:
				if coin <= i:
					minCoins[i] = min(minCoins[i], minCoins[i - coin] + 1)
		
		if minCoins[amount] == amount + 1: return -1
		return minCoins[amount]
```
### 300. Longest Increasing Subsequence
```python
class Solution:
	def lengthOfLIS(self, nums: List[int]) -> int:
		dp = [1] * len(nums)
		for i in range(1, len(nums)):
			for j in range(i):
				if nums[i] > nums[j]:
					dp[i] = max(dp[i], dp[j] + 1)
		return max(dp)
```
### 139. Word Break
```python
class Solution:
	def wordBreak(self, s: str, wordDict: List[str]) -> bool:
		dp = [False] * len(s)
		for i in range(len(s)):
			for word in wordDict:
				# Handle out of bounds case
				if i < len(word) - 1:
					continue
			  
				if i == len(word) - 1 or dp[i - len(word)]:
					if s[i - len(word) + 1 : i + 1] == word:
						dp[i] = True
						break
		  
		return dp[-1]
```
### 39. Combination Sum I
```python
class Solution:
	def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
		result = []
		candidates.sort()
		self.dfs(candidates, target, 0, [], result)
		return result
	
	def dfs(self, nums, target, index, path, result):
		if target < 0:
			return
		if target == 0:
			result.append(path)
			return
		for i in range(index, len(nums)):
			self.dfs(nums, target - nums[i], i, path+[nums[i]], result)
```

### 40. Combination Sum II
```python
class Solution:
		def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
		result = []
		candidates.sort()
		self.dfs(candidates, target, 0, [], result)
		return result
	  
	def dfs(self, nums, target, index, path, result):
		if target < 0: return
		if target == 0:
			result.append(path)
			return
		for i in range(index, len(nums)):
			if i > index and nums[i] == nums[i-1]:
				continue
			self.dfs(nums, target - nums[i], i+1, path + [nums[i]], result)
```

### 216. Combination Sum III
```python
class Solution:
	def combinationSum3(self, k: int, n: int) -> List[List[int]]:
		result = []
		self.dfs(k, n, 1, [], result)
		return result
	
	def dfs(self, length, target, index, path, result):
		if len(path) == length and target == 0:
			result.append(path)
			return
		for i in range(index, 10):
			if i <= target:
				self.dfs(length, target - i, i+1, path + [i], result)
			else:
				return
```

### 377. Combination Sum IV
```python
class Solution:
	def combinationSum4(self, nums: List[int], target: int) -> int:
		memo = {}
		nums.sort()
		return self.helper(nums, target, memo)
	
	def helper(self, nums, target, memo):
		if target in memo:
			return memo[target]
		if target < 0:
			return 0
		if target == 0:
			return 1
			
		result = 0
		for num in nums:
			result += self.helper(nums, target - num, memo)
			
		memo[target] = result
		return result
```
### 198. House Robber
```python
class Solution:
	def rob(self, nums: List[int]) -> int:
		memo = {}
		return self.helper(nums, len(nums) - 1, memo)
	
	def helper(self, nums, index, memo):
		if index < 0: return 0
		if index in memo:
			return memo[index]
		result = max(self.helper(nums, index - 2, memo) + nums[index],
					 self.helper(nums, index - 1, memo))
		memo[index] = result
		return result
```
### 213.House Robber II
```python
class Solution:
	def rob(self, nums: List[int]) -> int:
		if len(nums) == 1:
			return nums[0]
		return max(self.rob_helper(nums, 0, len(nums) - 2),
				   self.rob_helper(nums, 1, len(nums) - 1))
	
	def rob_helper(self, nums, start, end):
		memo = {}
		return self.helper(nums, end, start, memo)
	
	def helper(self, nums, index, start, memo):
		if index < start:
			return 0
		if index in memo:
			return memo[index]
		result = max(self.helper(nums, index - 2, start, memo) + nums[index],
					 self.helper(nums, index - 1, start, memo))
		memo[index] = result
		return result
```
### 91. Decode Ways
```python
```
### 62. Unique Path
```python
```
### 63. Unique Path II
```python
```
### 55. Jump Game
```python
```
