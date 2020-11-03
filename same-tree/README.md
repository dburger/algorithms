# Same Tree

Given two binary trees, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical and
the nodes have the same value.

## Hints

1. The recursive solution couldn't be easier.

## Solutions

### Recursive solution

```java
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if (p == null && q == null) {
            return true;
        } else if (p == null || q == null) {
            return false;
        } else {
          return p.val == q.val &&
              isSameTree(p.left, q.left) &&
              isSameTree(p.right, q.right);
        }
    }
}
```
