# Merge k Sorted Lists

You are given an array of `k` linked-lists `lists`, each linked-list is sorted
in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

## Hints

1. This is just the typical merge of `2` sorted lists take to `k` sorted lists.
   A very similar approach applies. I don't think this should be classified as
   a leeter "hard" problem.

## Solutions

### Typical merge procedure

This solution follows the typical merging procedure. That is, we look for the
ListNode with the lowest value and store the index to that item in the list.
At the end of the loop we put that node on our merged list, and advance the
source list position to its next element. In subsequent loopings when we see
a null we know that list is already exhuasted and just hop to the next list.
Eventually all lists are exhausted and the sorted merged list is complete.

This solution does not fair well in the leeter speed ranking. The time
complexity here is O(m * n) where m is the number of lists and n is the length
of the longest list. The space complexity is O(1) as we allocate only constant
space in processing of the lists and reuse the source ListNodes themselves to
formulate the merged list.

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        ListNode head = null;
        ListNode curr = null;
        for (;;) {
            int minVal = Integer.MAX_VALUE;
            int minIdx = -1;
            for (int i = 0; i < lists.length; i++) {
                ListNode ln = lists[i];
                if (ln != null && ln.val < minVal) {
                    minVal = ln.val;
                    minIdx = i;
                }
            }
            if (minIdx == -1) {
                break;
            }
            ListNode ln = lists[minIdx];
            if (head == null) {
                head = curr = ln;
            } else {
                curr.next = ln;
                curr = ln;
            }
            lists[minIdx] = ln.next;
        }
        return head;
    }
}
```
