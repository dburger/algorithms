# Lowest Common Ancestor of a Binary Search Tree

Given a binary search tree (BST), find the lowest common ancestor (LCA) node of
two given nodes in the BST.

According to the definition of LCA on Wikipedia: "The lowest common ancestor is
defined between two nodes `p` and `q` as the lowest node in `T` that has both
`p` and `q` as descendants (where we allow **a node to be a descendant of
itself**)."

## Hints

1. Don't forget the "search".

## Solutions

### Re-root me

Usually in these problems you need to use all the information at your disposal
to solve the problem. In other words, this is not only a binary tree but it is
a binary **search** tree. Thus, starting at a node, if one of `p` and `q` has
a smaller value than the node and one has a larger value than the node then we
have the correct node. If not, both or either smaller or greater and we know
which way to navigate. Notice how this accounts for the case of current node
being one of `p` or `q`.

The running time of this algorithm is O(n) where `n` is the number of elements
in the tree. This comes from the possible need to visit almost all the nodes.
The space complexity in this implementation is O(1).

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        while (root != null) {
            if (p.val < root.val && q.val < root.val) {
                root = root.left;
            } else if (p.val > root.val && q.val > root.val) {
                root = root.right;
            } else {
                break;
            }
        }
        return root;
    }
}
```

### Re-root me, recursively

Let's do that same strategy but recursively. The space complexity changes to
O(n) to account for the recursive stack frames.

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null) {
            return null;
        } else if (p.val < root.val && q.val < root.val) {
            return lowestCommonAncestor(root.left, p, q);
        } else if (p.val > root.val && q.val > root.val) {
            return lowestCommonAncestor(root.right, p, q);
        } else {
            return root;
        }
    }
}
```
