# Insertion Sort List

Sort a linked list using insertion sort.

**Algorithm of Insertion Sort**:

1. Insertion sort iterates, consuming one input element each repetition, and
   growing a sorted output list.
1. At each iteration, insertion sort removes one element from the input data,
   finds the location it belongs within the sorted list, and inserts it there.
1. It repeates until no input elements remain.

## Hints

1. No magic here.

## Solutions

### Insertion sort

This solution builds up a new sorted list from scratch. It is O(n^2).

```java
class Solution {
    public ListNode insertionSortList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        // Build up the sorted list on sorted head pointer.
        ListNode sorted = head;
        head = head.next;
        sorted.next = null;
        // Exhaust the input list.
        while (head != null) {
            ListNode prev = null;
            ListNode curr = sorted;
            // Find the insert position.
            while (curr != null && curr.val > head.val) {
                prev = curr;
                curr = curr.next;
            }
            if (prev == null) {
                // Insertion at the front.
                ListNode temp = head.next;
                head.next = sorted;
                sorted = head;
                head = temp;
            } else if (curr == null) {
                // Insertion at the end.
                prev.next = head;
                ListNode temp = head.next;
                head.next = null;
                head = temp;
            } else {
                // Insertion in the middle.
                ListNode temp = head.next;
                prev.next = head;
                head.next = curr;
                head = temp;
            }
        }
        return sorted;
    }
}
```
