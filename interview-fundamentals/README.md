# Interview Fundamentals

TODO From other lists:

Data structures:
- Linked Lists
- Trees, Tries, Graphs
- Stacks and Queues
- Heaps
- Vectors / ArrayLists
- Hash Tables

Algos:
BFS
DFS
Binary Search
Merge Sort
Quick Sort

Concepts:
Bit Manipulation
Memory (stack vs heap)
Recursion
Dynamic Programming
Big O

TODO: sliding window techniques
TODO: a recursion section?
TODO: logarithms

This document is a guide to the fundamentals of technical interviews for
software engineers.

## Core Concepts

### [Big O Time and Space Complexity](https://en.wikipedia.org/wiki/Big_O_notation)

"Big O" is a concept that allows us to characterize the running time and memory
space expansion of an algorithm based upon the input size. Formally, an algorithm
f(n) is said to be O(g(n)) if f(n) <= Mg(n) for n > m where M is some constant. In
other words, it is an upper bound on the algorithm. Of course giving a tight upper
bound is preferred.

Some of the most commonly used bounding functions, in order are:

O(1)
O(log(n))
O(n)
O(n * log(n))
O(n^2)
O(n^3)
O(2^n)
O(n!)

#### Time Complexity for Algorithms to Generate Tree Like Structures

Many algorithms solutions generate or expand into tree like structures. The
time complexities of these are often bound by the creation of a full tree
of a certain depth `d` with maximum branching `b`. The time complexity of
such solutions follows from how many nodes are traversed or produced, which
is `O(b^d)`.

The fibonacci sequence is an example of an algorithm that can be implemented
in a way that produces a tree. This leads directly from the definition of
the sequence where `fib(n) = fib(n - 1) + fib(n - 2)` with base cases of
`fib(0) = 0` and `fib(1) = 1`. This leads to a branching factor of 2 with a
depth that reduces n by 1 at each layer. Thus the time complexity is bound by
O(2^n). What happens if we slap in a memoizer? Think of `fib(5)`. The first
execution will spawn `fib(4)` + `fib(3)`. By the time the "right child",
`fib(3)` executes, the left child will have already seeded the memoizer with
that result. This makes it so that execution will proceed down the left
children, and all the right children will return immediately from the memoizer.
Thus, with a memoizer, the time complexity of the fibonacci implementation
is merely O(n).

#### Doubling Time

Many algorithms split something in half and operate only on one of the halves.
They may continue to do this until there is only one unit left. For a given
value `n`, these must operate `k` times until `2^k = n`. For discrete
operations, we take the ceiling of `k`. This corresponds to the logarithm
`log2(n) = k`. Thus the often seen `log(n)` time complexity.

#### ArrayList Resizing and Amortized Time Complexity

Many languages feature built in array like data structures that feature
automatic resizing. In java this is the `ArrayList`. A fundamental Big O
question is how do these data structures perform when the adding of an element
can cause an O(n) copy of the old elements into a new array of say double the
size. We can think of this in terms of the number of copies that take place. Say
we start with a backing array of size one and we double each time we reach full
capacity. After a number of additions, the number of copies that were made would
be:

`1 + 2 + 4 + 8 + ... + x`

Reversing this, stating it in terms of `x` and summing these together, we have:

`x + x/2 + x/4 + x/8 + ... + 1 = 2x`

So for the insertion of `x` items we had `2x` copies and thus insertion of a
single element into such a data structure is amortized O(1).

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

You should know how to implement binary search in both iterative and recursive
fashion. You should also know the way determine the midpoint that helps to
avoid overflow.

Iterative:

The iterative solution has the advantage of avoiding the memory from the
recursive stack frames and ever the ever so slight slowdown from the recursion.

```java
int binarySearch(int[] values, int target) {
  int lo = 0;
  int hi = values.length - 1;
  while (lo <= hi) {
    int mid = lo + (hi - lo) / 2;
    int value = values[mid];
    if (target == value) {
      return mid;
    }
    if (target < value) {
       hi = mid - 1;
    } else {
       lo = mid + 1;
    }
  }
  return -1;
}
```

Recursive:

The recursive solution is just clean and simple. Slightly slower and more
memory intensive than the iterative solution.

```java
int binarySearch(int[] values, int target) {
  return binarySearch(values, target, 0, values.length - 1);
}

int binarySearch(int[] values, int target, int lo, int hi) {
  int mid = lo + (hi - lo) / 2;
  int value = values[mid];
  if (target == value) {
    return mid;
  }
  if (target < value) {
    mid = hi - 1;
  } else {
    mid = lo + 1;
  }
  return binarySearch(values, target, lo, hi);
}
```
### Horner's Method

