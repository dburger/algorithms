# Clone Graph

Given a reference of a node in a connected undirected graph, return a
**deep copy** (clone) of the graph.

Each node in the graph contains a val (int) and list (List<Node>) of its
neighbors.

```java
class Node {
  public int val;
  public List<Node> neighbors;
}
```

**Test case format:**

For simplicity sake, each node's value is the same as the nodes index
(1-indexed). For example, the first node with `val = 1`, the second
with `val = 2`, and so on. The graph is represented in the test case
using an adjaceny list.

**Adjacency list** is a collection of unordered **lists** used to
represent a finite graph. Each list describes the set of neighbors of
a node in the graph.

The given node will always be the first node with `val = 1`. You must
return a **copy of the given node** as a reference to the cloned graph.

## Hints

1. You will need to do a traversal through the input graph by starting
   with the first node and its `neighbors`. Think of keeping track of
   nodes yet to process.

## Solutions

### Stack and map

This solution uses a Stack to track the input nodes that need to be processed.
It uses the common approach of iterating until the stack is empty. The cloned
nodes are tracked in a map by their indexed value. Neighbors on the clone are
fixed up when the input node is processed.

```java
class Solution {
    public Node cloneGraph(Node n) {
        if (n == null) {
            return n;
        }
        // Holds input nodes that need to be processed.
        Stack<Node> nodeStack = new Stack<>();
        // Holds cloned nodes and acts as an indicator if an
        // input node has been added to nodeStack yet.
        Map<Integer, Node> cloneMap = new HashMap<>();
        nodeStack.push(n);
        // Reset to null so we can use this reference as the
        // return value, set below.
        n = null;
        while (!nodeStack.isEmpty()) {
            Node original = nodeStack.pop();
            Node clone = cloneMap.get(original.val);
            if (clone == null) {
                clone = new Node(original.val);
                cloneMap.put(clone.val, clone);
            }
            List<Node> neighbors = new ArrayList<>();
            for (Node originalNeighbor : original.neighbors) {
                Node cloneNeighbor = cloneMap.get(originalNeighbor.val);
                if (cloneNeighbor == null) {
                    cloneNeighbor = new Node(originalNeighbor.val);
                    cloneMap.put(cloneNeighbor.val, cloneNeighbor);
                    nodeStack.add(originalNeighbor);
                }
                neighbors.add(cloneNeighbor);
            }
            clone.neighbors.addAll(neighbors);
            if (n == null) {
                n = clone;
            }
        }
        return n;
    }
}
```
