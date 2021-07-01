# Unique Binary Search Trees II

Given an integer `n`, return all the structurally unique BST's (binary search
trees) which has exactly `n` nodes of unique values from `1` to `n`. Return the
answer in any order.

## Hints

1. Similar to [Unique Binary Search Trees](../unique-binary-search-trees), the
   recursive approach can be adjusted to return a list of `TreeNode` instead
   of a count.

## Solutions

### Recursion is magic

This recursive approach is quite nice. It builds up the trees for `n` nodes by
iterating through the root being at each position. That is:

* `0` nodes left of root, `n - 1` nodes right of root
* `1` node left of root, `n - 2` nodes right of root
* ...
* `n - 2` nodes left of root, `1` node right of root
* `n - 1` nodes left of root, `0` nodes right of root

The recursive calls return the list of nodes for the sizes of subtrees down the
left link and the right link. The root and those subtrees are put together to
form a resultant BST.

Note how the numbering of the nodes, and keeping the BST properties,  is nicely
handled by the parameterization through `lo` and `hi`. They do double duty
specifying the count of nodes and the start to finsish numbering.

```java
class Solution {
    public List<TreeNode> generateTrees(int n) {
        return gt(1, n);
    }

    private static List<TreeNode> gt(int lo, int hi) {
        List<TreeNode> result = new ArrayList<>();
        if (lo > hi) {
            result.add(null);
        } else if (lo == hi) {
            result.add(new TreeNode(lo, null, null));
        } else {
            for (int i = lo; i <= hi; i++) {
                List<TreeNode> left = gt(lo, i - 1);
                List<TreeNode> right = gt(i + 1, hi);
                for (TreeNode l : left) {
                    for (TreeNode r : right) {
                        result.add(new TreeNode(i, l, r));
                    }
                }
            }
        }
        return result;
    }
}
```
