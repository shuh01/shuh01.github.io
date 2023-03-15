---
layout: single
title: Binary Tree - Root to leaf DFS/BFS
categories: Algorithms
use_math: true
toc: true
---

# DFS - Recursive

```python
def binaryTreePaths(self, root):
        result = []
        def dfs(root, path):
            nonlocal result
            if not root:
                return []
            path += [root.val]           
            if not root.left and not root.right:
                result.append(path.copy())
            dfs(root.left, path)
            dfs(root.right, path)
            path.pop()
            return result    
        return dfs(root, [])
```

# DFS - Iterative

```python
def binaryTreePaths(self, root):
        if not root:
            return []
        result, stack = [], [[root, ""]]
        while stack:
            node, string = stack.pop()
            if not node.left and not node.right:
                result.append([string+ str(node.val)].copy())
            if node.right:
                stack.append([node.right, string+str(node.val) + "->"])
            if node.left:
                stack.append([node.left, string+str(node.val) + "->"])
        return result
```

# BFS

```python
def binaryTreePaths(self, root):
        if not root:
            return []
        result, queue = [], deque([[root, []]])
        while queue:
            node, string = queue.popleft()
            if not node.left and not node.right:
                result.append([string+ [node.val]])
            if node.left:
                queue.append([node.left, string+ [node.val]])
            if node.right:
                queue.append([node.right, string+ [node.val]])
        return result
```
