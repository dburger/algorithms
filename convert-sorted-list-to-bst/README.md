# Convert Sorted List to Binary Search Tree

Given the `head` of a singly linked list where elements are in
**sorted ascending order**, convert it to a heigh balanced BST.

For this problem, a height-balanced binary tree is defined as a binary
tree in which the depth of the two subtrees of every node never differ
by more than 1.

## Hints

1. The natural approach would seem to be to determine the middle of the list
   and then make that the root node, with subtrees from what is to the left
   and to the right. This implies a recursive approach.

## Solutions

### Determine middle, recurse on both sides

Following the hint, this approach determines the middle, makes a root node
from that, and then recurses on both sides.

```java
class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        if (head == null) {
            return null;
        }
        int length = 0;
        ListNode curr = head;
        while (curr != null) {
            length++;
            curr = curr.next;
        }
        return toBst(head, length);
    }

    private TreeNode toBst(ListNode head, int length) {
        if (length < 1) {
            return null;
        }
        // Determine middle element and walk to it.
        int mid = length / 2;
        ListNode curr = head;
        for (int i = 0; i < mid; i++) {
            curr = curr.next;
        }
        // Make a TreeNode for that element and then set left
        // and right by recursing on each side.
        TreeNode node = new TreeNode(curr.val);
        node.left = toBst(head, mid);
        node.right = toBst(curr.next, length - mid - 1);
        return node;
    }
}
```
