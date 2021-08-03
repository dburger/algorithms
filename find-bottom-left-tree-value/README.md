# Find Bottom Left Tree Value

Given the `root` of a binary tree, return the leftmost value in the last row
of the tree.

## Hints

1. Think of how typical BFS and DFS traversals work. In BFS, the bottom left
   most is the first node encountered on the last level. In DFS, the bottom
   left most the the first node encountered at the deepest depth the tree
   contains.

## Solutions

### BFS first node on level

Executing a BFS traversal and recording the first node seen on the level is an
easy approach to solving this problem. We only need store one reference to the
"first" node as it will be continually written over until the last level is
encountered and the correct value is written into the reference. This solution
uses the nested loop trick of processing a tree level at a time.

The time complexity of this solution is O(n), as each node of the tree must be
traversed. The space complexity of this solution is O(largest level). This is
easily bounded by O(n).

```java
class Solution {
    public int findBottomLeftValue(TreeNode root) {
        Deque<TreeNode> d = new LinkedList<>();
        d.push(root);
        TreeNode bottomLeft = null;
        while (!d.isEmpty()) {
            bottomLeft = d.peekFirst();
            int size = d.size();
            for (int i = 0; i < size; i++) {
                TreeNode n = d.removeFirst();
                if (n.left != null) {
                    d.addLast(n.left);
                }
                if (n.right != null) {
                    d.addLast(n.right);
                }
            }
        }
        return bottomLeft.val;
    }
}
```

### DFS first node at deepest depth

We can also solve this problem with a DFS traversal approach. To use this
approach we keep track of the depth and record the node value for the deepest
depth we have seen so far. The first node encountered at each depth is the left
node for each row. Thus this approach will end up recording the leftmost value
in the last row of the tree.

This solution passes the mutable values, max depth seen and leftmost value at
that level, in a two element array.

The time complexity of this solution is O(n) because each node in the tree must
be traversed. The space complexity corresponds to the depth of the recursion /
depth of the tree. This is bounded by O(n).

```java
class Solution {
    public int findBottomLeftValue(TreeNode root) {
        int[] tracker = new int[2];
        tracker[0] = -1;
        fblv(root, 0, tracker);
        return tracker[1];
    }

    private void fblv(TreeNode node, int depth, int[] tracker) {
        if (node == null) {
            return;
        }
        if (node.left == null && node.right == null) {
            if (depth > tracker[0]) {
                tracker[0] = depth;
                tracker[1] = node.val;
            }
        } else {
            fblv(node.left, depth + 1, tracker);
            fblv(node.right, depth + 1, tracker);
        }
    }
}
```
