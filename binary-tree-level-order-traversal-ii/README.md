# Binary Tree Level Order Traversal II

Given the `root` of a binary tree, return the bottum-up level order traversal
of its nodes` values. (i.e., from left to right, level by level from leaf to
root).

## Hints

1. This can be solved with DFS and BFS approaches. The BFS approach seems
   more natural.

## Solutions

### BFS, naturally

This solution uses a "level at a time" BFS style traversal. This makes it
easy to insert the new level into the front of the accumulating list. By
doing this into a `LinkedList` through the `Deque` interface, we avoid
shuffling entries around when we insert into the front.

The time complexity of this solution is O(n). That is each node in the tree
must be visited. The space complexity is also O(n), for the queue (via the
`Deque` interface) used to facilitate the BFS traversal.

```java
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        Deque<List<Integer>> result = new LinkedList<>();
        Deque<TreeNode> d = new LinkedList<>();
        if (root != null) {
            d.addLast(root);
        }
        while (!d.isEmpty()) {
            List<Integer> level = new ArrayList<>();
            int size = d.size();
            while (size-- > 0) {
                TreeNode n = d.removeFirst();
                level.add(n.val);
                if (n.left != null) {
                    d.addLast(n.left);
                }
                if (n.right != null) {
                    d.addLast(n.right);
                }
            }
            result.addFirst(level);
        }
        // return new ArrayList<>(result);
        return (List) result;
    }
}
```
