# Path Sum II

Given a binary tree and a sum, find all root-to-leaf paths where each path's
sum equals the given sum.

## Hints

1. This is just [Path Sum](path-sum) with the extension of:
   * Keeping track of the path, not just the sum.
   * Accumulating all matching paths, not just true or false.

## Solutions

### Recursive recorder

Recursion is used while keeping track of the current path and providing an
accumulator.

```java
class Solution {
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        List<List<Integer>> acc = new ArrayList<>();
        ps(root, sum, 0, new ArrayList<>(), acc);
        return acc;
    }

    private void ps(TreeNode node, int target, int sum, List<Integer> path, List<List<Integer>> acc) {
        if (node == null) {
            return;
        }
        sum += node.val;
        path.add(node.val);
        if (node.left == null && node.right == null && sum == target) {
            acc.add(new ArrayList<>(path));
            path.remove(path.size() - 1);
            return;
        } else {
            ps(node.left, target, sum, path, acc);
            ps(node.right, target, sum, path, acc);
            path.remove(path.size() - 1);
        }
    }
}
```
