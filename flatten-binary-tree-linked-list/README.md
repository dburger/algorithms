# Flatten Binary Tree to Linked List

Given the `root` of a binary tree, flatten the tree into a "linked list":

*   The "linked list" should use the same `TreeNode` class where the `right` child pointer points
    to the next node in the list and the `left` child pointer is always `null`.
*   The "linked list" should be in the same order as a **pre-order traversal** of the binary tree.

## Hints

1. Standard pre-order with an accumulator should be trivial?
1. Doing this in place involves repeated subtree movements. Can you see how to do it?

## Solutions

### Accumulate

Here we merely follow the first hint and do a standard pre-order traversal with an accumulator.

The time complexity for this solution is O(n). This comes from two traversals of the nodes of the
tree. The first is the pre-order traversal and the second is the traversal through the
accumulator to connect the list. The space complexity of this solution is O(n) as the accumulator
is sized to hold all the nodes of the tree.

```java
class Solution {
    public void flatten(TreeNode root) {
        List<TreeNode> acc = new ArrayList<>();
        inorder(root, acc);
        for (int i = 0; i < acc.size(); i++) {
            TreeNode node = acc.get(i);
            node.left = null;
            node.right = i < acc.size() - 1 ? acc.get(i + 1) : null;
        }
    }

    private void inorder(TreeNode node, List<TreeNode> acc) {
        if (node == null) {
            return;
        }
        acc.add(node);
        inorder(node.left, acc);
        inorder(node.right, acc);
    }
}
```

### In place transplanting

This is pretty cool. With this approach we reconfigure the tree to the correct shape in place.
This boils down to two cases that are repeated until done.

*   The node we are looking at doesn't have a left child. Merely proceed down the right child.
*   The node we are looking at has a left child. Identify the last pre-order node in the subtree
    starting from this left child. Transplant the current node's right subtree onto this node.
    Make the current node's left subtree its right subtree and null out the left.

This process will end up with the tree in the correct shape.

The time complexity for this solution is O(n) as we must visit each node in the iteration. Note
that each node may be "visited" three times:

*   When it is `curr` and you move its right subtree down the last pre-order node in its left
    subtree.
*   When it is `curr` and you advance past it as it no longer has a left subtree.
*   When you iterate through it when finding the last pre-order node in the left subtree.

The space complexity for this solution is O(1) only one additional node pointer is allocated for
the iteration.

```java
class Solution {
    public void flatten(TreeNode root) {
        TreeNode curr = root;
        while (curr != null) {
            if (curr.left == null) {
                curr = curr.right;
            } else {
                TreeNode pre = curr.left;
                while (pre.right != null) {
                    pre = pre.right;
                }
                pre.right = curr.right;
                curr.right = curr.left;
                curr.left = null;
            }
        }
    }
}
```
