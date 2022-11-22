# Delete Node Linked List

There is a singly-linked list `head` and we want to delete a node `node` in it.

You are given the node to be deleted `node`. You will **not be given access** to
the first node of `head`.

All the values of the linked list are **unique**, and it is guaranteed that the
given node `node` is not the last node in the linked list.

Delete the given node. Note that by deleting the node, we do not mean removing it
from memory. We mean:

- The value of the given node should not exist in the linked list.
- The number of nodes in the linked list should decrease by one.
- All the values before `node` should be in the same order.
- All the values after `node` should be in the same order.

## Hints

1. This problem description is dumb. If you don't mean delete then don't say
   delete.

## Solutions

### Skip

The solution is simple. Given that the problem says you won't be given the last
node in the list you can merely make the given `node` have the value of the next
node and then have it skip over that node. Done. This is not exactly a delete
but the resultant list is what you want.

The time and space complexity of this solution is `O(1)`.

```java
class Solution {
    public void deleteNode(ListNode node) {
        node.val = node.next.val;
        node.next = node.next.next;
    }
}
```
