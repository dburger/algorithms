# Merge Two Sorted Lists

Merge two sorted linked lists and return it as a sorted list.
The list should be made by splicing together the nodes of the
first two lists.

## Hints

1. The only wrinkle here that makes this slightly tricky is that
   you are supposed to use the existing nodes and not make new
   ones.

## Solutions

### Typical procedure

This is the typical procedure of comparing the heads of each list
and choosing the smaller. Note how the one loop also handles the
complete exhaustion of the lists. That is it will drain the remaining
list when the other is already done.

The time complexity of this algorithm is O(m + n) where m and n are
the lengths of the respective lists. The space complexity is O(1).

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode result = null;
        ListNode curr = null;
        while (l1 != null || l2 != null) {
            ListNode next = null;
            if (l1 != null && l2 != null) {
                // The still sorting case.
                if (l1.val <= l2.val) {
                    next = l1;
                    l1 = l1.next;
                } else {
                    next = l2;
                    l2 = l2.next;
                }
            } else if (l1 != null) {
                next = l1;
                l1 = l1.next;
            } else {
                next = l2;
                l2 = l2.next;
            }
            if (result == null) {
                result = curr = next;
            } else {
                curr.next = next;
                curr = next;
            }
        }
        return result;
    }
}
```
