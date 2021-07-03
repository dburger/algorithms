# Remove Duplicates from Sorted List II

Given the `head` of a sorted linked list, delete all nodes that have duplicate
numbers, leaving only distinct numbers from the original list. Return the list
sorted as well.

## Hints

1. Identify the repeated spans with start and end references, and then skip
   them.

## Solutions

### Span and skip

This solution works by pointing `start` and `end` at a node and then advancing
`end` until the value does not match `start`. At this point if `start.next`
is equal to `end`, then `start.val` has no duplicates. If `start.next` is not
equal to `end` then there are duplicates. In the case of no duplicates `prior`
is set to `start`. This allows for possible link manipulation of `prior.next`
if it needs to be used to skip a duplicate span. In the case of duplicates we
have two cases. If `prior` is still null we haven't seen a non-duplicate span
yet. In this case head can just be moved up to `end`. If `prior` is not null
then we set `prior.next` to `end` to skip the span.

The time complexity of this algorithm is O(n). That is we must traverse the
entire linked list. The space complexity of this solution is O(1).

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        ListNode prior = null;
        ListNode start = head;
        ListNode end = head;
        while (end != null) {
            while (end != null && end.val == start.val) {
                end = end.next;
            }
            if (start.next == end) {
                prior = start;
            } else {
                if (prior == null) {
                    head = end;
                } else {
                    prior.next = end;
                }
            }
            start = end;
        }
        return head;
    }
}
```
