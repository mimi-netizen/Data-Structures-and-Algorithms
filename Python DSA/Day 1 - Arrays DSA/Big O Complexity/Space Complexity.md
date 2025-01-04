# Deep Dive into Space Complexity

## 1. Introduction to Space Complexity

Space complexity measures the total amount of memory space an algorithm uses relative to the input size. It includes both:

- Auxiliary Space: Extra space used by the algorithm
- Input Space: Space used by the input itself

## 2. Common Space Complexities with Examples

### 2.1 O(1) - Constant Space

These algorithms use the same amount of extra space regardless of input size.

```python
def find_sum(numbers):
    total = 0  # Single variable - constant space
    for num in numbers:
        total += num
    return total

def find_max_min(numbers):
    # Only two variables regardless of input size
    current_max = current_min = numbers[0]
    for num in numbers:
        current_max = max(current_max, num)
        current_min = min(current_min, num)
    return current_max, current_min
```

### 2.2 O(n) - Linear Space

Space usage grows linearly with input size.

```python
def create_double_array(numbers):
    # Creates new array of same size as input
    doubled = []  # O(n) space
    for num in numbers:
        doubled.append(num * 2)
    return doubled

def count_frequencies(string):
    # Hash map storing character counts
    freq = {}  # O(n) space in worst case
    for char in string:
        freq[char] = freq.get(char, 0) + 1
    return freq
```

### 2.3 O(n²) - Quadratic Space

Space usage grows with square of input size.

```python
def create_multiplication_table(n):
    # Creates n x n matrix
    table = []  # O(n²) space
    for i in range(n):
        row = []
        for j in range(n):
            row.append(i * j)
        table.append(row)
    return table

def find_all_pairs(array):
    # Stores all possible pairs
    pairs = []  # O(n²) space
    for i in range(len(array)):
        for j in range(len(array)):
            pairs.append((array[i], array[j]))
    return pairs
```

### 2.4 O(log n) - Logarithmic Space

Often seen in recursive algorithms that divide the problem.

```python
def binary_search_recursive(array, target, left, right):
    # Space used by recursive call stack is O(log n)
    if left > right:
        return -1

    mid = (left + right) // 2
    if array[mid] == target:
        return mid
    elif array[mid] < target:
        return binary_search_recursive(array, target, mid + 1, right)
    else:
        return binary_search_recursive(array, target, left, mid - 1)
```

## 3. Space-Time Tradeoffs

### 3.1 Trading Space for Time

```python
# Time-efficient but space-heavy (O(n) space)
def find_number_occurs_twice(numbers):
    seen = set()  # O(n) space
    for num in numbers:  # O(n) time
        if num in seen:
            return num
        seen.add(num)
    return None

# Space-efficient but time-heavy (O(1) space)
def find_number_occurs_twice_space_efficient(numbers):
    # O(1) space but O(n²) time
    for i in range(len(numbers)):
        for j in range(i + 1, len(numbers)):
            if numbers[i] == numbers[j]:
                return numbers[i]
    return None
```

### 3.2 Memoization Example

```python
def fibonacci_with_memoization(n, memo=None):
    # Trading space for time
    # O(n) space but O(n) time
    if memo is None:
        memo = {}

    if n in memo:
        return memo[n]

    if n <= 1:
        return n

    memo[n] = fibonacci_with_memoization(n-1, memo) + fibonacci_with_memoization(n-2, memo)
    return memo[n]
```

## 4. In-Place Algorithms

Algorithms that modify input directly to save space.

```python
def reverse_array_in_place(array):
    # O(1) extra space
    left, right = 0, len(array) - 1
    while left < right:
        array[left], array[right] = array[right], array[left]
        left += 1
        right -= 1
    return array

def sort_array_in_place(array):
    # Bubble sort: O(1) extra space
    n = len(array)
    for i in range(n):
        for j in range(0, n-i-1):
            if array[j] > array[j+1]:
                array[j], array[j+1] = array[j+1], array[j]
    return array
```

## 5. Recursive Space Complexity

### 5.1 Linear Recursive Space

```python
def factorial(n):
    # O(n) space due to recursive call stack
    if n <= 1:
        return 1
    return n * factorial(n - 1)

def print_array_elements(array, index=0):
    # O(n) space due to recursive call stack
    if index >= len(array):
        return
    print(array[index])
    print_array_elements(array, index + 1)
```

### 5.2 Tree Recursion Space

```python
def calculate_paths(m, n, memo=None):
    # Space complexity: O(m*n) due to memoization
    if memo is None:
        memo = {}

    if (m, n) in memo:
        return memo[(m, n)]

    if m == 1 or n == 1:
        return 1

    memo[(m, n)] = calculate_paths(m-1, n, memo) + calculate_paths(m, n-1, memo)
    return memo[(m, n)]
```
