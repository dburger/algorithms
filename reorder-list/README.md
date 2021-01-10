# Reorder List

Given a singly linked list L: `l0 -> l1 -> ... -> ln`,
reorder it to: `l0 -> ln -> l1 -> l(n-1) ...`.

You may **not** modify the values in the list's nodes, only the nodes
themselves may be changed.

## Hints

1. You could store the linked list in another data structure so that you can
   positionally access the items towards the tail. Then walking the front of
   the list you can get the items from the tail and piece this new order
   together.
2. Split and reverse. Split the list into two pieces. Reverse the second half
   and then merge the lists back together.

## Solutions

### Store and access in a list

This solution uses the store and access approach. The storage is managed in a
list. This gives positional access to the tail of the linked list that can
be used to piece the desired order back together.

This solution has time and space complexity O(n).

```java
class Solution {
    public void reorderList(ListNode head) {
        if (head == null) return;
        List<ListNode> l = new ArrayList<>();
        ListNode curr = head;
        while (curr != null) {
            l.add(curr);
            curr = curr.next;
        }

        curr = head;
        int mid = l.size() / 2;
        int last = l.size() - 1;
        for (int i = 0; i < mid; i++) {
            ListNode next = curr.next;
            ListNode end = l.get(last - i);
            curr.next = end;
            end.next = next;
            curr = next;
        }
        curr.next = null;
    }
}
```

### Store and access in a stack

Similar to above, but by using the storage of a `Stack` the pulling of the end
node is just a simple `s.pop()` rather than an offset off the end.

The same O(n) time and space complexity apply.

```java
class Solution {
    public void reorderList(ListNode head) {
        if (head == null) return;
        Stack<ListNode> s = new Stack<>();
        ListNode curr = head;
        while (curr != null) {
            s.push(curr);
            curr = curr.next;
        }

        curr = head;
        int mid = s.size() / 2;
        for (int i = 0; i < mid; i++) {
            ListNode next = curr.next;
            ListNode end = s.pop();
            curr.next = end;
            end.next = next;
            curr = next;
        }
        curr.next = null;
    }
}
```

### Split, reverse, and merge

Here we follow down the path of the "split and reverse" hint from above. First
we split the list into two pieces, a first and second half. If there is an odd
number of elements the extra element belongs in the first half per the
reordering spec. Then, the second half list is reversed. Finally these lists
are merged back together. It may look more complicated than the above solutions
due to the amount of code but actually each step is fairly simple.

This solution is the same O(n) time complexity but has an improved O(1) space
complexity. This solution is faster in practice than the above solutions. This
is probably due to not having to allocate as many objects per the O(1) space
complexity noted here.

```java
class Solution {
    public void reorderList(ListNode head) {
        if (head == null) return;
        ListNode l2 = split(head);
        l2 = reverse(l2);
        merge(head, l2);
    }

    private ListNode split(ListNode head) {
        int length = 0;
        ListNode curr = head;
        while (curr != null) {
            curr = curr.next;
            length++;
        }

        ListNode prev = null;
        curr = head;
        int mid = length / 2;
        if (length % 2 == 1) mid++;
        for (int i = 0; i < mid; i++) {
            prev = curr;
            curr = curr.next;
        }

        prev.next = null;
        return curr;
    }

    private ListNode reverse(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }

    private void merge(ListNode l1, ListNode l2) {
        ListNode curr1 = l1;
        ListNode curr2 = l2;
        while (curr2 != null) {
            ListNode curr1Next = curr1.next;
            ListNode curr2Next = curr2.next;
            curr1.next = curr2;
            curr2.next = curr1Next;
            curr1 = curr1Next;
            curr2 = curr2Next;
        }
    }
}
```
