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

### List length

This approach works by determining the length of each list, and then using that
information to walk out to the common node. Note the need to check if the last
node is the same for each list. If they are not, then these lists don't
intersect.

The space complexity for this is O(1). The time complexity is O(n) where n is
the length of the longer list. This comes from iterating the lists a few times
to gather the lengths and then to identify the first shared node.

```java
public class Solution {
    private class ListLength {
        int length;
        ListNode lastNode;
        ListLength(int length, ListNode lastNode) {
            this.length = length;
            this.lastNode = lastNode;
        }
    }

    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListLength llA = length(headA);
        ListLength llB = length(headB);
        // If the last node is not the same, these lists do not intersect.
        if (llA.lastNode != llB.lastNode) {
            return null;
        }

        // Make sure llA is the shorter one, adjusting references as necessary.
        if (llB.length < llA.length) {
            ListLength llTemp = llB;
            llB = llA;
            llA = llTemp;
            ListNode lnTemp = headB;
            headB = headA;
            headA = lnTemp;
        }

        // Now walk out until the same distances are left in each list.
        ListNode currA = headA;
        ListNode currB = headB;
        while (llB.length > llA.length) {
            currB = currB.next;
            llB.length--;
        }

        // Now walk until we find the common node.
        while (currA != currB) {
            currA = currA.next;
            currB = currB.next;
        }
        return currA;
    }

    private ListLength length(ListNode node) {
        ListNode prev = null;
        int length = 0;
        while (node != null) {
            prev = node;
            node = node.next;
            length++;
        }
        return new ListLength(length, prev);
    }
}
```

### Head swapper

This is a clever as hell approach which is similar to the list length solution
above - but no explicit counting of the length need take place. In this approach
the lists are iterated until the hit upon the same first node. How can you do
that? Well generally if you iterate to the tail of a list you can make sure that
both lists have the same tail. If they don't, then obviously they don't
intersect. Now on a second iteration the pointers are flipped so that `currA`
is set to iterate list B. Likewise `currB` is set to iterate list A. Because of
this flipping of the list pointers, the pointer that started on the shorter list
has been given the exact head start needed when iterating the longer list. This
is equivalent to the "walking out the same distance" part of the above
algorithm. This means that they will in fact converge on the correct node.

This solution has O(n) time complexity where n is the length of the longer list.
The space complexity is O(1).

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if (headA == null || headB == null) {
            return null;
        }
        ListNode lastA = null;
        ListNode lastB = null;
        ListNode currA = headA;
        ListNode currB = headB;
        while (currA != currB) {
            ListNode prev = currA;
            currA = currA.next;
            if (currA == null) {
                lastA = prev;
                if (lastB != null && lastA != lastB) {
                    return null;
                }
                currA = headB;
            }

            prev = currB;
            currB = currB.next;
            if (currB == null) {
                lastB = prev;
                if (lastA != null && lastB != lastA) {
                    return null;
                }
                currB = headA;
            }
        }
        return currA;
    }
}
```
