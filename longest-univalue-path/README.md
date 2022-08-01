# Longest Univalue Path

Given the `root` of a binary tree, return the length of the longest path, where
each node in the path has the same value. This path may or may not pass through
the root.

**The length of the path** between two nodes is represented by the number of
edges between them.

## Hints

1. Trust recursion.

## Solutions

### Recursion to the rescue

Like a lot of these types of problems a rather simple solution can be had with
a recursive approach. The following solution just recurses to the longest path
below a node, and then checks if the child nodes are of the same value. If they
are, then those lengths are included in the consideration of the parent length.

The time complexity of this solution is O(n) where `n` is the number of
elements in the tree. This comes from having to visit every node in the tree.
The space complexity of this solution O(d) where `d` is the depth of the tree.
This comes from the recursive calls to traverse the tree. In the worst case
this would be O(n).

```java
class Solution {
    private int max = 0;
    public int longestUnivaluePath(TreeNode root) {
        visit(root);
        return max;
    }

    private int visit(TreeNode node) {
        if (node == null) {
            return 0;
        }

        int left = visit(node.left);
        int right = visit(node.right);

        int longLeft = 0;
        int longRight = 0;

        if (node.left != null && node.val == node.left.val) {
            longLeft = 1 + left;
        }

        if (node.right != null && node.val == node.right.val) {
            longRight = 1 + right;
        }

        int nodeMax = longLeft + longRight;
        if (nodeMax > max) {
            max = nodeMax;
        }

        // The longest path from node would use the longer from
        // the children.
        return Math.max(longLeft, longRight);
    }
}
```
