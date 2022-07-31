# Peeking Iterator

Design an iterator that supports the `peek` operations on an existing iterator
in addition to the `hasNext` and `next` operations.

Implement the `PeekingIterator` class:

- `PeekingIterator(Iterator<Integer> nums)` Initializes the object with the
  given `iterator`.
- `int next()` Returns the next element in the array and moves the pointer to
  the next element.
- `boolean hasNext()` Returns `true` if there are still elements in the array.
- `int peek()` Returns the next element in the array **without** moving the
  pointer.

**Note:** Each language may have a different implementation of the constructor
and `Iterator`, but that all support the `int next()` and `boolean hasNext()`
functions.

## Hints

1. You want to avoid coding two different states as in, last call was `peek()`
   and last call was `next()`.

## Solutions

### Pre-process next

As noted in the hint, you want to avoid confusing code that does two different
things for `peek()` and `next()`. This can be done by simply pre-processing
the `next` element. This makes it so that:

1. `peek()` merely returns what has already been determined.
1. `hasNext()` merely checks if what has already been processed is a null
1. `next()` merely returns what has already been determined, and then
   pre-processes the next element.

The time complexity of this solution does not differ from the underlying
iterator. The space complexity is O(1).

```java
class PeekingIterator implements Iterator<Integer> {
    private final Iterator<Integer> iterator;
    private Integer next;

    public PeekingIterator(Iterator<Integer> iterator) {
        this.iterator = iterator;
        if (this.iterator.hasNext()) {
            this.next = this.iterator.next();
        }
    }

    // Returns the next element in the iteration without advancing the iterator.
    public Integer peek() {
        return this.next;
    }

    // hasNext() and next() should behave the same as in the Iterator interface.
    // Override them if needed.
    @Override
    public Integer next() {
        Integer temp = this.next;
        if (this.iterator.hasNext()) {
            this.next = this.iterator.next();
        } else {
            this.next = null;
        }
        return temp;
    }

    @Override
    public boolean hasNext() {
        return this.next != null;
    }
}
```
