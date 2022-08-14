# Implement Stack Using Queues

Implement a last-in-first-out (LIFO) stack using only two queues. The
implemented stack should support all the functions of a normal stack
(`push`, `top`, `pop`, and `empty`).

Implement the `MyStack` class:

- `void push(int x)` Pushes element `x` to the top of the stack.
- `int pop()` Removes the element on the top of the stack and returns it.
- `int top()` Returns the element on the top of the stack.
- `boolean empty()` Returns `true` if the stack is empty, `false` otherwise.


## Hints

1. Think about it? If the data must be held in a queue, this last out of
   the queue is first out of the stack.

## Solutions

### Fire drill

Stacks are LIFO. Queues are LIFO. We can simulate the stack using queues
by taking the last item in the queue as the top of the stack. Using two
queues, we can do this by sending the items back and forth amongst two
queues. We keep `curr` and `spare` pointers to keep track of which queue
is currently doing what.

The time complexity of the various operations is:

- `void push(int x)` O(1)
- `int pop()` O(n) where `n` is the size of the "stack" at the time
- `int top()` O(1)
- `boolean empty()` O(1)

The space complexity is O(n) where `n` is the number of elements in
the stack.

```java
class MyStack {
    private final Queue<Integer> q1 = new LinkedList<>();
    private final Queue<Integer> q2 = new LinkedList<>();
    private Queue<Integer> curr = q1;
    private Queue<Integer> spare = q2;
    private int last = -1;

    public MyStack() {
    }

    public void push(int x) {
        this.curr.add(x);
        this.last = x;
    }

    public int pop() {
        while (this.curr.size() > 1) {
            this.last = this.curr.remove();
            this.spare.add(this.last);
        }
        int result = this.curr.remove();
        Queue<Integer> temp = curr;
        curr = spare;
        spare = temp;
        return result;
    }

    public int top() {
        return this.last;
    }

    public boolean empty() {
        return curr.isEmpty();
    }
}
```
