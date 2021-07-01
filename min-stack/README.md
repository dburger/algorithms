# Min Stack

Design a stack that supports push, pop, top, and retrieving the minimum
element in constant time. Implement the `MinStack` class:

*   `MinStack()` initializes the stack object.
*   `void push(val)` pushes the element `val` onto the stack.
*   `void pop()` removes the element on the top of the stack.
*   `int top()` gets the top element of the stack.
*   `int getMin()` retrieves the minimum element in the stack.

## Hints

1. Note that this is a stack and you aren't being asked to implement a call
   to remove the minimum element. Thus you can always record the current
   minimum element on each push.

## Solutions

### Wrap a deck....err a Deque

This solution merely delegates to a `Deque` instance while recording the min
on each push. A private `Node` class is used to record the `val` along with
the current `min`.

```java
class MinStack {

    private static class Node {
        int val;
        int min;
        Node(int val, int min) {
            this.val = val;
            this.min = min;
        }
    }

    private Deque<Node> deque = new ArrayDeque<>();

    /** initialize your data structure here. */
    public MinStack() {
    }

    public void push(int val) {
        if (deque.isEmpty()) {
            deque.push(new Node(val, val));
        } else {
            deque.push(new Node(val, Math.min(val, deque.peek().min)));
        }
    }

    public void pop() {
        if (deque.isEmpty()) {
            throw new IllegalStateException("MinStack is empty.");
        } else {
            deque.pop();
        }
    }

    public int top() {
        if (deque.isEmpty()) {
            throw new IllegalStateException("MinStack is empty.");
        } else {
            return deque.peek().val;
        }
    }

    public int getMin() {
        if (deque.isEmpty()) {
            throw new IllegalStateException("MinStack is empty.");
        } else {
            return deque.peek().min;
        }
    }
}
```
