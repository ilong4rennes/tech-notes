### [17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

```python
class Solution: 
	def letterCombinations(self, digits: str) -> List[str]: 
		if not digits: return [] 
		phone_map = { '2': 'abc', 
					  '3': 'def', 
					  '4': 'ghi', 
					  '5': 'jkl', 
					  '6': 'mno', 
					  '7': 'pqrs', 
					  '8': 'tuv', 
					  '9': 'wxyz' } 
		combinations = [''] 
		
		for digit in digits: 
			new_combinations = [] 
			for combination in combinations: 
				for letter in phone_map[digit]: 
					new_combinations.append(combination + letter) 
			combinations = new_combinations 
		return combinations
```

### [46. Permutations](https://leetcode.com/problems/permutations/)

```python
class Solution: 
	def permute(self, nums: List[int]) -> List[List[int]]: 
		result = [] 
		self.dfs(nums, [], result) 
		return result 
	
	def dfs(self, nums, path, result): 
		if not nums: 
			result.append(path) 
			return 
		
		for i in range(len(nums)): 
			self.dfs(nums[:i] + nums[i+1:], path+[nums[i]], result)
```

### 39. Combination Sum

```python
class Solution:
	def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
		result = []
		candidates.sort()
		self.dfs(candidates, target, 0, [], result)
		return result
	
	def dfs(self, candidates, target, index, path, result):
		if target < 0: return
		if target == 0:
			result.append(path)
			return
		for i in range(index, len(candidates)):
			self.dfs(candidates, target - candidates[i], 
					 i, path + [candidates[i]], result)
```

### 77. Combinations

```python
class Solution:
	def combine(self, n: int, k: int) -> List[List[int]]:
		result = []
		self.dfs(range(1, n + 1), k, 0, [], result)
		return result
	  
	def dfs(self, nums, k, index, path, result):
		if k == 0:
			result.append(path)
			return
		for i in range(index, len(nums)):
			self.dfs(nums, k-1, i+1, path + [nums[i]], result)
```

