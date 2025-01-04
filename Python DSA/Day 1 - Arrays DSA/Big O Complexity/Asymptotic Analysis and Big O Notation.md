# Asymptotic Analysis and Big O Notation

## Introduction

Asymptotic analysis helps us understand how algorithms perform as input sizes approach infinity. Big O notation specifically describes the upper bound or worst-case scenario of an algorithm's growth rate.

## Common Time Complexities with Examples

### 1. O(1) - Constant Time

Operations that execute in the same time regardless of input size.

```python
def constant_time(arr):
    """O(1) - Constant time to access first element"""
    if len(arr) > 0:
        return arr[0]
    return None

# Example of constant space
def swap_first_two(arr):
    """O(1) - Constant space and time"""
    if len(arr) >= 2:
        arr[0], arr[1] = arr[1], arr[0]
    return arr
```

### 2. O(log n) - Logarithmic Time

Typically seen in algorithms that divide the problem in half each time.

```python
def binary_search(arr, target):
    """O(log n) - Binary search implementation"""
    left, right = 0, len(arr) - 1

    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

### 3. O(n) - Linear Time

Time grows linearly with input size.

```python
def linear_search(arr, target):
    """O(n) - Linear search implementation"""
    for i, num in enumerate(arr):
        if num == target:
            return i
    return -1

def calculate_sum(arr):
    """O(n) - Sum all elements"""
    total = 0
    for num in arr:
        total += num
    return total
```

### 4. O(n log n) - Linearithmic Time

Common in efficient sorting algorithms.

```python
def merge_sort(arr):
    """O(n log n) - Merge sort implementation"""
    if len(arr) <= 1:
        return arr

    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])

    return merge(left, right)

def merge(left, right):
    """Helper function for merge sort"""
    result = []
    i = j = 0

    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1

    result.extend(left[i:])
    result.extend(right[j:])
    return result
