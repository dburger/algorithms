# Reverse Linked List

Reverse a singly linked list.

## Hints

## Solutions

### Recursive

The recursive solution recurses on previous and current nodes. It starts with
`prev = null` and `curr = head`. At each invocation it points the current node
at the previous node. Then it checks `curr.next`. If this is null, we are at
the end of the list and we can return the new head. If this is not null, we
continue to recurse.

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null) {
            return head;
        }
        return rl(null, head);
    }

    private ListNode rl(ListNode prev, ListNode curr) {
        curr.next = prev;
        ListNode temp = curr.next;
        return temp == null ? curr : rl(curr, temp);
    }
}
```
