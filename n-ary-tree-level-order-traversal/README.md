# N-ary Tree Level Order Traversal

Given an n-ary tree, return the *level order* traversal of its nodes' values.

## Hints

1. A BFS exploration of the tree with an accumulator should do the trick. The
   fact that this is an n-ary tree rather than a binary tree changes the
   algorithm little, and a BFS exploration of a tree should be in your bag of
   tricks.

## Solutions

### Standard BFS with queue

This solution follows the hint above, accumulating the lists as it does a
typical BFS queue approach through the queue. If the nodes had merely been
requested in the BFS order (a flat list), the depth and list wrangling could
have been avoided here. This adds a bit of a wrinkle to the problem. Here
the depth is tracked and when it changes a new list is introduced to continue
the accumulation.

The time complexity of this solution is O(n) where n is the number of nodes
in the tree. The space complexity is also O(n) as the list of lists has
one integer entry per node.

```java
class Solution {
    private class NodeWrapper {
        Node node;
        int depth;
        NodeWrapper(Node node, int depth) {
            this.node = node;
            this.depth = depth;
        }
    }

    public List<List<Integer>> levelOrder(Node root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        List<Integer> l = new ArrayList<>();
        result.add(l);
        Queue<NodeWrapper> q = new LinkedList<>();
        int depth = 1;
        q.add(new NodeWrapper(root, depth));
        while (!q.isEmpty()) {
            NodeWrapper nw = q.poll();
            if (nw.depth != depth) {
                l = new ArrayList<>();
                result.add(l);
                depth = nw.depth;
            }
            l.add(nw.node.val);
            for (Node n : nw.node.children) {
                q.add(new NodeWrapper(n, depth + 1));
            }
        }
        return result;
    }
}
```

### DFS with depth capture

In this approach we traverse the tree in a DFS manner, however, we additionally
pass:

1. an accumulator for the lists
1. the depth

This allows us to capture the values into the correct level order list during
the DFS traversal.

This solution also has O(n) time complexity as it must traverse every node in
the tree. The space complexity is O(n) as well. In contrast to the prior problem
the space complexity doesn't just follow from the accumulation of the n nodes
into a list of lists. In this case the recursion stack could potentially be n
levels deep as well.

There is less code here than the prior example, although that has mainly to do
with not needing the `NodeWrapper` approach to track depth. Here the depth is
managed via an argument to the recursive calls.

```java
class Solution {
    public List<List<Integer>> levelOrder(Node root) {
        List<List<Integer>> acc = new ArrayList<>();
        levelOrder(root, 1, acc);
        return acc;
    }

    private void levelOrder(Node node, int depth, List<List<Integer>> acc) {
        if (node == null) {
            return;
        }
        List<Integer> l;
        if (depth > acc.size()) {
            l = new ArrayList<>();
            acc.add(l);
         } else {
            l = acc.get(depth - 1);
        }
        l.add(node.val);
        for (Node n : node.children) {
            levelOrder(n, depth + 1, acc);
        }
    }
}
```
