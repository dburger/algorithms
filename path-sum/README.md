# Path Sum

Given a binary tree and a sum, determine if the tree has a root-to-leaf path
such that adding up all the values along the path equals the given sum.

## Hints

1. Recursion is your friend and only leafs qualify.

## Solutions

### Recursion

This is a nice little recursion exercise. The time complexity is O(n) where
n is the number of nodes in the tree. The algorithm may need to examine every
node.

```java
class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        if (root == null) {
            return false;
        }
        return hasPathSum(root, sum, 0);
    }

    private boolean hasPathSum(TreeNode node, int target, int sum) {
        if (node == null) {
            return false;
        }
        sum += node.val;
        if (node.left == null && node.right == null) {
            return sum == target;
        } else {
            return hasPathSum(node.left, target, sum) || hasPathSum(node.right, target, sum);
        }
    }
}
```
