# Rotate List

## Hints

1. Brute force. Write a function that rotates the tail to become the head.
   Then repeat that `k` times in a loop.
1. If we rotate a list its entire length, the list is back to the original.
   Instead of doing all the rotations, eliminate all the rotations that just
   you back to original. Only do the difference making rotations. For a bonus,
   switch the method to merely finding the new head instead of doing
   "rotations" at all. This solution is O(n), both from counting the list and
   an upper bounded n tail to head swap outs.

## Solutions

### Brute force, with some optimization

This solution uses the brute force approach of making the tail the new
head repeatedly. It optimizes by only doing this k % i times, since
anything i times and over involves rotations that merely put the list
back to its original state.

```java
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || head.next == null || k == 0) {
            return head;
        }
        // Find how long the list is and store in i.
        int i = 1;
        ListNode curr = head;
        while (curr.next != null) {
            curr = curr.next;
            i++;
        }
        // Now anything over i is just a repeated rotation, thus
        // k can be reduced to the modulus.
        k = k % i;
        // k times, find the tail node and make it the new head.
        while (k-- > 0) {
            ListNode prev = null;
            ListNode tail = head;
            while (tail.next != null) {
                prev = tail;
                tail = tail.next;
            }
            prev.next = null;
            tail.next = head;
            head = tail;
        }
        return head;
    }
}
```

### Modulus mania and identify the new head.

This approach determines how many rotations need to be performed using
the modulus approach from above and then directly identifies and swaps
the new head. That is no repeated swapping of the tail. This solution
is also O(n) from counting the list. In practice it should be slightly
faster than the above solution as it avoids the repeated tail head swap.

```java
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || head.next == null || k == 0) {
            return head;
        }

        // Get the size of the list.
        int i = 1;
        ListNode curr = head;
        while (curr.next != null) {
            curr = curr.next;
            i++;
        }

        // Now determine how many rotations are needed.
        k = k % i;
        if (k == 0) {
            // No rotations required.
            return head;
        }
        // And how far this is from the current head.
        k = i - k;
        // Now walk down to the head and keep a previous pointer.

        ListNode prev = null;
        curr = head;
        while (k-- > 0) {
            prev = curr;
            curr = curr.next;
        }

        // Now locate the tail.
        ListNode tail = head;
        while (tail.next != null) {
            tail = tail.next;
        }

        // Now finally swap everything out.
        prev.next = null;
        tail.next = head;
        head = curr;

        return head;
    }
}
```
