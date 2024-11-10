### 104. Maximum Depth of Binary Tree
```python
# Definition for a binary tree node.
# class TreeNode:
# def __init__(self, val=0, left=None, right=None):
# self.val = val
# self.left = left
# self.right = right

class Solution:
	def maxDepth(self, root: Optional[TreeNode]) -> int:
		if not root: return 0
		return max(self.maxDepth(root.left), self.maxDepth(root.right)) + 1
```

### 100. Same Tree
```python
# Definition for a binary tree node.
# class TreeNode:
# def __init__(self, val=0, left=None, right=None):
# self.val = val
# self.left = left
# self.right = right

class Solution:
	def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
		stack = [(p, q)]
		while stack:
			a, b = stack.pop()
			if a is None and b is None:
				continue
			if a is None or b is None or a.val != b.val:
				return False
			stack.append((a.left, b.left))
			stack.append((a.right, b.right))
		return True
```

### 226. Invert Binary Tree
```python
# Definition for a binary tree node. 
# class TreeNode: 
# def __init__(self, val=0, left=None, right=None): 
# self.val = val 
# self.left = left 
# self.right = right 

class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root: return root
        stack = [root]
        while stack:
            node = stack.pop()
            node.left, node.right = node.right, node.left
            if node.left: stack.append(node.left)
            if node.right: stack.append(node.right)
        return root
```

### 101. Symmetric Tree
```python
# Definition for a binary tree node.
# class TreeNode:
# def __init__(self, val=0, left=None, right=None):
# self.val = val
# self.left = left
# self.right = right

class Solution:
	def isSymmetric(self, root: Optional[TreeNode]) -> bool:
		stack = [(root.left, root.right)]
		while stack:
			left, right = stack.pop()
			if not left and not right: continue
			if not left or not right or left.val != right.val: return False
			if left.left or right.right: stack.append((left.left, right.right))
			if left.right or right.left: stack.append((left.right, right.left))
		return True
```

### 129. Sum Root to Leaf Numbers
```python
# Definition for a binary tree node.
# class TreeNode:
# def __init__(self, val=0, left=None, right=None):
# self.val = val
# self.left = left
# self.right = right

class Solution:
	def sumNumbers(self, root: Optional[TreeNode]) -> int:
		result = 0
		stack = [(root, 0)]
		  
		while stack:
			root, curr_num = stack.pop()
			curr_num = curr_num * 10 + root.val
			if not root.left and not root.right:
				result += curr_num
			if root.left: stack.append((root.left, curr_num))
			if root.right: stack.append((root.right, curr_num))
		return result
```

### 129. Sum Root to Leaf Numbers
```python
# Definition for a binary tree node.
# class TreeNode:
# def __init__(self, val=0, left=None, right=None):
# self.val = val
# self.left = left
# self.right = right

class Solution:
	def sumNumbers(self, root: Optional[TreeNode]) -> int:
		result = 0
		stack = [(root, 0)]
		  
		while stack:
			root, curr_num = stack.pop()
			if root:
				curr_num = curr_num * 10 + root.val
				if not root.left and not root.right:
					result += curr_num
				else:
					stack.append((root.left, curr_num))
					stack.append((root.right, curr_num))
		return result
```
### 106. Construct Binary Tree from Inorder and Postorder Traversal
```python
# Definition for a binary tree node.
# class TreeNode:
# def __init__(self, val=0, left=None, right=None):
# self.val = val
# self.left = left
# self.right = right

class Solution:
	def buildTree(self, inorder: List[int], postorder: List[int]) -> Optional[TreeNode]:
		index_map = {}
		for idx, val in enumerate(inorder):
			index_map[val] = idx
		return self.helper(index_map, 0, len(inorder) - 1, postorder)
	  
	def helper(self, index_map, left, right, postorder):
		if left > right: return None
		val = postorder.pop()
		root = TreeNode(val)
		index = index_map[val]
		  
		root.right = self.helper(index_map, index + 1, right, postorder)
		root.left = self.helper(index_map, left, index - 1, postorder)
		return root
```
### 105. Construct Binary Tree from Inorder and Preorder Traversal
```python
# Definition for a binary tree node.
# class TreeNode:
# def __init__(self, val=0, left=None, right=None):
# self.val = val
# self.left = left
# self.right = right

class Solution:
	def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
		index_map = {}
		for idx, val in enumerate(inorder):
			index_map[val] = idx
		return self.helper(index_map, 0, len(inorder) - 1, preorder)
	
	def helper(self, index_map, left, right, preorder):
		if left > right: return None
		val = preorder[0]
		root = TreeNode(val)
		index = index_map[val]
		  
		left_preorder = preorder[1:]
		right_preorder = preorder[index - left + 1:]
		
		root.left = self.helper(index_map, left, index - 1, left_preorder)
		root.right = self.helper(index_map, index + 1, right, right_preorder)
		return root
```