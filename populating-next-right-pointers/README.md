# Populating Next Right Pointers in Each Node

You are given a **perfect binary tree** where all leaves are on the same level,
and every parent has two children. The binary tree has the following
definition:

```java
struct Node {
  int val;
  Node* left;
  Node* right;
  Node* next;
}
```

Populate each next pointer to point to its next right node. If there is no next
right node, the next pointer should be set to `NULL`.

Initially, all next pointers are set to `NULL`.

## Hints

1. BFS by level traversal makes this all too easy.

## Solutions

### BFS is my best friend

This solution uses a by level BFS traversal and sets the `next` pointers on
a per level basis. This is the obvious "easy" approach if you are familar with
the by level BFS traversal of a tree.

The time complexity for this algorithm is O(n) where n is the size of the tree.
This comes from visiting each node in the tree once. The space complexity is
O(2^n) where n is the depth of the tree. This comes from this being a "perfect"
binary tree and the largest the queue will need to be is to hold the bottom
level of the tree.

```java
class Solution {
    public Node connect(Node root) {
        if (root == null) {
            return null;
        }
        Deque<Node> d = new LinkedList<>();
        d.addFirst(root);
        while (!d.isEmpty()) {
            int size = d.size();
            Node prev = null;
            while (size-- > 0) {
                Node next = d.removeFirst();
                if (prev != null) {
                    prev.next = next;
                }
                prev = next;
                if (next.left != null) {
                    d.addLast(next.left);
                }
                if (next.right != null) {
                    d.addLast(next.right);
                }
            }
        }
        return root;
    }
}
```
