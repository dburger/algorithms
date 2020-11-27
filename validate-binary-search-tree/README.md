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

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }
        // All left subtree nodes must be less than root.
        // All right subtree nodes must be greater than root.
        return isValidBST(root.left, null, root.val) && isValidBST(root.right, root.val, null);
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
