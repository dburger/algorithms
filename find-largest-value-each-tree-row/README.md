# Find Largest Value in Each Tree Row

Given the `root` of a binary tree, return an array of the largest value in each
row of the tree (0-indexed).

## Hints

1. The BFS process tree by level works nicely for this.
1. DFS works as well, try passing down the depth as you recurse.

## Solutions

### BFS level at a time

The BFS traversal approach with nested loops to process a level at a time works
very nicely for this. Here we simply determine the max for each level and add
that to an accumlating list.

The time complexity of this solution is O(n). That is each node of the tree must
be visited to determine max values. The space complexity of this solution is
related to the size of the largest level as the queue used to process the BFS
holds the nodes of 2 levels at a time. This is easily bound by O(n).

```java
class Solution {
    public List<Integer> largestValues(TreeNode root) {
        List<Integer> l = new ArrayList<>();
        if (root == null) {
            return l;
        }
        Deque<TreeNode> d = new LinkedList<>();
        d.addFirst(root);
        int depth = 0;
        while (!d.isEmpty()) {
            int size = d.size();
            int max = d.peekFirst().val;
            for (int i = 0; i < size; i++) {
                TreeNode n = d.removeFirst();
                if (n.val > max) {
                    max = n.val;
                }
                if (n.left != null) {
                    d.addLast(n.left);
                }
                if (n.right != null) {
                    d.addLast(n.right);
                }
            }
            l.add(max);
        }
        return l;
    }
}
```
