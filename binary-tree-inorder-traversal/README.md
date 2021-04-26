# Binary Tree Postorder Traversal

Given the `root` of a binary tree, return the inorder
traversal of its node's values.

## Hints

1. This is one of the classic tree algorithms. You just need to know
   the order to visit within a recursion. In, pre, and post are the
   three variants.

## Solutions

### Typical recursion

The time and space complexity of this approach are O(n). The time
complexity comes from visiting each node exactly once. The space
complexity is obviously holding a list that can contain the value
of each node. The space for the recursion, is also bounded by O(n).

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> acc = new ArrayList<>();
        inorderTraversal(root, acc);
        return acc;
    }

    private void inorderTraversal(TreeNode node, List<Integer> acc) {
        if (node == null) {
            return;
        }
        inorderTraversal(node.left, acc);
        acc.add(node.val);
        inorderTraversal(node.right, acc);
    }
}
```
