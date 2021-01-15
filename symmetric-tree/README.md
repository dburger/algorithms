# Symmetric Tree

Given a binary tree, check whether it is a mirror of itself (ie symmetric around
its center).

## Hints

1. Symmetry would imply that each level of the tree reads the same backwards and
   forwards. How could you build a solution around this?

## Solutions

### BFS with level check

This solution follows the hint above. In it, a BFS traversal coordinated via a
queue is performed. A `NodeWrapper` class is used for the objects in the queue
so that the `TreeNode` along with its `depth` can be tracked. When nodes are
pulled from the queue there are added to a list. When the depth changes, that
is the cue to check for the symmetry of the level held in the list. If this
check fails, `false` is returned. If it passes, the list is cleared in
prepration for the next level check.

This solution has O(n) time commplexity as each node is handled 2n times. Once
when processed from the queue and once when compared within the list.

This is a lot of code although a fair chunk of it is dedicated to the
`NodeWrapper` class and its `equals` method. Other than that there are some
tricky details to get right in the list handling. Note that null nodes at a
level that is populated elsewhere must be represented in the traversal so
that symmetry can be check.

```java
class Solution {
    private class NodeWrapper {
        TreeNode node;
        int depth;
        NodeWrapper(TreeNode node, int depth) {
            this.node = node;
            this.depth = depth;
        }

        @Override
        public boolean equals(Object o) {
            NodeWrapper other = (NodeWrapper) o;
            if (this.node == null && other.node == null) {
                return true;
            } else if (this.node == null || other.node == null) {
                return false;
            } else {
                return this.node.val == other.node.val;
            }
        }
    }

    public boolean isSymmetric(TreeNode root) {
        if (root == null) {
            return true;
        }
        List<NodeWrapper> l = new ArrayList<>();
        Queue<NodeWrapper> q = new LinkedList<>();
        int depth = 1;
        q.add(new NodeWrapper(root, depth));
        while (!q.isEmpty()) {
            NodeWrapper nw = q.poll();
            if (nw.depth != depth) {
                if (!isSymmetric(l)) {
                    return false;
                }
                l.clear();
                depth = nw.depth;
            }
            l.add(nw);
            if (nw.node != null) {
                q.add(new NodeWrapper(nw.node.left, depth + 1));
                q.add(new NodeWrapper(nw.node.right, depth + 1));
            }
        }
        return true;
    }

    private boolean isSymmetric(List<NodeWrapper> l) {
        int last = l.size() - 1;
        int mid = l.size() / 2;
        for (int i = 0; i < mid; i++) {
            NodeWrapper front = l.get(i);
            NodeWrapper back = l.get(last - i);
            if (!front.equals(back)) {
                return false;
            }
        }
        return true;
    }
}
```
