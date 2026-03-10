## Reverse Linked List

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
  
class Solution:
	def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
		prev, curr, next = None, head, None
		while curr:
			temp = curr.next
			curr.next = prev
			prev = curr
			curr = temp
		return prev
```

## Merge Two Sorted Linked Lists

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
  
class Solution:
	def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
		head1, head2 = list1, list2
		dummy = head = ListNode()
		while head1 and head2:
			if head1.val <= head2.val:
				head.next = head1
				head1 = head1.next
			else:
				head.next = head2
				head2 = head2.next
			head = head.next
		if head1: head.next = head1
		if head2: head.next = head2
		return dummy.next
```

## Linked List Cycle Detection

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
  
class Solution:
	def hasCycle(self, head: Optional[ListNode]) -> bool:
		slow, fast = head, head
		while fast and fast.next:
			fast = fast.next.next
			slow = slow.next
			if fast == slow: return True
		return False
```