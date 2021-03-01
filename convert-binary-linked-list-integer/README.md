# Convert Binary Number in a Linked List to Integer

Given `head` which is a reference to a singly-linked list. The value of
each node in the list is `0` or `1`. The linked list holds a binary
representation of a number.

Return the decimal value of the number in the linked list.

## Hints

1. If you knew the length of the list then you would know the value of
   each node on a second pass.

## Solutions

### Length with second pass

Here we follow the hint above. The length is determined in a first pass.
In a second pass the decimal value is accumulated.

Time time complexity of this solution is O(n) due to iterating the list.
The space complexity is O(1).

```java
class Solution {
    public int getDecimalValue(ListNode head) {
        int len = 0;
        ListNode curr = head;
        while (curr != null) {
            len++;
            curr = curr.next;
        }

        int result = 0;
        curr = head;
        while (curr != null) {
            result += curr.val * Math.pow(2, --len);
            curr = curr.next;
        }
        return result;
    }
}
```
