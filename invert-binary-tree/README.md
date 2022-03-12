# Invert Binary Tree

## Hints

1. Classic recursion susceptible tree problem

## Solutions

### I recurse you!

As noted in the hint above, this problem easily falls to a classic recursive
approach to handling trees. The node is examined. If `null`, `null` is
returned. If it is not `null`, the `left` and `right` subtrees are recursed
upon and they are swapped in the current node.

The time complexity of this solution is O(n), because each node is visited
once. The space complexity of this solution is also O(n). This comes from the
stack frames used to handle the recursion, which works out to the depth of the
tree. In the worst case the tree has no branching and thus the O(n).

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return null;
        }
        TreeNode left = invertTree(root.left);
        TreeNode right = invertTree(root.right);
        root.left = right;
        root.right = left;
	return root;
    }
}
```
