# Reverse Nodes in k Group

Given a linked list, reverse the nodes of the list `k` at a time and return the
modified list.

`k` is a positive integer and is less than or equal to the length of the linked
list. If the number of nodes is not a multiple of `k` then left out nodes, at
the end, should remain as is (not reversed).

**Follow up:**
* Could you solve this problem in `O(1)` extra memory space?
* You may not alter the values in the list's nodes, only nodes itself may be
  changed.

## Hints

## Solutions

### Split and append

This approach strives for simplicity. A `split` method is introduced to split
off nodes in groups of size `k`. These are then reversed and stored in a list.
Then these lists are all appened together to produce the final result.

This solution is O(n) time and space complexity.

```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        List<ListNode> segments = new ArrayList<>();
        while (head != null) {
            ListNode tail = split(head, k);
            if (size(head) == k) {
                head = reverse(head);
            }
            segments.add(head);
            head = tail;
        }
        head = segments.get(0);
        for (int i = 1; i < segments.size(); i++) {
            append(head, segments.get(i));
        }
        return head;
    }

    /**
     * Splits nodes off the list starting with {@code head} and returns the tail.
     * {@code head} will have {@code k} nodes or as many were left in the list if
     * the list does not have {@code k} nodes.
     */
    private ListNode split(ListNode head, int k) {
        if (head == null) {
            return null;
        }
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null && k > 0) {
            prev = curr;
            curr = curr.next;
            k--;
        }
        if (prev != null) {
            prev.next = null;
        }
        return curr;
    }

    private int size(ListNode head) {
        int size = 0;
        while (head != null) {
            head = head.next;
            size++;
        }
        return size;
    }

    private ListNode reverse(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }

    private void append(ListNode l1, ListNode l2) {
        ListNode prev = null;
        ListNode curr = l1;
        while (curr != null) {
            prev = curr;
            curr = curr.next;
        }
        prev.next = l2;
    }
}
```

### Recursive madness

This solution uses recursion in an elegant manner. It works by reversing a
segment at a time and then pointing the tail of a given segment at the result
of the reversal invocation on the next segment.

This solution has O(n) time and space complexity. The space complexity comes
from the recursive calls.

```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        if (head == null || k == 1) {
            return head;
        }

        // Reverse a segment.
        int count = 0;
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null && count < k) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
            count++;
        }

        // If it was shorter than k, we didn't need to reverse it,
        // so we undo it.
        if (count < k) {
            return reverseKGroup(prev, count);
        }

        // Point the next tail at the result of reversing the next segment.
        head.next = reverseKGroup(curr, k);
        // Return the new head.
        return prev;
    }
}
```