```

### 5. O(n²) - Quadratic Time

Often seen in nested iterations.

```python
def bubble_sort(arr):
    """O(n²) - Bubble sort implementation"""
    n = len(arr)
    for i in range(n):
        for j in range(0, n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
    return arr

def find_pairs(arr):
    """O(n²) - Find all possible pairs"""
    pairs = []
    for i in range(len(arr)):
        for j in range(i + 1, len(arr)):
            pairs.append((arr[i], arr[j]))
    return pairs
```

### 6. O(2ⁿ) - Exponential Time

Often seen in recursive algorithms solving combinatorial problems.

```python
def fibonacci_recursive(n):
    """O(2ⁿ) - Recursive Fibonacci implementation"""
    if n <= 1:
        return n
    return fibonacci_recursive(n-1) + fibonacci_recursive(n-2)

def generate_subsets(arr):
    """O(2ⁿ) - Generate all possible subsets"""
    if not arr:
        return [[]]

    element = arr[0]
    subsets_without = generate_subsets(arr[1:])
    subsets_with = [[element] + subset for subset in subsets_without]

    return subsets_without + subsets_with
```

## Space Complexity Examples

```python
def space_constant(n):
    """O(1) space - Only uses a fixed amount of extra space"""
    total = 0
    for i in range(n):
        total += i
    return total

def space_linear(n):
    """O(n) space - Creates an array of size n"""
    return [i * 2 for i in range(n)]

def space_quadratic(n):
    """O(n²) space - Creates a matrix of size n×n"""
    return [[0 for _ in range(n)] for _ in range(n)]
```

## Analyzing Combined Complexities

```python
def combined_complexity(arr):
    """
    Time: O(n log n) - sorting
    Space: O(n) - creating new array
    """
    # O(n) space for new array
    doubled = [x * 2 for x in arr]

    # O(n log n) time for sorting
    return sorted(doubled)
```

## Best and Worst Case Analysis

```python
def quick_sort_partition(arr, low, high):
    """
    Quick Sort Partition
    Best Case: O(n log n) - Balanced partitions
    Worst Case: O(n²) - Already sorted array
    """
    if low < high:
        pivot = arr[high]
        i = low - 1

        for j in range(low, high):
            if arr[j] <= pivot:
                i += 1
                arr[i], arr[j] = arr[j], arr[i]

        arr[i + 1], arr[high] = arr[high], arr[i + 1]

        quick_sort_partition(arr, low, i)
        quick_sort_partition(arr, i + 2, high)

    return arr
```

Here's a detailed explanation of Big O, Big Omega, and Big Theta notations:

# Understanding Asymptotic Notations: Big O, Big Omega, and Big Theta

## 1. Big O (O) - Upper Bound

- Represents the worst-case scenario
- Describes the upper limit of growth rate
- Formally: f(n) = O(g(n)) means f(n) grows no faster than g(n)

```python
def find_max(arr):
    """
    O(n) - Linear time upper bound
    Best case could be O(1) if max is first element
    """
    if not arr:
        return None

    max_val = arr[0]
    for num in arr:  # Worst case: must check every element
        if num > max_val:
            max_val = num
    return max_val
```

## 2. Big Omega (Ω) - Lower Bound

- Represents the best-case scenario
- Describes the lower limit of growth rate
- Formally: f(n) = Ω(g(n)) means f(n) grows at least as fast as g(n)

```python
def bubble_sort(arr):
    """
    Ω(n) - Best case when array is already sorted
    O(n²) - Worst case upper bound
    """
    n = len(arr)
    for i in range(n):
        swapped = False
        for j in range(0, n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True
        # Best case: array is sorted, no swaps needed
        if not swapped:
            break
    return arr
```

## 3. Big Theta (Θ) - Tight Bound

- Represents both upper and lower bounds
- Used when upper and lower bounds are the same
- Formally: f(n) = Θ(g(n)) means f(n) grows exactly as fast as g(n)

```python
def sum_array(arr):
    """
    Θ(n) - Both best and worst case are linear
    Always needs to process every element exactly once
    """
    total = 0
    for num in arr:  # Always processes n elements
        total += num
    return total
```

## Comparative Example

```python
def search_examples(arr, target):
    """Different search implementations showing all three notations"""

    def linear_search():
        """
        O(n) - Worst case: element at end or not found
        Ω(1) - Best case: element at beginning
        No Θ notation as bounds aren't equal
        """
        for i, num in enumerate(arr):
            if num == target:
                return i
        return -1

    def binary_search():
        """
        O(log n) - Worst case: last comparison
        Ω(1) - Best case: middle element
        No Θ notation as bounds aren't equal
        """
        left, right = 0, len(arr) - 1
        while left <= right:
            mid = (left + right) // 2
            if arr[mid] == target:
                return mid
            elif arr[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        return -1

    def constant_comparison():
        """
        Θ(1) - Both best and worst case are constant
        O(1) and Ω(1) are the same
        """
        return arr[0] if arr and arr[0] == target else -1
```

## Practical Examples with Different Bounds

```python
class BoundExamples:
    def merge_sort(self, arr):
        """
        Θ(n log n) - Always divides and merges same way
        O(n log n) = Ω(n log n)
        """
        if len(arr) <= 1:
            return arr

        mid = len(arr) // 2
        left = self.merge_sort(arr[:mid])
        right = self.merge_sort(arr[mid:])

        return self.merge(left, right)

    def quick_sort(self, arr):
        """
        O(n²) - Worst case: Poor pivot selection
        Ω(n log n) - Best case: Good pivot selection
        No Θ notation as bounds differ
        """
        if len(arr) <= 1:
            return arr

        pivot = arr[len(arr) // 2]
        left = [x for x in arr if x < pivot]
        middle = [x for x in arr if x == pivot]
        right = [x for x in arr if x > pivot]

        return self.quick_sort(left) + middle + self.quick_sort(right)

    @staticmethod
    def merge(left, right):
        """Helper function for merge sort"""
        result = []
        i = j = 0

        while i < len(left) and j < len(right):
            if left[i] <= right[j]:
                result.append(left[i])
                i += 1
            else:
                result.append(right[j])
                j += 1

        result.extend(left[i:])
        result.extend(right[j:])
        return result
```

## Key Differences Summary:

1. Big O (O)

   - Upper bound
   - "Growth rate is at most..."
   - Most commonly used in practice
   - Useful for worst-case analysis

2. Big Omega (Ω)

   - Lower bound
   - "Growth rate is at least..."
   - Used for best-case analysis
   - Less commonly used in practice

3. Big Theta (Θ)
   - Tight bound (both upper and lower)
   - "Growth rate is exactly..."
   - Used when behavior is consistent
   - Most precise of the three notations
