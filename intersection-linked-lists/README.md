# Intersection of Two Linked Lists

Write a program to find the node at which the intersection of two singly linked
lists begins.

**Notes:**

* If the two linked lists have no intersection at all, return `null`.
* The linked lists must retain their original structure after the function
  returns.
* You may assume there are no cycles anywhere in the entire linked structure.
* You code should preferably run in O(n) time and use only O(1) memory.

## Hints

1. Brute force could use a set to check when the first duplicate node is found.

## Solutions

### Brute force

This is the obvious brute force approach using a `Set`. Space requirements here
are O(n) where n is the length of the list with head `headA`. Time complexity
is O(m) where m is the longer of the list with head `headA` or head `headB`.

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        Set<ListNode> seen = new HashSet<>();
        ListNode curr = headA;
        while (curr != null) {
            seen.add(curr);
            curr = curr.next;
        }
        curr = headB;
        while (curr != null) {
            if (seen.contains(curr)) {
                return curr;
            }
            curr = curr.next;
        }
        return null;
    }
}
```
