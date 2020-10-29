# Add Two Numbers

You are given two **non-empty** linked lists representing two non-negative
integers. The digits are stored in **reverse order**, and each of their nodes
contains a single digit. Add the two numbers and return the sum as a linked
list.

You may assume the two numbers do not contain any leading zero, except the
number 9 itself.

## Hints

You can do this with the typical algorithm for adding that kids use, adding
and keeping track of a carry between digits. Since the linked lists are in
reverse digit order this works by merely walking the lists in order and building
up the result in order.

## Solutions

### Walk and carry

The solution merely walks the digits, adding, and keeping track of a
carry for the next digits.

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        int carry = 0;
        ListNode head = null;
        ListNode curr = null;
        while (l1 != null || l2 != null) {
            int l1Int = 0;
            if (l1 != null) {
                l1Int = l1.val;
                l1 = l1.next;
            }
            int l2Int = 0;
            if (l2 != null) {
                l2Int = l2.val;
                l2 = l2.next;
            }
            int sum = l1Int + l2Int + carry;
            int digit = sum % 10;
            carry = sum / 10;
            ListNode temp = new ListNode(digit);
            if (head == null) {
                head = curr = temp;
            } else {
                curr.next = temp;
                curr = temp;
            }
        }
        if (carry > 0) {
            curr.next = new ListNode(carry);
        }
        return head;
    }
}
```
