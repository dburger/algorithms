# Binary Tree Paths

Given the `root` of a binary tree, return all root-to-leaf paths in
**any order**.

A **leaf** is a node with no children.

## Hints

1. Nothing fancy needed here. Keep the current path in a list and every time you
   hit a leaf add it to an accumulator.

## Solutions

### DFS to the ends of the earth

This solution merely uses a DFS traversal while keeping track of the current
path. An accumulator goes along for the ride. When a leaf is detected, the
current path is written into the accumulator. Simple.

The time complexity of this solution is O(n) where `n` is the number of nodes
in the tree. This comes from the need to visit each node in the tree. The
space complexity of this solution is also O(n). This comes from tracking the
current path which could be all the nodes in the tree.

```java
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> curr = new ArrayList<>();
        List<String> accum = new ArrayList<>();
        traverse(root, curr, accum);
        return accum;
    }

    private void traverse(TreeNode node, List<String> curr, List<String> accum) {
        if (node == null) {
            return;
        }
        curr.add(String.valueOf(node.val));
        if (node.left == null && node.right == null) {
            accum.add(String.join("->", curr));
        } else {
            traverse(node.left, curr, accum);
            traverse(node.right, curr, accum);
        }
        curr.remove(curr.size() - 1);
    }
}
```
