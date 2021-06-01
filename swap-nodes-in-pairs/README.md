# Swap Nodes in Pairs

Given a linked list, swap every two adjacent nodes and return its head. You
must solve the problem without modifying the values in the list's nodes
(i.e., only the nodes themselves may be changed.)

## Hints

1. This can be done with your basic one pass link manipulation. You just need
   to think hard about what link references you need to track.

## Solutions

### Iterate and swap

This solution iterates through the list and changes links as appropriate.
Nothing fancy here, just link management.

This time complexity of this solution is O(n) in making a pass through the
linked list. The space complexity is O(1), as only a few reference variables
must be kept regardless of the size of the list.

```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null) {
            ListNode next = curr.next;
            if (next != null) {
                curr.next = next.next;
                next.next = curr;
                if (prev == null) {
                    head = next;
                } else {
                    prev.next = next;
                }
            }
            prev = curr;
            curr = curr.next;
        }
        return head;
    }
}
```
