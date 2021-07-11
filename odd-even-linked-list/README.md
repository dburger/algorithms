# Odd Even Linked List

Given the `head` of a singly linked list, group all the nodes with odd indices
together followed by the nodes with even indices, and return the reordered
list.

The first node is at an odd position, the second at even, and so on.

Note that the relative order inside both the even and odd groups should remain
as it was in the input.

You must solve the problem in O(1) extra space complexity and O(n) time
complexity.

## Hints

1. Just a bunch of list management right? Build on to new heads keeping track
   of new currents and then put them together?

## Solutions

### Split and join

Here we iterate through the list keeping track if we are at an odd or even
position, and link up the node accordingly. In the end we piece the lists
back together.

The time complexity of this solution is O(n) to do the iteration through the
original list. The space complexity is O(1).

```java
class Solution {
    public ListNode oddEvenList(ListNode head) {
        if (head == null) {
            return null;
        }

        ListNode oddHead = null;
        ListNode oddCurr = null;
        ListNode evenHead = null;
        ListNode evenCurr = null;

        boolean odd = true;
        ListNode curr = head;

        while (curr != null) {
            if (odd) {
                if (oddHead == null) {
                    oddHead = oddCurr = curr;
                } else {
                    oddCurr.next = curr;
                    oddCurr = curr;
                }
            } else {
                if (evenHead == null) {
                    evenHead = evenCurr = curr;
                } else {
                    evenCurr.next = curr;
                    evenCurr = curr;
                }
            }
            odd = !odd;
            curr = curr.next;
        }

        oddCurr.next = evenHead;
        if (evenCurr != null) {
            evenCurr.next = null;
        }

        return oddHead;
    }
}
```
