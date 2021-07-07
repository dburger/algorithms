# Binary Search Tree Iterator

Implement the `BSTIterator` class that represents an iterator over the
`in-order traversal` of a binary search tree (BST):

*   `BSTIterator(TreeNode root)` Initializaes an object of the `BSTIterator`
    class. The `root` of the BST is given as part of the constructor. The
    pointer should be initialized to a non-existent number smaller than any
    element in the BST.
*   `boolean hasNext()` Returns `true` if there exists a number in the
    traversal to the right of the pointer, otherwise returns `false`.
*   `int next()` Moves the pointer to the right, then returns the number at the
    pointer.

Notice that by initializing the pointer to a non-existent smallest number, the
first call to `next()` will return the smallest element in the BST.

You may assume that `next()` calls will always be valid. That is, there will be
at least a next number in the in-order traversal when `next()` is called.

## Hints

1. Tree traversals are easily handled with recursion. Here, this won't work.
   A stack based in-order traversal will make this possible.

## Solutions

### Stack it

Here we use a `Stack<TreeNode>` to perfrom the traversal, always leaving the
top of the stack with the next node to emit a value in the traversal.

In-order traversal travel as far left as possible, then visit, and then attempt
to repeat that with the right node. This translates to stack code with the
not visited nodes remaining on the stack to be backtracked to.

```java
class BSTIterator {
    private final Deque<TreeNode> d = new LinkedList<>();

    public BSTIterator(TreeNode root) {
        while (root != null) {
            d.push(root);
            root = root.left;
        }
    }

    public int next() {
        TreeNode n = d.pop();
        int result = n.val;
        n = n.right;
        while (n != null) {
            d.push(n);
            n = n.left;
        }
        return result;
    }

    public boolean hasNext() {
        return !d.isEmpty();
    }
}
```
