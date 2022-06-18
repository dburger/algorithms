# Count Complete Tree Nodes

Given the `root` of a **complete** binary tree, return the number of nodes in
the tree.

According to Wikipedia, every level, except possibly the last, is completely
filled in a complete binary tree, and all nodes in the last level are as far
left as possible. It can have between `1` and `2^h` nodes inclusive at the
last level `h`.

Design an algorithm that runs in less than `O(n)` time complexity.

## Hints

1. Ignore the constraint for a nice little trivial tree exercise.
1. Good Lord can you divide and conquer it?

## Solutions

I like this problem a lot. Without the "less than `O(n)` time complexity"
constraint this is a beginner problem. With it, an interesting solution
becomes necessary.

### Ignore the constraint

Often in an interview you'll want to solve a problem in a non-optimal way very
quickly and then move on. This shows the interviewee that you know about
alternatives and why they may not be the best. This problem of course has a
very easy O(n) solutuion that is pretty much the canonical "I understand
recursion" and "I have worked with tree structures" solution. The solution
is a one-liner.

As mentioned this solution has time complexity O(n) where `n` is the number of
nodes in the tree. The space complexity is O(log(n)) as the stack frame depth
for the balanced binary tree is the depth of the tree.

```java
class Solution {
    public int countNodes(TreeNode root) {
        return root == null ? 0 : 1 + countNodes(root.left) + countNodes(root.right);
    }
}
```

### Are we there yet?

This is just a very neat solution. What is the simplest way we can check if a tree
has a last level that is full from left to right? Well, merely count how many times
you can go left from the root, count how many times you can go right from the root,
if these are equal, the last level is completely full. This leads to a rather cool
recursive approach which works as follows:

1. Determine if the last level is full. If it is return the number of nodes which
   is easily calculated from the depth.
1. If it is not full, return 1 (for the current node) + countNodes(left) + countNodes(right)

It is almost as simple as the one liner above.

What is the time complexity of this solution? When we recurse into the two
subtrees the split where the last row has no more nodes can only be in one of
the subtrees. Thus one of the recursions terminates while the other may
continue. This makes it so that the tree is split in half on each recursion and
the depth remaining is reduced by 1. Thus to reduce the depth to 0 we may take
up to log(n) invocations. On each iteration we have to count how tall the
remaining subtree is on the left and right sides which can result in up to
log(n) hops to get to the bottom. Thus the time complexity is O((log(n)^2)).
The space complexity is O(log(n)) to hold the stack frames to recurse to the
bottom of the tree.

```java
class Solution {
    public int countNodes(TreeNode root) {
        if (root == null) {
            return 0;
        }

        int lefts = lefties(root);
        int rights = righties(root);

        if (lefts == rights) {
            // Last level is full.
            return (int) Math.pow(2, lefts + 1) - 1;
        } else {
            // Last level not full, return 1 for root plus subtree counts.
            return 1 + countNodes(root.left) + countNodes(root.right);
        }
    }

    private int lefties(TreeNode root) {
        int lefts = 0;
        while (root.left != null) {
            lefts++;
            root = root.left;
        }
        return lefts;
    }

    private int righties(TreeNode root) {
        int rights = 0;
        while (root.right != null) {
            rights++;
            root = root.right;
        }
        return rights;
    }
}
```
