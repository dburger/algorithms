# Binary Tree Right Side View

Given the `root` of a binary tree, imagine yourself standing on the
**right side** of it, return the values of the nodes you can see ordered
from top to bottom.

## Hints

1. A certain traversal makes this workable.

## Solutions

### BFS is my best friend

Once again the old BFS traversal makes a tree problem trivial. Here we
traverse the tree in layers, adding the last value in each layer to
our result.

The time complexity of this solution is O(n) where `n` is the number of
nodes in the tree. This is from visiting each node. The space complexity
of this solution is also O(n). In practice the most space used is the
largest layer which is certainly bound by `n`.

```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        while (!q.isEmpty()) {
            int size = q.size();
            while (size-- > 0) {
                TreeNode node = q.remove();
                if (node.left != null) {
                    q.add(node.left);
                }
                if (node.right != null) {
                    q.add(node.right);
                }
                if (size == 0) {
                    result.add(node.val);
                }
            }
        }
        return result;
    }
}
```
