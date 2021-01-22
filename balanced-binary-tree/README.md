# Balanced Binary Tree

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as *a binary tree in
which the left and right subtrees of every node different in height by at most
1.*

## Hints

## Solutions

### Literal recursive translation

This solution is pretty much a literal recursive translation.

On leeter this solution performs well. That is it clocks in at 0 ms and thus
is as faster / faster than 100% of the solutions. I think this is a flaw in the
leeter test cases. This algorith is O(n^2) in time complexity. This comes from
the calls to `height`. Imagine that the tree is set up so that the balance
difference is seen at the two nodes directly attached to the root. That is say
the left subtree is a balanced tree of height `x`, and the right subtree is a
balanced subtree of height `x + 2`. In this case `height` would be called on
all the from the bottom of the tree up to the root node until the problematic
height difference is discovered.

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        if (root == null) {
            return true;
        }

        return isBalanced(root.left) && isBalanced(root.right) &&
                Math.abs(height(root.left) - height(root.right)) < 2;
    }

    private int height(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return root == null ? 0 :
                Math.max(height(root.left), height(root.right)) + 1;
    }
}
```

### Height with sentinel

This solution avoids the time complexity problem of the prior solution by
using a `height` function to recursively calculate the height of the tree.
This is O(n) as each node is visited only once as the height travels back
up the tree in the returns from the recursive calls. Note that `-1` is used
as a sentinel value to indicate that an imbalance was observed and thus
operates as a stopping case.

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return height(root) != -1;
    }

    private int height(TreeNode root) {
        if (root == null) {
            return 0;
        }

        int left = height(root.left);
        int right = height(root.right);

        if (left == -1 || right == -1 || Math.abs(left - right) > 1) {
            return -1;
        }

        return Math.max(left, right) + 1;
    }
}
```
