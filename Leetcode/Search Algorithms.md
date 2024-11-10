![[Pasted image 20240721021351.png]]

## Linear Search
```python
def linear_search(arr, target):
    for i in range(len(arr)):
        if arr[i] == target:
            return i  # Return the index of the target element
    return -1  # Return -1 if the target element is not found

# Example usage
arr = [10, 23, 45, 70, 11, 15]
target = 70
result = linear_search(arr, target)
print(f"Linear Search: Element {target} is at index {result}")
```

## Binary Search
```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid  # Return the index of the target element
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
            
    return -1  # Return -1 if the target element is not found

# Example usage
arr = [11, 15, 23, 45, 70]
target = 70
result = binary_search(arr, target)
print(f"Binary Search: Element {target} is at index {result}")
```

