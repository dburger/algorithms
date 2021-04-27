# Remove Nth Node From End of List

Given the `head` of a linked list, remove the `nth` node from the end of the
list and return its head.

**Follow up:** Could you do this in one pass?

**Note:** This problem does not ask you to program for special cases such as
the list being shorter than `n`. This eliminates the need to put special edge
case code around the list iteration as all test cases are in the happy path.

## Hints

1. The easy two pass solution uses the first pass to count the length of the
   list, and the second pass to remove the correct node.
2. To honor the follow up, think of using two node pointers, one for a
   trailing node.

## Solutions

### Trailing node

This solution uses two primary pointers, `curr` to get to find the end of the
list and `trailer` to trail `n` hops behind.

The time complexity of this solution is O(n), that is we traverse to the end
of the list. The space complexity is O(1) as we only spawn up a few reference
variables regardless of the size of the list.

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode curr = head;
        while (n > 0) {
            curr = curr.next;
            n--;
        }
        ListNode prev = null;
        ListNode trailer = head;
        while (curr != null) {
            curr = curr.next;
            prev = trailer;
            trailer = trailer.next;
        }
        if (prev == null) {
            // Edge case, head was then nth from end.
            return trailer.next;
        }
        prev.next = trailer.next;
        return head;
    }
}
```
