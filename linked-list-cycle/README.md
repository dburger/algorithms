# Linked List Cycle

Given `head`, the head of a linked list, determine if the linked list has a
cycle in it.

**Follow up:** Can you solve it using `O(1)` memory?

## Hints

1. This is one of those classic interview questions that is probably rarely
   asked in practice because of the proliferation of interview prep books and
   sites like leeter.
1. The follow up is actually the question most folks would want to see you
   answer.

## Solutions

One easy solution here would be to maintain a set of visited nodes. While
iterating through the list you would check visited for the presence of the
encountered node. If it is already there you return false. If it is not there
you continue iterating. If you hit the end of the list without finding such
a repeat you return true. This solution is trivial so I will not add it here.

### Fast and slow pointers (Floyd the Barber)

This is the classic fast and slow pointer approach. In it, you iterate down the
list with two pointers. One pointer you one hop on each round while the other
you move two hops on each round. If the faster pointer catches up with the
slower pointer you have a cycle. If it does not, and you reach the end of the
list, no cycle. This is known as Floyd's Cycle-Finding Algorithm.

This solution has O(n) time complexity in that it must traverse the list. The
space complexity is O(1) as only two pointers are needed regardless of the size
of the list.

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (fast == slow) {
                return true;
            }
        }
        return false;
    }
}
```
