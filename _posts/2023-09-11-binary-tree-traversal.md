---
layout: post
title: Binary Tree different traversal ways
date: 2023-09-11 15:09:00
description: Binary Tree preorder, inorder, postorder and levelorder.
tags: code
categories: algo
giscus_comments: true
related_posts: false
related_publications: false
featured: true
toc:
  sidebar: right
---

# Definition

```go
type TreeNode struct {
    Val   int
    Left  *TreeNode
    Right *TreeNode
}
```

# Preorder

Access order: `Root -> Left -> Right`.

```go
// Recursion
func preorderTraversal(root *TreeNode) []int {
    // Time complexity: O(n)
    // Spatial complexity: O(n)
    res := []int{}
    var preOrder func(*TreeNode)
    preOrder = func(root *TreeNode) {
        if root == nil {
            return
        }
        res = append(res, root.Val)  // root
        preOrder(root.Left)          // left
        preOrder(root.Right)         // right
    }
    preOrder(root)
    return res
}

// Iterate
func preorderTraversal(root *TreeNode) []int {
    // Time complexity: O(n)
    // Spatial complexity: O(n)
    if root == nil {
        return nil
    }

    res := make([]int, 0)
    stack := make([]*TreeNode, 0)
    for len(stack) > 0 || root != nil {
        for root != nil {
            res = append(res, root.Val)     // root
            stack = append(stack, root)
            root = root.Left                // left
        }
        // pop
        node := stack[len(stack)-1]
        stack = stack[:len(stack)-1]
        root = node.Right                  // right
    }
    return res
}
```

# Inorder

Access order: `Left -> Root -> Right`

```go
// Recursion
func inorderTraversal(root *TreeNode) []int {
    // Time complexity: O(n)
    // Spatial complexity: O(n)
    res := []int{}
    var inorder func(*TreeNode)
    inorder = func(root *TreeNode) {
        if root == nil {
            return
        }
        inorder(root.Left)          // left
        res = append(res, root.Val)  // root
        inorder(root.Right)         // right
    }
    inorder(root)
    return res
}

// Iterate
func inorderTraversal(root *TreeNode) []int {
    // Time complexity: O(n)
    // Spatial complexity: O(n)
    if root == nil {
        return nil
    }

    res := make([]int, 0)
    stack := make([]*TreeNode, 0)
    for len(stack) > 0 || root != nil {
        for root != nil {
            stack = append(stack, root)
            root = root.Left            // left
        }
        node := stack[len(stack)-1]
        stack = stack[:len(stack)-1]
        res = append(res, node.Val)    // root
        root = node.Right              // right
    }

    return res
}
```

# Postorder

Access order: `Left -> Right -> Root`

```go
// Recursion
func postorderTraversal(root *TreeNode) []int {
    // Time complexity: O(n)
    // Spatial complexity: O(n)
    res := []int{}
    var postorder func(*TreeNode)
    postorder = func(root *TreeNode) {
        if root == nil {
            return
        }
        postorder(root.Left)          // left
        postorder(root.Right)         // right
        res = append(res, root.Val)   // root
    }
    postorder(root)
    return res
}

// Iterate
func postorderTraversal(root *TreeNode) []int {
    // Time complexity: O(n)
    // Spatial complexity: O(n)
    if root == nil {
        return nil
    }

    res := make([]int, 0)
    stack := make([]*TreeNode, 0)
    // will check if right node has been pop
    var lastVisit *TreeNode
    for len(stack) > 0 || root != nil {
        for root != nil {
            stack = append(stack, root)
            root = root.Left                                // left
        }

        node := stack[len(stack)-1]
        // root will pop after right node
        if node.Right == nil || node.Right == lastVisit {
            stack = stack[:len(stack)-1]
            res = append(res, node.Val)                   // root
            lastVisit = node
        } else {
            root = node.Right                             // right
        }
    }
    return res
}
```

# Levelorder

Access order: `from left to right, level by level`

```go
// Recursion (BFS)
func levelorder(root *TreeNode) [][]int {
    // Time complexity: O(n)
    // Spatial complexity: O(n)
    res := [][]int{}
    var bfs func(node *TreeNode, dep int)
    bfs = func(node *TreeNode, dep int) {
        if node == nil {
            return
        }
        if len(res) == dep {
            res = append(res, []int{})
        }
        res[dep] = append(res[dep], node.Val)
        bfs(node.Left, dep+1)
        bfs(node.Right, dep+1)
    }

    bfs(root, 0)
    return res
}

// Iterate
func levelorder(root *TreeNode) [][]int {
    // Time complexity: O(n)
    // Spatial complexity: O(n)
    if root == nil {
        return [][]int{}
    }

    res := make([][]int, 0)
    curr := []*TreeNode{root}
    for len(curr) > 0 {
        next := []*TreeNode{}
        vals := []int{}

        for _, node := range curr {
            vals = append(vals, node.Val) // add curr level node val

            if node.Left != nil {
                next = append(next, node.Left)
            }

            if node.Right != nil {
                next = append(next, node.Right)
            }
        }
        res = append(res, vals)
        curr = next
    }
    return res
}
```
