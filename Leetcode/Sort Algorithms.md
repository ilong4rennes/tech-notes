![[Pasted image 20240721021351.png]]
### Bubble Sort
Bubble Sort repeatedly steps through the list, compares adjacent elements, and swaps them if they are in the wrong order.

```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        for j in range(0, n-i-1):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]

# Example usage
arr = [64, 34, 25, 12, 22, 11, 90]
bubble_sort(arr)
print("Bubble Sort:", arr)
```

- **Best Case**: O(n) - When the list is already sorted.
- **Worst Case**: O(n^2) - When the list is sorted in reverse order.
- **Average Case**: O(n^2)

### Selection Sort
Selection Sort repeatedly finds the minimum element from the unsorted part and moves it to the beginning.

```python
def selection_sort(arr):
    n = len(arr)
    for i in range(n):
        min_idx = i
        for j in range(i+1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j
        arr[i], arr[min_idx] = arr[min_idx], arr[i]

# Example usage
arr = [64, 25, 12, 22, 11]
selection_sort(arr)
print("Selection Sort:", arr)
```

- **Best Case**: O(n^2)
- **Worst Case**: O(n^2)
- **Average Case**: O(n^2)

### Insertion Sort
Insertion Sort builds the sorted array one item at a time by repeatedly taking the next item and inserting it into its correct position.

```python
def insertion_sort(arr):
    n = len(arr)
    for i in range(1, n):
        key = arr[i]
        j = i-1
        while j >= 0 and key < arr[j]:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key

# Example usage
arr = [12, 11, 13, 5, 6]
insertion_sort(arr)
print("Insertion Sort:", arr)
```

- **Best Case**: O(n) - When the list is already sorted.
- **Worst Case**: O(n^2) - When the list is sorted in reverse order.
- **Average Case**: O(n^2)

### Merge Sort
Merge Sort is a divide-and-conquer algorithm that divides the array into halves, sorts them, and then merges them back together.

```python
def merge_sort(arr):
    if len(arr) > 1:
        mid = len(arr) // 2
        L = arr[:mid]
        R = arr[mid:]

        merge_sort(L)
        merge_sort(R)

        i = j = k = 0
        while i < len(L) and j < len(R):
            if L[i] < R[j]:
                arr[k] = L[i]
                i += 1
            else:
                arr[k] = R[j]
                j += 1
            k += 1

        while i < len(L):
            arr[k] = L[i]
            i += 1
            k += 1

        while j < len(R):
            arr[k] = R[j]
            j += 1
            k += 1

# Example usage
arr = [12, 11, 13, 5, 6, 7]
merge_sort(arr)
print("Merge Sort:", arr)
```

- **Best Case**: O(n log n)
- **Worst Case**: O(n log n)
- **Average Case**: O(n log n)

### Quick Sort
Quick Sort is a divide-and-conquer algorithm that picks a pivot element, partitions the array around the pivot, and then sorts the subarrays.

```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    else:
        pivot = arr[len(arr) // 2]
        left = [x for x in arr if x < pivot]
        middle = [x for x in arr if x == pivot]
        right = [x for x in arr if x > pivot]
        return quick_sort(left) + middle + quick_sort(right)

# Example usage
arr = [10, 7, 8, 9, 1, 5]
sorted_arr = quick_sort(arr)
print("Quick Sort:", sorted_arr)
```

- **Best Case**: O(n log n)
- **Worst Case**: O(n^2) - When the pivot selection results in the most unbalanced partitions (e.g., already sorted array).
- **Average Case**: O(n log n)