Horner's method is a technique for converting between positional numeric
systems. In algorithm problems it can come in handy in a variety of
situations. The most bare bones example would be in converting a string
into a number. See how it gets the correct power of ten by multiplying
the prior result by 10.

```java
int convert(String value) {
  int result = 0;
  for (char c : value.toCharArray()) {
    result += 10 * result + (c - '0');
  }
  return result;
}
```

### Stripping Digits

TODO

### Bit Techniques

#### Flipping bits

Many algorithm problems involving bit manipulation will require you to flip
bits. Thus you should know how to do the basic bit operations. Setting a bit
can be done by or'ing the input by a mask with a single bit set in the
appropriate position. In Java this is done as:

```java
int setBit(int val, int bit) {
  return val | (1 << bit);
}
```

Clear bit is somewhat the opposite. You need to and with a mask that contains
a zero in the bit to clear and a one everywhere else. Again, in Java:

```java
int clearBit(int val, int bit) {
  return val & ~(1 << bit);
}
```

#### Counting bits

Counting bits can be done by sliding a one through every bit position and
keep track of how many times and'ing with that mask results in a non-zero.

```java
int countBits(int val) {
    int count = 0;
    for (int i = 0; i < 32; i++) {
        if ((val & (1 << i)) != 0) {
            count++;
        }
    }
    return count;
}
```

Ah, but there is a trick. There is a better way to count bits. When you
subract one from a number, in the binary representation, the least significant
bit is changed to a zero and everything to its right becomes a one. This
provides an optimized way to count bits. When you AND this subtracted by one
value with the original number, this results in the lowest bit being cleared.
Thus you can count bits by repeating this process until you drive the number
to one. In practice, this means this loops only exactly as many times as the
number of bits.

```java
int cb(int val) {
    int count = 0;
    while (val != 0) {
        count++;
        val = val & (val - 1);
    }
    return count;
}
```

TODO - enumerating set, finding set with constraint

### Negative Modulus

TODO

### Counting characters

Many algorithm problems 

TODO: counts[c - 'a'] += 1;

## Core Techniques

### Hash Table for Marking Visited

TODO

### Hash Table for Memoization

TODO

### Classes to use for Stacks, Queues, Multiset (counting map) in Java

#### Stack

#### Queue

#### Counting map

Guava contains a data structure called a [Multiset](https://guava.dev/releases/18.0/api/docs/com/google/common/collect/Multiset.html). A multiset allows you
to add objects to it and it maintains a count for objects considered equal.
For interviews you can likely assume that this class is available and use it
in your solutions. If you need to write counting code without this third party
library a map can be used for simple counting. This is useful for many
leeter problems. The code would look something like this:

```java
Map<Thingy, Integer> m = new HashMap<>();
m.put(t, m.getOrDefault(t, 0) + 1);
```

Removal can be handled via subtraction in a similar way, but you likely want to
remove the entry instead of allowing zero / allowing to go negative.

### Heap

TODO

### Suffix Trees

TODO

### Manipulating Multiple Pointers / Indices

TODO - index off center, longest common palindromic subsequence, march to the middle

### Sliding window

There is a large class of problems that can be solved by a sliding window
algorithm approach. Typically these problems have an input that is an array
or list and ask for some optimization across a contiguous subsequence of the
input.

The solution process is some variation of the following:

*   Set up temp variables to hold current and max values.
*   Set up temp variables for the left and right sides of the window, `l` and
    `r`, starting at `0`.
*   Set up temp variables that will be used to track if the current subsequence
    is valid. For example, this could be a `Set` that tracks the values in the
    window if the criteria for a valid subsequence is that all values must be
    unique.
*   In a loop consume the next value at `r`. If this value causes the window
    to be invalid slide `l` to the right until it is valid again. All while
    keeping the current and max variables up to date.
*   Increment `r` and continue with the next iteration until you have consumed
    the entire input.

Examples of sliding window algorithm problems include:

*   [Maximum Erasure Value](../maximum-erasure-value)
*   [Find All Anagrams in a String](../find-all-anagrams-string)

### Sorting Algorithms

### Dynamic Programming

TODO - memoization and tabular approaches

## Core Questions

### Primes

TODO(dburger): check if n prime
TODO(dburger): sieve

### Matching Parenthesis

TODO

### Reverse a Linked List

TODO - recursive and iterative

### Detect a Cycle in a Linked List

TODO

## Core Knowledge

### Sorting Time Complexity

TODO
