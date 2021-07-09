# LRU Cache

Design a data structure that follows the constraints of a Least Recently Used
(LRU) cache.

Implement the LRUCache class:

*   `LRUCache(int capacity)` initialize the LRU cache with **positive** size
    `capacity`.
*   `int get(int key)` return the value of the `key` if the key exists,
    otherwise return `-1`.
*   `void put(int key, int value)` update the value of the `key` if the `key`
    exists. Otherwise, add the `key-value` pair to the cache. If the number
    of keys exceeds the `capacity` from this operation, **evict** the least
    recently used key.

The functions `get` and `put` must each run in O(1) average time complexity.

## Hints

1. This is slightly tricky but I will give it away - by mapping `key`s to
   `Node`s, instead of directly to `value`, a `Node` can be mapped that is
   also a doubly linked list node. This allows for direct list removal and
   moving to the start of the list. Think on it.

## Solutions

### Map the Node to a double stinker

This solution follows the above hint. A `Node` is a doubly linked list node.
`key`s are mapped to the `Node` holding the `key` and the `value`. This enables
the magic of:

*   when retrieving what a `key` is mapped to, you get the doubly linked list
    `Node` enabling O(1) removal
*   when evicting last, the `key` is available to also remove it from the
    `Map`

Using this strategy the time complexity of both `get` and `put` is O(1).

```java
class LRUCache {
    private class Node {
        int key;
        int val;
        Node prev;
        Node next;
        Node(int key, int val) {
            this.key = key;
            this.val = val;
        }
    }

    private final int capacity;
    // Dummies prevent null checks in list manipulation.
    private final Node head;
    private final Node tail;
    private final Map<Integer, Node> m = new HashMap<>();

    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.head = new Node(0, 0);
        this.tail = new Node(0, 0);
        this.head.next = this.tail;
        this.tail.prev = head;
    }

    public int get(int key) {
        if (this.m.containsKey(key)) {
            Node n = this.m.get(key);
            remove(n);
            addFirst(n);
            return n.val;
        } else {
            return -1;
        }
    }

    public void put(int key, int val) {
        if (this.m.containsKey(key)) {
            Node n = this.m.get(key);
            n.val = val;
            remove(n);
            addFirst(n);
        } else {
            Node n = new Node(key, val);
            this.m.put(n.key, n);
            addFirst(n);
        }
        if (this.m.size() > this.capacity) {
            this.m.remove(this.tail.prev.key);
            remove(this.tail.prev);
        }
    }

    private void remove(Node n) {
        n.prev.next = n.next;
        n.next.prev = n.prev;
    }

    private void addFirst(Node n) {
        this.head.next.prev = n;
        n.next = this.head.next;
        this.head.next = n;
        n.prev = this.head;
    }
}
```
