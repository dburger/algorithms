# Most Frequent Subtree Sum

Given the `root` of a binary tree, return the most frequent **subtree sum**.
If there is a tie, return all the values with the highest frequency in any
order.

The **subtree sum** of a node is defined as the sum of all the node values
formed by the subtree rooted at that node (including the node itself).

## Hints

1. A traversal with a counter should do the trick.

## Solutions

This problem seems like it could have been an easy.

### DFS let me count the ways

This solution uses a recursive depth first search with a counter passed to
track the sums. Like I noted above, it seems like this problem should be rated
easy.

The time complexity of this solution is O(n) where `n` is the number of
elements in the tree. Likewise, the space complexity is also O(n). This comes
from the recursive stackframes used in the solution. In the worst case there
is no branching and the stackframe depth becomes `n` deep.

```java
class Solution {
    public int[] findFrequentTreeSum(TreeNode root) {
        Map<Integer, Integer> counts = new HashMap<>();
        ffts(root, counts);
        int maxCount = Integer.MIN_VALUE;
        int seenCount = 0;
        for (int count : counts.values()) {
            if (count > maxCount) {
                maxCount = count;
                seenCount = 1;
            } else if (count == maxCount) {
                seenCount++;
            }
        }
        int i = 0;
        int[] result = new int[seenCount];
        for (Map.Entry<Integer, Integer> entry : counts.entrySet()) {
            if (entry.getValue() == maxCount) {
                result[i++] = entry.getKey();
            }
        }
        return result;
    }

    private int ffts(TreeNode node, Map<Integer, Integer> counts) {
        if (node == null) {
            return 0;
        }

        int count = node.val + ffts(node.left, counts) + ffts(node.right, counts);
        counts.put(count, counts.getOrDefault(count, 0) + 1);
        return count;
    }
}
```
