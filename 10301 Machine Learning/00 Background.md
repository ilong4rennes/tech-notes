# Binary search tree (BST)
- Each node:
	- has a value, v  
	- up to 2 children, a left descendant and a right descendant  
	- all its left descendants have values less than v and its right descendants have values greater than v
- Search time: $O(\log(n))$, assuming n nodes in the tree
- Iterative search 
```
def contains_iterative(node, key): 
	cur = node 
	while true: 
		if key < cur.value & cur.left != null: 
			cur = cur.left 
		else if cur.value < key & cur.right != null: 
			cur = cur.right 
		else: break 
	return key == cur.value
```

- Recursive search
```
def contains_recursive(node, key): 
	if key < node.value & node.left != null: 
		return contains_recursive(node.left, key) 
	else if node.value < key & node.right != null: 
		return contains_recursive(node.right, key) 
	else: 
		return key == node.value
```

# Linear Algebra

![[Screen Shot 2024-02-18 at 21.35.02.png]]

# Geometry

![[Screen Shot 2024-02-18 at 21.37.28.png]]

# Matrix Calculus

![[Screen Shot 2024-03-14 at 02.00.36.png]]
![[Screen Shot 2024-03-14 at 02.00.48.png]]
![[Screen Shot 2024-03-14 at 02.01.02.png]]
# Sample Variance
![[Screen Shot 2024-04-25 at 00.26.14.png]]
# Sample Covariance Matrix
![[Screen Shot 2024-04-25 at 00.56.17.png]]
# Eigenvectors & Eigenvalues

![[Screen Shot 2024-04-25 at 04.09.51.png]]
- Fact #1: The eigenvectors of a symmetric matrix are orthogonal to each other.
- Fact #2: The covariance matrix $\Sigma$ is symmetric.