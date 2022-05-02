# Cousins in Binary Tree

Given the `root` of a binary tree with unique values and the vlaues of two
different nodes of the tree `x` and `y`, return `true` if the nodes
corresponding to the values `x` and `y` in the tree are **cousins**, or
`false` otherwise.

Two nodes of a binary tree are **cousins** if they have the same depth with
different parents.

Note that in a binary tree, the root node is at the depth `0`, and the children
of each depth `k` node are at the depth `k + 1`.

## Hints

1. Rather straightforward tree traversal keeping track of depth.

## Solutions

### For the children

This solution uses a typical depth first tree traversal where we keep track
of the depth. We continue until we record the depth and parents of the desired
nodes.

The time and space complexity of this solution is O(n) where `n` is the number
of nodes in the tree. The time complexity comes from the potential need to
explore each node. The space complexity comes from the stack frames that will
extend to the depth of the tree. In the worst case, there is no branching and
this depth is `n`.

```java
class Solution {
    private TreeNode cxParent = null;
    private int cxDepth = 0;
    private TreeNode cyParent = null;
    private int cyDepth = 0;

    public boolean isCousins(TreeNode root, int x, int y) {
        findCousins(root, x, y, 0);
        return cxDepth == cyDepth && cxDepth > 0 && cxParent.val != cyParent.val;
    }

    private void findCousins(TreeNode node, int x, int y, int depth) {
        if (node == null) {
            return;
        }
        if (node.left != null) {
            int val = node.left.val;
            if (val == x) {
                cxParent = node;
                cxDepth = depth;
            } else if (val == y) {
                cyParent = node;
                cyDepth = depth;
            }
        }
        if (node.right != null) {
            int val = node.right.val;
            if (val == x) {
                cxParent = node;
                cxDepth = depth;
            } else if (val == y) {
                cyParent = node;
                cyDepth = depth;
            }
        }
        if (cxParent == null || cyParent == null) {
            findCousins(node.left, x, y, depth + 1);
        }
        if (cxParent == null || cyParent == null) {
            findCousins(node.right, x, y, depth + 1);
        }
    }
}
```
