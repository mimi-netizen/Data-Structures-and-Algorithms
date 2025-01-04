# Space Complexity Analysis of Tree Algorithms

## 1. Basic Tree Structure

First, let's define a basic tree node structure:

```python
class TreeNode:
    def __init__(self, val=0):
        self.val = val
        self.left = None
        self.right = None
```

## 2. Tree Traversals

### 2.1 Recursive Depth-First Search (DFS)

#### Inorder Traversal

```python
def inorder_traversal(root):
    # Space Complexity: O(h) where h is height of tree
    # Worst case: O(n) for skewed tree
    # Average case: O(log n) for balanced tree
    if not root:
        return []

    return (inorder_traversal(root.left) +
            [root.val] +
            inorder_traversal(root.right))
```

#### Iterative DFS using Stack

```python
def inorder_traversal_iterative(root):
    # Space Complexity: O(h) for stack
    # Worst case: O(n) for skewed tree
    result = []
    stack = []
    current = root

    while current or stack:
        while current:
            stack.append(current)
            current = current.left

        current = stack.pop()
        result.append(current.val)
        current = current.right

    return result
```

### 2.2 Breadth-First Search (BFS)

```python
from collections import deque

def level_order_traversal(root):
    # Space Complexity: O(w) where w is max width of tree
    # Worst case: O(n/2) ≈ O(n) for last level of complete tree
    if not root:
        return []

    result = []
    queue = deque([root])

    while queue:
        level = []
        level_size = len(queue)

        for _ in range(level_size):
            node = queue.popleft()
            level.append(node.val)

            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)

        result.append(level)

    return result
```

## 3. Tree Construction and Modification

### 3.1 Building Binary Search Tree (BST)

```python
def insert_into_bst(root, val):
    # Space Complexity: O(h) due to recursion stack
    # h is height of tree
    if not root:
        return TreeNode(val)

    if val < root.val:
        root.left = insert_into_bst(root.left, val)
    else:
        root.right = insert_into_bst(root.right, val)

    return root

def build_bst(values):
    # Space Complexity: O(n) for the tree storage
    # Plus O(h) for recursion stack
    root = None
    for val in values:
        root = insert_into_bst(root, val)
    return root
```

### 3.2 Building Tree from Arrays

```python
def build_tree_from_level_order(arr):
    # Space Complexity: O(n) for the complete tree
    if not arr:
        return None

    root = TreeNode(arr[0])
    queue = deque([root])
    i = 1

    while queue and i < len(arr):
        node = queue.popleft()

        if i < len(arr) and arr[i] is not None:
            node.left = TreeNode(arr[i])
            queue.append(node.left)
        i += 1

        if i < len(arr) and arr[i] is not None:
            node.right = TreeNode(arr[i])
            queue.append(node.right)
        i += 1

    return root
```

## 4. Advanced Tree Operations

### 4.1 Serialization and Deserialization

```python
class TreeSerializer:
    def serialize(self, root):
        # Space Complexity: O(n) for the string representation
        if not root:
            return "null"

        return (f"{root.val}," +
                self.serialize(root.left) + "," +
                self.serialize(root.right))

    def deserialize(self, data):
        # Space Complexity: O(n) for the tree
        # Plus O(n) for the values list
        def dfs():
            val = values.pop(0)
            if val == "null":
                return None

            node = TreeNode(int(val))
            node.left = dfs()
            node.right = dfs()
            return node

        values = data.split(",")
        return dfs()
```

### 4.2 Lowest Common Ancestor (LCA)

```python
def lowest_common_ancestor(root, p, q):
    # Space Complexity: O(h) due to recursion stack
    # h is height of tree
    if not root or root == p or root == q:
        return root

    left = lowest_common_ancestor(root.left, p, q)
    right = lowest_common_ancestor(root.right, p, q)

    if left and right:
        return root
    return left if left else right
```

## 5. Tree Path Problems

### 5.1 All Paths from Root to Leaf

```python
def all_paths(root):
    # Space Complexity: O(n*h)
    # n paths of average length h
    def dfs(node, path, paths):
        if not node:
            return

        path.append(node.val)

        if not node.left and not node.right:
            paths.append(path[:])
        else:
            dfs(node.left, path, paths)
            dfs(node.right, path, paths)

        path.pop()

    paths = []
    dfs(root, [], paths)
    return paths
```

### 5.2 Path Sum Checking

```python
def has_path_sum(root, target_sum):
    # Space Complexity: O(h) for recursion stack
    # where h is height of tree
    def dfs(node, remaining):
        if not node:
            return False

        remaining -= node.val

        if not node.left and not node.right:
            return remaining == 0

        return (dfs(node.left, remaining) or
                dfs(node.right, remaining))

    return dfs(root, target_sum)
```

## 6. Memory-Efficient Tree Operations

### 6.1 Morris Traversal (Constant Space)

```python
def morris_inorder(root):
    # Space Complexity: O(1)
    # Modifies tree temporarily but restores it
    current = root
    result = []

    while current:
        if not current.left:
            result.append(current.val)
            current = current.right
        else:
            # Find the inorder predecessor
            predecessor = current.left
            while predecessor.right and predecessor.right != current:
                predecessor = predecessor.right

            if not predecessor.right:
                predecessor.right = current
                current = current.left
            else:
                predecessor.right = None
                result.append(current.val)
                current = current.right

    return result
```
