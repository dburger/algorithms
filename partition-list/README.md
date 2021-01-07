# Partition List

Given the `head` of a linked list and a value `x`, partition it such that all
nodes **less than** `x` come before nodes **greater than or equal** to `x`.

You should `preserve` the original relative order of the nodes in each of the
two partitions.

## Hints

1. The most obvious way to approach this is to build two new lists while you
   iterate the given list. For the return value you attach them together as
   appropriate.

## Solutions

### Build up separate lists and join

Following the hint, this solution is O(n) time and space complexity, where n
is the length of the provided list.

```java
class Solution {
    public ListNode partition(ListNode head, int x) {
        ListNode lessHead = null;
        ListNode lessTail = null;
        ListNode greaterHead = null;
        ListNode greaterTail = null;

        while (head != null) {
            if (head.val < x) {
                if (lessHead == null) {
                    lessHead = lessTail = head;
                } else {
                    lessTail.next = head;
                    lessTail = head;
                }
            } else {
                if (greaterHead == null) {
                    greaterHead = greaterTail = head;
                } else {
                    greaterTail.next = head;
                    greaterTail = head;
                }
            }
            head = head.next;
        }

        if (lessHead == null && greaterHead == null) {
            return null;
        } else if (lessHead == null) {
            greaterTail.next = null;
            return greaterHead;
        } else if (greaterHead == null) {
            lessTail.next = null;
            return lessHead;
        } else {
            lessTail.next = greaterHead;
            greaterTail.next = null;
            return lessHead;
        }
    }
}
```
