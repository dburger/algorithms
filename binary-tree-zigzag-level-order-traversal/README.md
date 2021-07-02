# Binary Tree ZigZag Level Order Traversal

Given the `root` of a binary tree, return the zigzag level order of its node's
values. (i.e., from left to right, then right to left for the next level and
alternate between).

## Hints

1. Level order means starting with a BFS.

## Solutions

### BFS and tracking

This solution works by doing a BFS that adds the nodes to a queue wrapped in a
`Node` with a `level` field. This allows nested loops where the inner loop
pulls out a level at a time. A `reverse` boolean is kept that indicates whether
or not to reverse a particular level before adding it to the result.

```java
class Solution {
    private static class Node {
        int level;
        TreeNode node;
        Node(int level, TreeNode node) {
            this.level = level;
            this.node = node;
        }
    }

    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        boolean reverse = false;
        Queue<Node> q = new LinkedList<>();
        q.add(new Node(0, root));
        while (!q.isEmpty()) {
            List<Integer> part = new ArrayList<>();
            Node n = q.peek();
            int level = n.level;
            while (n != null && n.level == level) {
                n = q.remove();
                part.add(n.node.val);
                if (n.node.left != null) {
                    q.add(new Node(n.level + 1, n.node.left));
                }
                if (n.node.right != null) {
                    q.add(new Node(n.level + 1, n.node.right));
                }
                n = q.peek();
            }
            if (reverse) {
                Collections.reverse(part);
            }
            result.add(part);
            reverse = !reverse;
        }
        return result;
    }
}
```
