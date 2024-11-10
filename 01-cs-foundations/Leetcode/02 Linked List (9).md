### 141. Linked List Cycle
```python
# Definition for singly-linked list.
# class ListNode:
# def __init__(self, x):
# self.val = x
# self.next = None
  
class Solution:
	def hasCycle(self, head: Optional[ListNode]) -> bool:
		slow, fast = head, head
		while fast and fast.next:
			slow = slow.next
			fast = fast.next.next
			if fast == slow:
				return True
		return False
```
### 2. Add Two Numbers
```python
# Definition for singly-linked list.
# class ListNode:
# def __init__(self, val=0, next=None):
# self.val = val
# self.next = next

class Solution:
	def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
		start = result = ListNode(0)
		carry = 0
		while l1 or l2 or carry != 0:
			digit1 = l1.val if l1 else 0
			digit2 = l2.val if l2 else 0
			sum = digit1 + digit2 + carry
			digit = sum % 10
			carry = sum // 10
			result.next = ListNode(digit)
			result = result.next
			l1 = l1.next if l1 else None
			l2 = l2.next if l2 else None
		return start.next
```
### 21. Merge Two Sorted Lists
```python
# Definition for singly-linked list.
# class ListNode:
# def __init__(self, val=0, next=None):
# self.val = val
# self.next = next

class Solution:
	def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
		start = dummy = ListNode()
		while list1 and list2:
			if list1.val <= list2.val:
				start.next = list1
				list1 = list1.next
			else:
				start.next = list2
				list2 = list2.next
			start = start.next
		if list1: start.next = list1
		if list2: start.next = list2
		return dummy.next
```
### 206. Reverse Linked List
```python
# Definition for singly-linked list.
# class ListNode:
# def __init__(self, val=0, next=None):
# self.val = val
# self.next = next

class Solution:
	def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
		prev, curr, next = None, head, None
		while curr:
			next = curr.next
			curr.next = prev
			prev = curr
			curr = next
		return prev
```

### 92. Reverse Linked List II
```python
# Definition for singly-linked list.
# class ListNode:
# def __init__(self, val=0, next=None):
# self.val = val
# self.next = next

class Solution:
	def reverseBetween(self, head: Optional[ListNode], left: int, right: int) -> Optional[ListNode]:
		if not head: return head
		start = result = ListNode()
		result.next = head
		  
		for i in range(left - 1):
			result = result.next
		  
		prev, curr, next = None, result.next, None
		tail = curr # This will be the tail of the reversed segment
		  
		for i in range(right - left + 1):
			next = curr.next
			curr.next = prev
			prev = curr
			curr = next
		  
		result.next = prev
		tail.next = curr
		return start.next
```

### 237. Delete Node in a Linked List
```python
class Solution:
	def deleteNode(self, node):
	"""
	:type node: ListNode
	:rtype: void Do not return anything, modify node in-place instead.
	"""
		node.val = node.next.val
		node.next = node.next.next
```

### 382. Linked List Random Node
```python
class Solution:
	def __init__(self, head: Optional[ListNode]):
		self.nodes = []
		while head:
			self.nodes.append(head.val)
			head = head.next
	  
	def getRandom(self) -> int:
		i = random.randint(0, len(self.nodes) - 1)
		return self.nodes[i]
	  
	# Your Solution object will be instantiated and called as such:
	# obj = Solution(head)
	# param_1 = obj.getRandom()
```

### 148. Sort List
```python
class Solution:
	def sortList(self, head: Optional[ListNode]) -> Optional[ListNode]:
		if not head or not head.next: return head
		  
		left = head
		right = self.getMid(head)
		temp = right.next
		right.next = None
		right = temp
		  
		left = self.sortList(left)
		right = self.sortList(right)
		return self.merge(left, right)
	
	def getMid(self, head):
		slow, fast = head, head.next
		while fast and fast.next:
			slow = slow.next
			fast = fast.next.next
		return slow
	
	def merge(self, l1, l2):
		head = start = ListNode()
		while l1 and l2:
			if l1.val < l2.val:
				head.next = l1
				l1 = l1.next
			else:
				head.next = l2
				l2 = l2.next
			head = head.next
		if l1: head.next = l1
		if l2: head.next = l2
		return start.next
```

### 143. Reorder List
```python
# Definition for singly-linked list.
# class ListNode:
# def __init__(self, val=0, next=None):
# self.val = val
# self.next = next

class Solution:
	def reorderList(self, head: Optional[ListNode]) -> None:
	"""
	Do not return anything, modify head in-place instead.
	"""
		if not head or not head.next: return
		id = 1
		nodeDict = {}
		start = ListNode(0, head)
		while head:
			nodeDict[id] = head
			id += 1
			head = head.next
		
		head = start.next	
		for i in range(1, (id // 2 + 1)):
			start.next = nodeDict[i]
			start = start.next
			if i != (id + 1) // 2:
				start.next = nodeDict[id - i]
				start = start.next
		start.next = None
```