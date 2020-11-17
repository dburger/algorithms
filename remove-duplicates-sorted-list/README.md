# Remove Duplicates from Sorted List

Given a sorted linked list, delete all duplicates such that each element
appears only once.

## Hints

1. This one should succumb to the trailing pointer approach.

## Solutions

### Trailing pointer

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null) {
            if (prev != null && curr.val == prev.val) {
                prev.next = curr.next;
                curr = curr.next;
            } else {
                prev = curr;
                curr = curr.next;
            }
        }
        return head;
    }
}
```
