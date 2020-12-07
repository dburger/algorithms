# Deepest Leaves Sum

Given a binary tree, return the sum of values of its deepest leaves.

## Hints

1. A BFS iteration through the tree could iterate through the deepest
   nodes last.
1. If you knew the depth of the tree, you could use that information
   to harvest only those nodes.

## Solutions

### BFS

This solution uses a `Deque` to do a BFS iteration through the tree in
levels. `null`s are used to separate the levels in the Deque. The sum is
kept current, and the sum of the last level is returned.

```java
class Solution {
    public int deepestLeavesSum(TreeNode root) {
        if (root == null) {
            return 0;
        }
        Deque<TreeNode> d = new LinkedList<>();
        // nulls will be used to mark separators between levels
        d.addLast(root);
        d.addLast(null);
        int sum = 0;
        while (!d.isEmpty()) {
            TreeNode node = d.removeFirst();
            if (node == null) {
                if (!d.isEmpty()) {
                    // We reached the end of a level and there are more nodes.
                    // Set the sum to zero as the deepest sum is yet to come.
                    // Add the separator after this levels children.
                    sum = 0;
                    d.addLast(null);
                }
            } else {
                // Add this nodes children.
                if (node.left != null) {
                    d.addLast(node.left);
                }
                if (node.right != null) {
                    d.addLast(node.right);
                }
                // Keep the current sum up to date.
                sum += node.val;
            }
        }
        return sum;
    }
}
```

### Determine the depth and use it

This solution takes a first path to determine the depth of the tree.
A second pass then uses this depth to generate the sum.

```java
class Solution {
    public int deepestLeavesSum(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int depth = treeDepth(root);
        return depthSum(root, depth);
    }

    private int treeDepth(TreeNode node) {
        if (node == null) {
            return 0;
        }
        return Math.max(treeDepth(node.left), treeDepth(node.right)) + 1;
    }

    private int depthSum(TreeNode node, int depth) {
        if (node == null) {
            return 0;
        }
        if (--depth == 0) {
            return node.val;
        } else {
            return depthSum(node.left, depth) + depthSum(node.right, depth);
        }
    }
}
```
