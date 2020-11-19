# Binary Tree Postorder Traversal

Given the `root` of a binary tree, return the postorder
traversal of its node's values.

## Hints

1. This is one of the classic tree algorithms. You just need to know
   the order to visit within a recursion. In, pre, and post are the
   three variants.

## Solutions

### Typical recursion

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> acc = new ArrayList<>();
        pt(root, acc);
        return acc;
    }

    private void pt(TreeNode node, List<Integer> acc) {
        if (node == null) {
            return;
        }
        pt(node.left, acc);
        pt(node.right, acc);
        acc.add(node.val);
    }
}
```
