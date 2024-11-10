### 74. Search a 2D Matrix
```python
class Solution:
	def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
		rowIndex = self.findRowIndex(matrix, target)
		rowList = matrix[rowIndex]
		r_left, r_right = 0, len(rowList) - 1
		while r_left <= r_right:
			mid = (r_left + r_right) // 2
			if rowList[mid] == target:
				return True
			elif rowList[mid] < target:
				r_left = mid + 1
			else:
				r_right = mid - 1
		return False
	
	def findRowIndex(self, matrix, target):
		left, right = 0, len(matrix) - 1
		while left <= right:
			mid = (left + right) // 2
			if matrix[mid][0] == target:
				return mid
			elif matrix[mid][0] < target:
				left = mid + 1
			else:
				right = mid - 1
		# return left
		return left if left < len(matrix) and matrix[left][0] <= target else left - 1
```

### 162. Find Peak Element
```python
class Solution:
	def findPeakElement(self, nums: List[int]) -> int:
		left, right = 0, len(nums) - 1
		while left < right:
			mid = (left + right) // 2
			if nums[mid] > nums[mid + 1]:
				right = mid
			else:
				left = mid + 1
		return left
```

### 33. Search in Rotated Sorted Array
```python
class Solution:
	def search(self, nums: List[int], target: int) -> int:
		left, right = 0, len(nums) - 1
		while left <= right:
			mid = (left + right) // 2
			if nums[mid] > nums[-1]:
				left = mid + 1
			else:
				right = mid - 1
		pivotIndex = left
		return self.helper(pivotIndex, nums, target)
	  
	def helper(self, pivotIndex, nums, target):
		n = len(nums)
		left, right = 0, len(nums) - 1
		while left <= right:
			mid = (left + right) // 2
			if (nums[(mid + pivotIndex) % n] == target):
				return (mid + pivotIndex) % n
			elif (nums[(mid + pivotIndex) % n] > target):
				right = mid - 1
			else:
				left = mid + 1
		return -1
```

