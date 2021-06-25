# Convert Sorted Array to Binary Search Tree

Given an integer array `nums` where the elements are sorted in **ascending order**, convert it to
a **height-balanced** binary search tree.

A **height-balanced** binary tree is a binary tree in which the depth of the two subtrees of
every node never differs by more than one.

## Hints

1. Determine a mid-point and recursively construct the left and right.

## Solutions

### Mid-point and recurse

This algorithm follows the obvious approach of determining the midpoint of the array, making
a node for that, and then setting left and right from recursing on the left and right
subarrays.

The time complexity of this solution is O(n) from visiting each position in the array once. The
space complexity for this solution is O(log(n)). This comes from the recursive calls stack
frames in reaching the bottom of the tree.

```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return sa(nums, 0, nums.length - 1);
    }

    private static TreeNode sa(int[] nums, int lo, int hi) {
        if (lo > hi) {
            return null;
        }
        int mid = lo + (hi - lo) / 2;
        TreeNode root = new TreeNode(nums[mid]);
        root.left = sa(nums, lo, mid - 1);
        root.right = sa(nums, mid + 1, hi);
        return root;
    }
}
```
