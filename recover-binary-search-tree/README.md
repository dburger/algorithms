# Recover Binary Search Tree

You are given the `root` of a binary search tree (BST) where exactly two nodes
of the tree were swapped by mistake. Recover the tree without changing its
structure.

## Hints

1. An inorder traversal of a BST gives a sorted view of the nodes. This can
   be exploited to determine the nodes that are out of place.

## Solutions

### Materialize inorder traversal

This solution proceeds by first accumulating the nodes into a list using an
inorder traversal. For a BST, an inorder traversal gives a sorted list. In
this case two nodes have been swapped. To identify the nodes that have been
swapped you need to identify:

1. The first node with a value greater than its neighbor to the right
1. The last node with a value less than its neighbor to the left

Swapping the value in these two nodes gives the solution.

The time complexity for this solution is O(n) as an inorder traversal of the
tree is performed followed by two linear scans of the list. The space
complexity is also O(n) in order to hold the nodes in a list.

```java
class Solution {
    public void recoverTree(TreeNode root) {
        List<TreeNode> nodes = new ArrayList<>();
        inorder(root, nodes);
        // Determine first swapped node.
        TreeNode first = null;
        for (int i = 0; i < nodes.size() - 1; i++) {
            TreeNode node = nodes.get(i);
            if (node.val > nodes.get(i + 1).val) {
                first = node;
                break;
            }
        }
        // Determine second swapped node.
        TreeNode second = null;
        for (int i = nodes.size() - 1; i > 0; i--) {
            TreeNode node = nodes.get(i);
            if (node.val < nodes.get(i - 1).val) {
                second = node;
                break;
            }
        }
        int temp = second.val;
        second.val = first.val;
        first.val = temp;
    }

    private static void inorder(TreeNode node, List<TreeNode> nodes) {
        if (node == null) {
            return;
        }
        inorder(node.left, nodes);
        nodes.add(node);
        inorder(node.right, nodes);
    }
}
```

### During inorder traversal

This follows the same approach as the prior solution but manages to identify
the first and second nodes during the in order traversal. That is it doesn't
need to materialize the node list.

The result is the same time complexity, 0(n). The space complexity, however,
is O(1), as the node list is eliminated.

```java
class Solution {
    private TreeNode prev = null;
    private TreeNode first = null;
    private TreeNode second = null;

    public void recoverTree(TreeNode root) {
        rt(root);
        int temp = first.val;
        first.val = second.val;
        second.val = temp;
    }

    private void rt(TreeNode node) {
        if (node == null) {
            return;
        }
        rt(node.left);
        if (prev != null && node.val < prev.val) {
            if (first == null) {
                first = prev;
            }
            second = node;
        }
        prev = node;
        rt(node.right);
    }
}
```
