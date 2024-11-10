### 125. Valid Palindrome
```python
class Solution:
	def isPalindrome(self, s: str) -> bool:
		i, j = 0, len(s) - 1
		while i < j:
			while i < j and not s[i].isalnum():
				i += 1
			while i < j and not s[j].isalnum():
				j -= 1
			  
			if s[i].lower() != s[j].lower():
			return False
			  
			i += 1
			j -= 1
		  
		return True
```
### 11. Container With Most Water
```python
class Solution:
	def maxArea(self, height: List[int]) -> int:
		left, right = 0, len(height) - 1
		champArea = 0
		currArea = min(height[right], height[left]) * (right - left)
		if currArea > champArea:
			champArea = currArea
		while left < right:
			if height[left] < height[right]: left += 1
			else: right -= 1
			currArea = min(height[right], height[left]) * (right - left)
			if currArea > champArea:
				champArea = currArea
		return champArea

```
### 15. 3Sum
```python
class Solution:
	def threeSum(self, nums: List[int]) -> List[List[int]]:
		nums.sort()
		result = []
		triplet = []
		for i in range(len(nums)):
			if i >= 1 and nums[i] == nums[i - 1]: continue
			left = i + 1
			right = len(nums) - 1
			while left < right:
				sum = nums[i] + nums[left] + nums[right]
				if sum == 0:
					triplet = [nums[i], nums[left], nums[right]]
					result.append(triplet)
					left += 1
					right -= 1
					while left < right and nums[left] == nums[left - 1]: 
						left += 1
					while left < right and nums[right] == nums[right + 1]: 
						right -= 1
				elif sum < 0:
					left += 1
				else:
					right -= 1
		return result
```

### 5. Longest Palindrome Substring
```python
class Solution:
	def longestPalindrome(self, s: str) -> str:
	if len(s) <= 1: return s
	  
	def expand_from_center(left, right):
		while left >= 0 and right < len(s) and s[left] == s[right]:
			left -= 1
			right += 1
		return s[left + 1:right]
	
	maxStr = s[0]
	  
	for i in range(len(s) - 1):
		odd = expand_from_center(i, i)
		even = expand_from_center(i, i+1)
		  
		if len(odd) > len(maxStr): maxStr = odd
		if len(even) > len(maxStr): maxStr = even
	return maxStr
```

### 167. Two Sum II - Input Array Is Sorted
```python
class Solution:
	def twoSum(self, numbers: List[int], target: int) -> List[int]:
		left = 0
		right = len(numbers) - 1
		while left < right:
			tempSum = numbers[left] + numbers[right]
			if tempSum < target: left += 1
			elif tempSum > target: right -= 1
			else:
				return [left + 1, right + 1]
```

### 170. Two Sum III - Data structure design
```python
class TwoSum:
	def __init__(self):
		self.numbers = []
	
	def add(self, number: int) -> None:
		self.numbers.append(number)
	
	def find(self, value: int) -> bool:
		self.numbers.sort()
		left, right = 0, len(self.numbers) - 1
		while left < right:
			tempSum = self.numbers[left] + self.numbers[right]
			if tempSum < value: left += 1
			elif tempSum > value: right -= 1
			else: return True
		return False
```

### 88. Merge Sorted Array
```python
class Solution:
	def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
	"""
	Do not return anything, modify nums1 in-place instead.
	"""
		result = []
		left, right = 0, 0
		while left < m and right < n:
			num1 = nums1[left]
			num2 = nums2[right]
			if num1 < num2:
				result.append(num1)
				left += 1
			else:
				result.append(num2)
				right += 1
		if left < m: result.extend(nums1[left:m])
		if right < n: result.extend(nums2[right:n])
		for i in range(len(result)):
			nums1[i] = result[i]
```