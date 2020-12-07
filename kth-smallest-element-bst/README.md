# Kth Smallest Element in a BST

Given a binary search tree, write a function `kthSmallest` to find the
**k**th smallest element in it.

## Hints

The obvious starting point here is that an in-order traversal of a BST
will visit the nodes in an order that will allow you to determine kth
smallest. Accumulating the entire traversal could work. Better yet would
be to visit and return immediately when the correct node is found.

## Solutions

### Recursive In-order

Here we use `KTracker` to track kth smallest coupled with an recursive
in-order approache.

```java
class Solution {
    private class KTracker {
        int k;
    }

    public int kthSmallest(TreeNode root, int k) {
        KTracker ktracker = new KTracker();
        ktracker.k = k;
        TreeNode node = ks(root, ktracker);
        return node.val;
    }

    private TreeNode ks(TreeNode node, KTracker ktracker) {
        if (node == null) {
            return null;
        }
        TreeNode left = ks(node.left, ktracker);
        if (left != null) {
            return left;
        }
        if (--ktracker.k == 0) {
            return node;
        }
        return ks(node.right, ktracker);
    }
}
```

### Iterative In-order

Similar to above but with iteration. In-order iteration is slightly tricky
to come up with. The process uses a stack to travel down left nodes as far
as possible, pops to visit, then proceeds in the same way down that nodes
right child, if any.

```java
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        Stack<TreeNode> stack = new Stack<>();
        while (true) {
            while (root != null) {
                stack.add(root);
                root = root.left;
            }
            root = stack.pop();
            if (--k == 0) {
                return root.val;
            }
            root = root.right;
        }
    }
}
```
