# Balance a Binary Search Tree

Given the `root` of a binary search tree, return a **balanced** binary
search tree with the same node values. If there is more than one answer,
return **any of them**.

A binary search tree is **balanced** if the depth of the two subtrees of
every node never differs by more than `1`.

## Hints

1. Sort, find the middle, and recurse on both sides?

## Solutions

### Inorder accumulate, recurse away

This solution follows the hint. To accomplish the "sort" we perform an
accumulating inorder traversal. With this list, we use the obvious find
the middle, make that the `root`, and then set `root.left` and `root.right`
via recursive invocations of this same method.

The time complexity of this solution is O(n) where n is the number of nodes
in the tree. That is each node of the tree must be visited twice. Once on
the intitial inorder traversal and a second time on the recursive building
of the tree. The space complexity of this solution is also O(n), as the
accumulator for the traversal must hold every node of the tree.

```java
class Solution {
    public TreeNode balanceBST(TreeNode root) {
        List<TreeNode> nodes = new ArrayList<>();
        inorder(root, nodes);
        root = makeTree(nodes, 0, nodes.size() - 1);
        return root;
    }

    private void inorder(TreeNode node, List<TreeNode> acc) {
        if (node == null) {
            return;
        }
        inorder(node.left, acc);
        acc.add(node);
        inorder(node.right, acc);
    }

    private TreeNode makeTree(List<TreeNode> nodes, int lo, int hi) {
        if (lo > hi) {
            return null;
        }
        int mid = lo + (hi - lo) / 2;
        TreeNode root = nodes.get(mid);
        root.left = makeTree(nodes, lo, mid - 1);
        root.right = makeTree(nodes, mid + 1, hi);
        return root;
    }
}
```
