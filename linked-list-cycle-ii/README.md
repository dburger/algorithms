# Linked List Cycle II

Given the `head` of a linked list, return the node where the cycle begins. If
there is no cycle, return `null`. There is a cycle in a linked list if there is
some node in the list that can be reached again by continously following the
`next` pointer. Internally, `pos` is used to denote the index of the node that
tail's  `next` pointer is connected to (**0-indexed**). It is -1 if there is no
cycle. **Note that** `pos` **is not passed as a parameter**.

**Do not modify** the linked list.

**Follow up**: Can use solve it using O(1) memory?

## Hints

1. Tracking visited makes this problem too easy.
1. Do you know the cycle trick?

## Solutions

This is a strange problem, because while it is listed as "medium", there is
a trivial solution. The constraints do not make that solution approach invalid.
The follow up does require the harder approach.

### Did you have any visitors?

As noted in the hints this problem is trivially solved with a `Set` tracking
visited nodes.

The time complexity of this solution is O(n) where `n` is the length of the
list. The space complexity is also O(n) to have the visited set.

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        Set<ListNode> visited = new HashSet<>();
        while (head != null) {
            if (!visited.add(head)) {
                return head;
            }
            head = head.next;
        }
        return null;
    }
}
```

### Use the force

If we use the classic linked list cycle detection algorithm, it just so happens
that the node at which the cycle is detected can be used to solve this problem.
By starting another pointer over at head and walking both pointers forward by
equal amounts they will meet at the node where the cycle begins.

The time complexity of this algorithm is O(n) where `n` is the length of the
list. The space complexity is O(1).

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if (head == null) {
            return null;
        }
        ListNode slow = head;
        ListNode fast = head;

        while (slow != null && fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (fast == slow) {
                break;
            }
        }

        if (fast == null || fast.next == null) {
            return null;
        }

        fast = head;
        while (fast != slow) {
            slow = slow.next;
            fast = fast.next;
        }

        return fast;
    }
}
```
