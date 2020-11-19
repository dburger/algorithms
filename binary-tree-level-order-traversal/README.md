# Binary Tree Level Order Traversal

Given a binary tree, return the level order traversal of its node's
values. (ie, from left to right, level by level).

## Hints

1. Think about this...when I give the hint the solution is too easy.
   (Use recursion with a depth indicator. Accumulate as per the depth.)

## Solutions

### Recursion with depth indicator

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> acc = new ArrayList<>();
        lo(root, 0, acc);
        return acc;
    }

    private void lo(TreeNode node, int depth, List<List<Integer>> acc) {
        if (node == null) {
            return;
        }
        if (depth < acc.size()) {
            acc.get(depth).add(node.val);
        } else {
            List<Integer> level = new ArrayList<>();
            level.add(node.val);
            acc.add(level);
        }
        lo(node.left, depth + 1, acc);
        lo(node.right, depth + 1, acc);
    }
}
```
