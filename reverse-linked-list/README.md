# Reverse Linked List

Reverse a singly linked list.

## Hints

## Solutions

### Recursive

The recursive solution recurses on previous and current nodes. It starts with
`prev = null` and `curr = head`. At each invocation it points the current node
at the previous node. Then it checks `curr.next`. If this is null, we are at
the end of the list and we can return the new head. If this is not null, we
continue to recurse.

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null) {
            return head;
        }
        return rl(null, head);
    }

    private ListNode rl(ListNode prev, ListNode curr) {
        curr.next = prev;
        ListNode temp = curr.next;
        return temp == null ? curr : rl(curr, temp);
    }
}
```

### Iterative

The iterative version proceeds much like the recursive version. Pointers to
previous and current nodes are set with `prev = null` and `curr = head`. In
each iteration it first checked if current has a next. If not, the loop
terminates. Within the loop current is pointed back at previous and the new
previous and current nodes are set. When the loop terminates the final pointing
of current back to previous is made and the final current is returned.

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null) {
            return head;
        }
        ListNode prev = null;
        ListNode curr = head;
        while (curr.next != null) {
            ListNode temp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = temp;
        }
        curr.next = prev;
        return curr;
    }
}
```
