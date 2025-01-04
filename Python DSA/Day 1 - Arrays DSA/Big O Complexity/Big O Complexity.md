# Understanding Big O Complexity: A Comprehensive Guide

## 1. Introduction to Big O Notation

Big O notation is a mathematical notation that describes the limiting behavior of a function when the argument tends towards a particular value or infinity. In computer science, it's used to classify algorithms according to how their run time or space requirements grow as the input size grows.

### Key Concepts:

- Measures the worst-case scenario
- Describes the upper bound of growth rate
- Focuses on the dominant term
- Ignores coefficients and lower-order terms

## 2. Common Time Complexities

### 2.1 O(1) - Constant Time

Operations that take the same amount of time regardless of input size.

```python
def get_first_element(array):
    return array[0]  # O(1) - always returns first element

def check_if_even(number):
    return number % 2 == 0  # O(1) - simple arithmetic operation
```

### 2.2 O(log n) - Logarithmic Time

Algorithms that divide the problem in half each time.

```python
def binary_search(array, target):
    left = 0
    right = len(array) - 1

    while left <= right:
        mid = (left + right) // 2
        if array[mid] == target:
            return mid
        elif array[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

### 2.3 O(n) - Linear Time

Runtime grows linearly with input size.

```python
def find_max(array):
    max_val = array[0]
    for num in array:  # O(n) - visits each element once
        if num > max_val:
            max_val = num
    return max_val
```

### 2.4 O(n log n) - Linearithmic Time

Common in efficient sorting algorithms.

```python
def merge_sort(array):
    if len(array) <= 1:
        return array

    mid = len(array) // 2
    left = merge_sort(array[:mid])
    right = merge_sort(array[mid:])

    return merge(left, right)

def merge(left, right):
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

### 2.5 O(n²) - Quadratic Time

Nested iterations over the input.

```python
def bubble_sort(array):
    n = len(array)
    for i in range(n):  # O(n²) - nested loops
        for j in range(0, n - i - 1):
            if array[j] > array[j + 1]:
                array[j], array[j + 1] = array[j + 1], array[j]
    return array
```

### 2.6 O(2ⁿ) - Exponential Time

Growth doubles with each addition to input.

```python
def fibonacci_recursive(n):
    if n <= 1:
        return n
    return fibonacci_recursive(n-1) + fibonacci_recursive(n-2)
```

### 2.7 O(n!) - Factorial Time

Growth is factorial of input size.

```python
def generate_permutations(array):
    if len(array) <= 1:
        return [array]

    perms = []
    for i in range(len(array)):
        current = array[i]
        remaining = array[:i] + array[i+1:]

        for p in generate_permutations(remaining):
            perms.append([current] + p)

    return perms
```

## 3. Space Complexity

Space complexity measures the memory usage of an algorithm.

```python
# O(n) space complexity
def create_array(n):
    return [0] * n

# O(1) space complexity
def sum_array(array):
    total = 0
    for num in array:
        total += num
    return total
```

## 4. Real-world Algorithm Analysis

### Example: Finding Duplicates

```python
# O(n) time, O(n) space
def find_duplicates_hash(array):
    seen = set()
    duplicates = set()
    for num in array:
        if num in seen:
            duplicates.add(num)
        seen.add(num)
    return list(duplicates)

# O(n²) time, O(1) space
def find_duplicates_nested(array):
    duplicates = []
    for i in range(len(array)):
        for j in range(i + 1, len(array)):
            if array[i] == array[j] and array[i] not in duplicates:
                duplicates.append(array[i])
    return duplicates
```

## 5. Best, Average, and Worst Cases

### Quick Sort Example:

```python
def quicksort(array):
    if len(array) <= 1:
        return array

    pivot = array[len(array) // 2]
    left = [x for x in array if x < pivot]
    middle = [x for x in array if x == pivot]
    right = [x for x in array if x > pivot]

    return quicksort(left) + middle + quicksort(right)

# Best case: O(n log n)
# Average case: O(n log n)
# Worst case: O(n²)
```

## 6. Common Data Structure Operations

### List/Array Operations:

```python
# Access by index - O(1)
array[5]

# Search - O(n)
value in array

# Insertion/Deletion at end - O(1)
array.append(value)
array.pop()

# Insertion/Deletion at beginning - O(n)
array.insert(0, value)
array.pop(0)
```

### Dictionary/Hash Map Operations:

```python
# Access/Insert/Delete - O(1) average case
dict[key] = value
del dict[key]

# Search - O(1) average case
key in dict
```

## 7. Tips for Writing Efficient Code

1. Choose the right data structure:

```python
# Bad: Using list for frequent lookups
def contains_value(array, value):
    return value in array  # O(n)

# Good: Using set for frequent lookups
def contains_value_set(data_set, value):
    return value in data_set  # O(1)
```

2. Avoid nested loops when possible:

```python
# Bad: O(n²)
def find_sum_pairs(array, target):
    pairs = []
    for i in range(len(array)):
        for j in range(i + 1, len(array)):
            if array[i] + array[j] == target:
                pairs.append((array[i], array[j]))
    return pairs

# Good: O(n)
def find_sum_pairs_optimized(array, target):
    pairs = []
    seen = set()
    for num in array:
        complement = target - num
        if complement in seen:
            pairs.append((complement, num))
        seen.add(num)
    return pairs
```
