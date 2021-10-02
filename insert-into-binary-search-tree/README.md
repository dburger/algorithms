# Insert Into a Binary Search Tree

You are given the `root` node of a binary search tree (BST) and a `value` to
insert into the tree. Return the root node of the BST after the insertion. It
is guaranteed that the new value does not exist in the original BST.

Notice that there may exist multiple valid ways for the insertion, as long as
the tree remains a BST after insertion. You can return any of them.

## Hints

1. Kind of basic stuff. Recursive or iterative should be possible.

## Solutions

### Recurse till ya burst

This solution starts by calling a recursive method on the root of the tree. At
each invocation it tests if we need to move left or right. If we are moving
to where a node already exists we merely recurse down that node. If the node
does not exist we assign a new node there with value `val` and we are done.

The time complexity of this solution is O(n) where n is the number of nodes
in the tree. Remember, this is not a balanced BST so we could end traveling
down every node in the tree. The space complexity of this solution is also
O(n). This comes from the recursive call stack that may need to be invoked
for every node in the tree.

```java
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if (root == null) {
            return new TreeNode(val);
        }
        insert(root, val);
        return root;
    }

    private void insert(TreeNode node, int val) {
        if (val > node.val) {
            if (node.right == null) {
                node.right = new TreeNode(val);
            } else {
                insert(node.right, val);
            }
        } else {
            if (node.left == null) {
                node.left = new TreeNode(val);
            } else {
                insert(node.left, val);
            }
        }
    }
}
```

### Iterate to your fate

In this solution we keep `parent` and `node` references and travel down to
the correct spot in the tree. That menas moving left if `val` is less than
the value in `node` and moving right if `val` is greater than the value in
`node`. This is continued until `node` is null. At this point we have the
correct `parent` when a `null` in the link that needs to be assigned the new
value. A final check is performed to see if this should happen on the left
or right and the assignment is made.

The time complexity of this algorithm, like the above solution, is O(n). The
space complexity of this solution is O(1).

```java
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        TreeNode parent = null;
        TreeNode node = root;
        while (node != null) {
            parent = node;
            if (val > node.val) {
                node = node.right;
            } else {
                node = node.left;
            }
        }
        if (parent == null) {
            root = new TreeNode(val);
        } else if (val > parent.val) {
            parent.right = new TreeNode(val);
        } else {
            parent.left = new TreeNode(val);
        }
        return root;
    }
}
```
