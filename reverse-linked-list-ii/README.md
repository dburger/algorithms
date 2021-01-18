# Reverse Linked List II

Reverse a linked list from position `m` to `n`. Do it in one-pass.

## Hints

1. The kicker here is that they say to do it in "one-pass." The pointer handling
   does get confusing. I will do this for you two ways below. A much simpler
   multi-pass approach and then their "one-pass" approach.

## Solutions

### Multi-pass but easy

Here we use a multi-pass approach but we do it with separate and very simple
functions and piece back together the result. This is done by introducing
`split`, `reverse`, and `append` functions. `split` splits off a tail list.
`reverse` reverses a linked list. `append` appends two linked lists together.
Combined, these functions make for a pretty straight forward implementation
of this algorithm.

This big O time and complexity of this solution is O(n). The space complexity
is O(1). Even while this solution is "multi-pass", it clocks in with a 0
millisecond leeter time.

The nice thing about this approach in addition to it being fairly easy to
implement and understand is that it breaks out some core methods that can be
used to solve other linked list problems.

```java
class Solution {
    public ListNode reverseBetween(ListNode head, int m, int n) {
        ListNode tail = split(head, n + 1);
        ListNode mid = split(head, m);
        if (head == mid) {
            mid = reverse(mid);
            head = mid;
        } else {
            mid = reverse(mid);
            append(head, mid);
        }
        append(head, tail);
        return head;
    }

    /**
     * Splits off and returns a tail off {@code n}. The tail will begin at
     * 1 based position k. If {@code n} is not long enough to support that,
     * null will be return and the list from {@code n} will not be modified.
     */
    private ListNode split(ListNode n, int k) {
        ListNode prev = null;
        ListNode curr = n;
        while (curr != null && k > 1) {
            prev = curr;
            curr = curr.next;
            k--;
        }
        if (prev != null) {
            prev.next = null;
        }
        return curr;
    }

    private ListNode reverse(ListNode n) {
        ListNode prev = null;
        ListNode curr = n;
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

### One-pass, oh my which pointer points where?

This solution works in one-pass as requested by the problem statement. I have
added comments in place to explain all the pointer handling which can be hard to
explain from afar. The primary edge case to consider is when `m == 1` and
therefore the leading segment is empty. The edge case of an empty trailing
segment is handled naturally as it assigns a null pointer to the last node of
the reversed segment.

This solution has O(n) time complexity on O(1) space complexity.

```java
class Solution {
    public ListNode reverseBetween(ListNode head, int m, int n) {
        // This first pass walks past the part of the list that will
        // remain unchanged. After this part, prev1 will point at the
        // last node of this segement, if m == 1, prev1 will be null.
        ListNode prev1 = null;
        ListNode curr1 = head;
        while (m > 1) {
            prev1 = curr1;
            curr1 = curr1.next;
            m--;
            n--;
        }

        // Now we reverse the middle segment. After the reversal,
        // prev2 will be the first node of this segement, and curr2
        // will be the first node of the last segment or null if there
        // is no trailing segment.
        ListNode prev2 = null;
        ListNode curr2 = curr1;
        while (n > 0) {
            ListNode next = curr2.next;
            curr2.next = prev2;
            prev2 = curr2;
            curr2 = next;
            n--;
        }

        // Attach the first segment to the middle reversed segment, if
        // there was a first segment (m > 1).
        if (prev1 != null) {
            prev1.next = prev2;
        }
        // Attach the middle reversed segment to the tail.
        curr1.next = curr2;
        // If there was no first segment (m == 1), return the middle
        // segment, otherwise return the first segment via head.
        return prev1 == null ? prev2 : head;
    }
}
```
