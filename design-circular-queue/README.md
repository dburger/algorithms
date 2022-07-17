# Design Circular Queue

Design your implementation of the circular queue. The circular queue is a
linear data structure in which the operations are performed based on FIFO
(First In First Out) principle and the last position is connected back to
the first position to make a circle. It is also called a "Ring Buffer".

One of the benefits of the circular queue is that we can make use of the
spaces in front of the queue. In a normal queue, once the queue becomes
full, we cannot insert the next element even if there is a space in front
of the queue. But using the circular queue we can use the space to store
new values.

Implement the MyCircularQueue class:

1. `MyCircularQueue(k)` Initializes the object with the size of the queue to
   be `k`.
1. `int Front()` Gets the front item from the queue. If the queue is empty
   return `-1`.
1. `int Rear()` Gets the rear item from the queue. If the queue is empty
   return `-1`.
1. `boolean enQueue(int value)` Inserts an element into the circular queue.
   Return `true1 if the operation is successful.
1. `boolean deQueue(int value)` Deletes an element from the circular queue.
   Return `true1 if the operation is successful.
1. `boolean isEmpty()` Checks whether the circular queue is empty or now.
1. `boolean isFull()` Checks whether the circular queue is full or not.

You must solve the prolbem without using the built-in queue data structure in
your programming language.

## Hints

1. Know your modulus operation.

## Solutions

### Standard approach

Here we use the standard approach where the values are stored in an array and
the modulus operation is used to wrap around the end of the array.

```java
class MyCircularQueue {
    private int[] values;
    private int rear = -1;
    private int front = 0;
    private int size = 0;

    public MyCircularQueue(int k) {
        this.values = new int[k];
    }

    public boolean enQueue(int value) {
        if (size == values.length) {
            return false;
        }
        rear = (rear + 1) % values.length;
        this.values[rear] = value;
        size++;
        return true;
    }

    public boolean deQueue() {
        if (size == 0) {
            return false;
        }
        this.front = (front + 1) % values.length;
        size--;
        return true;
    }

    public int Front() {
        return size == 0 ? -1 : this.values[front];
    }

    public int Rear() {
        return size == 0 ? -1 : this.values[rear];
    }

    public boolean isEmpty() {
        return size == 0;
    }

    public boolean isFull() {
        return size == values.length;
    }
}
```
