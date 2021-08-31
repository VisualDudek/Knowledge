# Binary Trees

## Node Depths

* my mistake: recursive solution, calling rec. func on root and imediately increasing depth

```text
# pseudo code

recursion call: depth + call(left, depth + 1) + call(right, depth + 1)
```

```python
# my mistake
def nodeDepths(root):
	return helper(root, 0)

def helper(node, depth):
	depth += 1   # My mistake
	if node is None:
		return 0
	return depth + helper(node.left, depth) + helper(node.right, depth)


# This is the class of the input binary tree.
class BinaryTree:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None
```

