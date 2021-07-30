# Interview Fundamentals

This document is a guide to the fundamentals of technical interviews for
software engineers.

## Core Algorithms

### DFS

A depth first search, or DFS, is a search approach in a tree or graph structure
that searches for a target by starting at some node (usually the root) and
explores as far as possible along each branch before backtracking.

#### Basic DFS, recursion

DFS can be implemented using recursion. A basic DFS using recursion
implemented in Java follows:

```java
boolean exists(TreeNode node, int target) {
    if (node == null) {
        retturn false;
    }
    if (node.val == target) {
        return true;
    }
    return exists(node.left) || exists(node.right);
}
```

#### Basic DFS, stack

DFS can also be implemented using a stack. A basic DFS using a stack
implemented in Java follows:

(Note that this search order is different than the recursive version above.
 To get the same order we should add right and then left.)

```java
boolean exists(TreeNode node, int target) {
    if (node == null) {
        return false;
    }
    Stack<TreeNode> s = new LinkedList<>();
    s.add(node);
    while (!s.isEmpty()) {
        TreeNode n = s.pop();
        if (n.val == target) {
            return true;
        }
        if (n.left != null) {
            s.push(n.left);
        }
        if (n.right != null) {
            s.push(n.right);
        }
    }
    return false;
}
```

### BFS

A breadth first search, or BFS, is a search approach in a tree or graph
structure that searches for a target by examining a level at a time. A queue
structure is used to ensure this order. Many interview questions require
either a BFS search or a BFS traversal.

#### Basic BFS

A basic BFS implemented in Java:

```java
boolean exists(TreeNode root, int target) {
    Queue<TreeNode> q = new LinkedList<>();
    q.add(root);
    while (!q.isEmpty()) {
        TreeNode n = q.remove();
        if (n.val == target) {
            return true;
        }
        if (n.left != null) {
            q.add(n.left);
        }
        if (n.right != null) {
            q.add(n.right);
        }
    }
}
```

#### Accumulate in BSF order

Many algorithm problems will require a BFS traversal and not a search. For
example say you want to return the tree in BFS order:

```java
List<TreeNod> breadFirst(TreeNode root) {
    List<TreeNode> result = new ArrayList<>();
    Queue<TreeNode> q = new LinkedList<>();
    q.add(root);
    while (!q.isEmpty()) {
        TreeNode n = q.remove();
        result.add(n);
        if (n.left != null) {
            q.add(n.left);
        }
        if (n.right != null) {
            q.add(n.right);
        }
    }
    return result;
}
```

#### BFS and knowing your level

Some problems may require you to know what level you are on. That can be
accomplished in a couple of different ways. One way is to use a wrapper node.
Another way is to consume a level at a time.

##### BFS level at a time with wrapper

One way to know the level you are on as you traverse is to wrap the `TreeNode`
instances in a class that tracks the level:

```java
class Node {
    TreeNode treeNode;
    int level;
    Node(TreeNode treeNode, int level) {
        this.treeNode = treeNode;
        this.level = level;
    }
}

void breadFirst(TreeNode root) {
    Queue<Node> q = new LinkedList<>();
    // Adding the root with level 0.
    q.add(new Node(root, 0));
    while (!q.isEmpty()) {
        Node n = q.remove();
        // here we have n.treeNode and n.level
        if (n.treeNode.left != null) {
            // The level is the parent's + 1.
            q.add(new Node(n.treeNode.left, n.level + 1));
        }
        if (n.treeNode.right != null) {
            // The level is the parent's + 1.
            q.add(new Node(n.treeNode.right, n.level + 1));
        }
     }
}
```

##### BFS consming a level at a time

While the wrapper approach works well, and may be the best fit for many
scenarios, it is also easy to consume a level at a time. This is done by
nesting loops where the inner loop consumes only the nodes already in the
queue at the start of that round, ignoring those added during the round.
A corresponding counter can be used to track what level you are on. The key is
snapshotting the length of the queue within the loop and only consuming the
nodes that existed at the start of that round.

```java
void breadFirst(TreeNode root) {
    Queue<TreeNode> q = new LinkedList();
    q.add(root);
    int level = 0;
    while (!q.isEmpty()) {
        // Snapshot the current length, all of these are in the same level.
        int len = q.size();
        for (int i = 0; i < len; i++) {
            Node n = q.remove();
            // Here we have a node and its level.
        }
        level++;
    }
}
```

TODO - queue, https://youtu.be/r1MXwyiGi_U?list=WL&t=200
with Wrapper (level) and without (zigzag has example)

### Binary Search

TODO - recursive and iterative

### Horner's Method

TODO

### Stripping Digits

TODO

### Bit Techniques

TODO - counting bits, enumerating set, finding set with constraint

### Negative Modulus

TODO

### Counting characters

TODO: counts[c - 'a'] += 1;

## Core Techniques

### Hash Table for Marking Visited

TODO

### Hash Table for Memoization

TODO

### Classes to use for Stacks, Queues, Multiset (counting map) in Java

TODO - explain map code to do multiset type map m.put(k, m.getOrDefault(k, 0) + 1);

### Manipulating Multiple Pointers / Indices

TODO - index off center, longest common palindromic subsequence, march to the middle

### Dynamic Programming

TODO - memoization and tabular approaches

## Core Questions

### Matching Parenthesis

TODO

### Reverse a Linked List

TODO - recursive and iterative

### Detect a Cycle in a Linked List

TODO

## Core Knowledge

### Sorting Time Complexity

TODO
