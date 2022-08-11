# Populating Next Right Pointers in Each Node II

Given a binary tree:

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

Populate each next pointer to point to its next right node. If there is no next
right node, the next pointer should be set to `null`.

Initially, all next pointers are set to `null`.

## Hints

1. A specific traversal gets you 99% of the way there.

## Solutions

### BFS for the win

Understanding breadth first tree traversal and being able to implement it with
a good implementation strategy makes this problem a breeze. In particular,
solving this problem is greatly benefited by the nested loop `size` snapshotting
technique. Using this technique, the problem of getting the right edge nodes
to remain set to `null` does not become an issue.

The following implementation has O(n) time and space complexity where `n` is
the number of nodes in the tree. For the time complexity this comes from
needing to visit every node of the tree. For the space complexity this comes
from the queue. In practice this queue will have a maximum size of the largest
layer in the tree which is certainly bound by `n`, the number of nodes in the
tree.

```java
class Solution {
    public Node connect(Node root) {
        if (root == null) {
            return null;
        }

        Queue<Node> q = new LinkedList<>();
        q.add(root);
        while (!q.isEmpty()) {
            int size = q.size();
            Node prev = null;
            while (size-- > 0) {
                Node node = q.remove();
                if (prev != null) {
                    prev.next = node;
                }
                if (node.left != null) {
                    q.add(node.left);
                }
                if (node.right != null) {
                    q.add(node.right);
                }
                prev = node;
            }
        }
        return root;
    }
}
```
