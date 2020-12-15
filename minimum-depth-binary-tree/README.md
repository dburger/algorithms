# Minimum Depth of Binary Tree

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root
node down to the nearest leaf node.

**Note:** A leaf is a node with no children.

## Hints

1. It would seem that a breadth first search looking for the first leaf node is
   the way to go. A depth first approach would require searching the entire tree
   before you could determine the min.

## Solutions

### Breadth first search

Here we use a breadth first search to identify the first level with a leaf node.
That is the minimum depth. Note the usage of `null` separators to determine
levels.

```java
class Solution {
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        Deque<TreeNode> d = new LinkedList<>();
        // We'll push nodes and use nulls to separate levels.
        d.addFirst(root);
        d.addLast(null);
        int level = 1;
        while (!d.isEmpty()) {
            TreeNode node = d.removeFirst();
            if (node == null) {
                if (!d.isEmpty()) {
                    // There is another level of nodes already added,
                    // add the separator.
                    d.addLast(null);
                }
                level++;
            } else {
                if (node.left == null && node.right == null) {
                    return level;
                }
                if (node.left != null) {
                    d.addLast(node.left);
                }
                if (node.right != null) {
                    d.addLast(node.right);
                }
            }
        }
        // It will never get to here. A leaf node is always found first.
        return -1;
    }
}
```
