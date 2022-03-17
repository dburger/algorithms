# Maximum Level Sum of a Binary Tree

Given the `root` of a binary tree, the level of its root is `1`, the level of
its children is `2`, and so on.

Return the **smallest** level `x` such that the sum of al the values of nodes
at level `x` is **maximal**.

## Hints

1. Only one traversal approach fits this very naturally

## Solutions

### BFS my bestie forever

While you could do this with a DFS traversal, it would be a bit awkward. DFS
would require passing an auxillary structure through the recursion to hold the
level sums. Using BFS, we can just calculate the sum as we traverse each level.
This problem can be solved very nicely with a BFS traversal. The BFS traversal
approach used here can be used to solve a number of tree problems. Note the
wrinkle that makes it work so well. It has nested loops where the inner loop
snapshots `size` before starting in order to process a level at a time.

The time comlexity of this approach is O(n) where n is the number of nodes in
the tree. That is you have to visit every node in the tree. The space
complexity is also O(n). This comes from the queue used to hold nodes during
the traversal.

```java
class Solution {
    public int maxLevelSum(TreeNode root) {
        int max = Integer.MIN_VALUE;
        int maxLevel = 0;
        int level = 1;
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        while (!q.isEmpty()) {
            int size = q.size();
            int sum = 0;
            while (size-- > 0) {
                TreeNode n = q.remove();
                sum += n.val;
                if (n.left != null) {
                    q.add(n.left);
                }
                if (n.right != null) {
                    q.add(n.right);
                }
            }
            if (sum > max) {
                max = sum;
                maxLevel = level;
            }
            level++;
        }
        return maxLevel;
    }
}
```

### DFS, awkward but workable

OK, well, for completeness I provide a solution that works with a DFS
traversal. A list passed to the recursion is used to accumulate the per level
sum. Upon return, the list is examined to determine the max level.

The time complexity and space complexity of this solution is O(n) where n is
the number of nodes in the tree. Each node must be examined thus the time
complexity. The space complexity comes from the recursion, where in the worst
case a non-branching tree requires n stackframes deep to complete the
recursion.

```java
class Solution {
    public int maxLevelSum(TreeNode root) {
        List<Integer> accum = new ArrayList<>();
        mls(root, 1, accum);
        int max = Integer.MIN_VALUE;
        int maxLevel = -1;
        for (int i = 0; i < accum.size(); i++) {
            int sum = accum.get(i);
            if (sum > max) {
                max = sum;
                maxLevel = i + 1;
            }
        }
        return maxLevel;
    }

    private void mls(TreeNode n, int level, List<Integer> accum) {
        if (n == null) {
            return;
        }
        int i = level - 1;
        if (i == accum.size()) {
            accum.add(n.val);
        } else {
            accum.set(i, accum.get(i) + n.val);
        }
        mls(n.left, level + 1, accum);
        mls(n.right, level + 1, accum);
    }

}
```
