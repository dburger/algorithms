# Copy List with Random Pointer

A linked list of length `n` is given such that each node contains an additional
random pointer, which could point to any node in the list, or `null`.

Construct a deep copy of the list. The deep copy should consist of exactly `n`
brand new nodes, where each node has its value set to the value of its
corresponding original node. Both the `next` and `random` pointer of the new
nodes should point to the new nodes in the copied list such that the pointers
in the original list and copied list represent the same list state. None of the
pointers in the new list should point to nodes in the original list.

For example, if there are two nodes `X` and `Y` in the original list, where
`X.random --> Y`, then for the corresponding two nodes `x` and `y` in the
copied list, `x.random --> y`.

Return the head of the copied linked list.

## Hints

1. The main concern here is how to keep track of new nodes? Especially ones
   from the random pointers that haven't been realized in the primary linked
   list yet? It is actually quite simple. You can map them from the original
   nodes.

## Solutions

### Map and two pass

This solution is actually very simple. We keep a map from original node to
new node through an original pass through the list, building the new list at
the same time. Then we do another pass through the original list to fix up
the random pointers in the copied list.

The time and space complexity of this solution is O(n). The time complexity
comes from the two passes through the list. The space complexity is required
to copy the list.

```java
class Solution {
    public Node copyRandomList(Node head) {
        Map<Node, Node> m = new HashMap<>();
        Node prev = null;
        Node curr = head;
        while (curr != null) {
            Node n = new Node(curr.val);
            m.put(curr, n);
            if (prev != null) {
                m.get(prev).next = n;
            }
            prev = curr;
            curr = curr.next;
        }

        curr = head;
        while (curr != null) {
            if (curr.random != null) {
                m.get(curr).random = m.get(curr.random);
            }
            curr = curr.next;
        }

        return m.get(head);
    }
}
```
