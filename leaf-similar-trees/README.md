# Leaf Similar Trees

Consider all the leaves of a binary tree, from left to right order, the values
of those leaves for a **leaf value sequence**.

Two binary trees are considered leaf-similar if their leaf value sequence is
the same.

Return `true` if and only if two given trees with head nodes `root` and `root2`
are leaf similar.

## Hints

1. Accumulate and compare?

## Solutions

### Yeah, accumulate leaves and compare

Here we merely initiate a depth first traversal that accumulates leaf nodes.
Then a simple list comparison can be made.

The time complexity of this solution is O(m + n) where m is the size of one
tree and n is the size of the other. The space complexity is O(max(m, n)),
because of the stack frames necessary to hold recursive tree traversals.

```java
class Solution {
    public boolean leafSimilar(TreeNode root1, TreeNode root2) {
        List<Integer> l1 = leaves(root1);
        List<Integer> l2 = leaves(root2);
        return l1.equals(l2);
    }

    private List<Integer> leaves(TreeNode root) {
        List<Integer> acc = new ArrayList<>();
        leaves(root, acc);
        return acc;
    }

    private void leaves(TreeNode node, List<Integer> acc) {
        if (node == null) {
            return;
        } else if (node.left == null && node.right == null) {
            acc.add(node.val);
        } else {
            leaves(node.left, acc);
            leaves(node.right, acc);
        }
    }
}
```
