# Palindrome Linked List

## Hints

1. The natural way to do this is to put the first half of the list on a stack,
   and then when iterating over the second half of the list pop the stack and
   make comparisons.

## Solutions

### Stack attack

Here we use the stack approach. First the length of the list is determined.
Then a loop puts the first half of the list on the stack. Iterating over the
second half of the list pops from the stack and compares. If the list has an
odd length, the middle element is ignored for comparison as it must only equal
itself.

```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        int length = 0;
        ListNode curr = head;
        while (curr != null) {
            length++;
            curr = curr.next;
        }
        Stack<ListNode> s = new Stack<>();
        curr = head;
        int mid = length / 2;
        int i = 0;
        while (curr != null) {
            if (i < mid) {
                s.push(curr);
            } else if (i == mid && length % 2 == 1) {
                // Ignore middle node of odd length list.
            } else {
                if (curr.val != s.pop().val) {
                    return false;
                }
            }
            curr = curr.next;
            i++;
        }
        return true;
    }
}
```
