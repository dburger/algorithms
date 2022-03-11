# Remove Linked List Elements

Given the `head` of a linked list and an integer `val`, remove all the nodes
of the linked list that have `Node.val == val`, and return the new head.

## Hints

1. The standard singly linked list iteration approach should suffice.

## Solutions

### Oh so standard

A very commonly used approach to solve these singly linked list problems is
the usage of `prev` and `curr` pointers. That approach works here. The approach
proceeds by keeping the trailing `prev` pointer and bypassing nodes with
value `val` as they are encountered. The special case of these elements being
at the front of the list is identified by `prev` not having been set yet.

The time complexity of this solution is O(n) where n is the length of the
linked list. The space complexity is O(1) as only a constant amount of space
is used to process the list regardless of size.

```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        ListNode prev = null;
	ListNode curr = head;
	while (curr != null) {
	    if (curr.val == val) {
	        if (prev == null) {
		    curr = head = curr.next;
		} else {
		    curr = curr.next;
		    prev.next = curr;
		}
	    } else {
	        prev = curr;
		curr = curr.next;
	    }
	}
    }
}
```
