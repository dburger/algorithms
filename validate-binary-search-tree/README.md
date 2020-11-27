# Validate Binary Search Tree

Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

* The left subtree of a node contains only nodes with keys **less than**
  the node's key.
* The right substree of a node contains only nodes with keys **greater than**
  the node's key.
* Both the left and right subtrees must also be binary search trees.

## Hints

1. Think recursion and passing the bounds for a subtree downwards.

## Solutions

### Recursion with bounds

Here the bounds - you need to be greater than `lo` and less than `hi` are
passed down in recursive calls.

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return isValidBST(root, null, null);
    }

    private boolean isValidBST(TreeNode node, Integer lo, Integer hi) {
        if (node == null) return true;
        int val = node.val;
        // Check the bounds.
        if (lo != null && val <= lo) return false;
        if (hi != null && val >= hi) return false;
        // Left subtree nodes are now bound by this node's val on the hi side.
        // Right subtree nodes are now bound by this node's val on the lo side.
        return isValidBST(node.left, lo, val) && isValidBST(node.right, val, hi);
    }
}
```

### Iterative - handle the recursion with a stack

This is equivalent to the recursive solution but has been converted to iterative.
Three stacks are used to hold what would have been the parameters to each call.
This is a good demo of converting recursion to iteration.

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        LinkedList<TreeNode> nodes = new LinkedList<>();
        LinkedList<Integer> los = new LinkedList<>();
        LinkedList<Integer> his = new LinkedList<>();

        pushAll(root, null, null, nodes, los, his);

        while (!nodes.isEmpty()) {
          TreeNode node = nodes.pop();
          Integer lo = los.pop();
          Integer hi = his.pop();

          if (node == null) continue;
          int val = node.val;
          if (lo != null && val <= lo) return false;
          if (hi != null && val >= hi) return false;

          pushAll(node.left, lo, val, nodes, los, his);
          pushAll(node.right, val, hi, nodes, los, his);
        }

        return true;
    }

    private void pushAll(TreeNode node, Integer lo, Integer hi, LinkedList<TreeNode> nodes, LinkedList<Integer> los, LinkedList<Integer> his) {
      nodes.push(node);
      los.push(lo);
      his.push(hi);
    }
}
```
