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

The time complexity of the solution is O(n^2). While the BFS visits each
node and is thus O(n), the need to reverse every other level level requires
a reversal of a list bounded by the size of the tree and the depth of the tree.
If we can assume this tree is balanced this becomes O(n * log(n)).

The space complexity of this solution is O(log(n)). This is for the queue that
needs to be made to process the BFS traversal. In practice it only holds a
level at a time.

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

### BFS without the wrapper

The above solution is good and easy to understand. The wrapper class `Node`,
however, is not necessary. You can process a level at a time in the inner loop
by snapshotting the size of the queue and removing that many nodes. While
processing those nodes only the next layer is added to the queue and the
process repeats. This results in less code than the prior approach.



```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        boolean reverse = false;
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        while (!q.isEmpty()) {
            List<Integer> part = new ArrayList<>();
            // We must snapshot the size as it will grow during the loop.
            // This is how we process one level at a time.
            int len = q.size();
            for (int i = 0; i < len; i++) {
                TreeNode n = q.remove();
                part.add(n.val);
                if (n.left != null) {
                    q.add(n.left);
                }
                if (n.right != null) {
                    q.add(n.right);
                }
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
