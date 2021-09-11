# Top k Frequent Words

Given an array of strings `words` and an integer `k`, return the `k` most
frequent strings.

Return the answer sorted by the frequency from highest to lowest. Sort the
words with the same frequency by their lexicographical order.

## Hints

1. Start with a count of each word and then enter the word/count values into
   a standard data structure. Which one?

## Solutions

### Maintain your list

This solution uses a doubly linked list and a map from word to `Node` for that
word. When a word is added to the list it is added to the tail and then
positioned appropriately. When a word that has already been added to the list
is encountered, it is retrieved, the count is incremented, and then it is
repositioned to the appropriate spot.

The time complexity of this algorithm in the worst case is O(n^2). This
corresponds to each encountered word needing to be moved to the front of
this list. The space complexity is O(n) as we need a `Node` for potentially
each `word` of `words`.

```java
class Solution {
    private class Node {
        private final String word;
        private int count;

        private Node prev;
        private Node next;

        Node(String word) {
            this.word = word;
            this.count = 1;
        }
    }

    public List<String> topKFrequent(String[] words, int k) {
        Node head = new Node("head");
        Node tail = new Node("tail");
        head.next = tail;
        tail.prev = head;

        Map<String, Node> m = new HashMap<>();
        for (String word : words) {
            if (m.containsKey(word)) {
                Node n = m.get(word);
                n.count += 1;
                reposition(n, head, tail);
            } else {
                Node n = new Node(word);
                tail.prev.next = n;
                n.prev = tail.prev;
                n.next = tail;
                tail.prev = n;
                m.put(word, n);
                reposition(n, head, tail);
            }
        }

        List<String> result = new ArrayList<>();
        Node curr = head.next;
        while (k-- > 0) {
            result.add(curr.word);
            curr = curr.next;
        }
        return result;
    }

    private void reposition(Node n, Node head, Node tail) {
        while (n.prev != head && (n.count > n.prev.count || (n.count == n.prev.count && n.word.compareTo(n.prev.word) < 0))) {
            Node prev = n.prev;
            Node next = n.next;
            n.prev = prev.prev;
            n.next = prev;
            prev.prev.next = n;
            prev.prev = n;
            prev.next = next;
            next.prev = prev;
        }
    }
}
```

### Mind your p`s and q`s, the priority queue

This solution merely produces a count for each word and then enters all the
values into a `PriorityQueue`. With the values entered as a class that
appropriately implements `Comparable`, the values can be retrieved in the
proper order.

The time complexity of this solution is O(n * log(n)). This comes from adding
the `Node`s into the priority queue. The space complexity is O(n). This
comes from the counting map and the priority queue.

```java
class Solution {
    private class Node implements Comparable<Node> {
        private final String word;
        private final int count;

        Node(String word, int count) {
            this.word = word;
            this.count = count;
        }

        @Override
        public int compareTo(Node o) {
            Node other = (Node) o;
            return this.count == o.count ? this.word.compareTo(o.word) : o.count - this.count;
        }
    }

    public List<String> topKFrequent(String[] words, int k) {
        Map<String, Integer> m = new HashMap<>();
        for (String word : words) {
            m.put(word, m.getOrDefault(word, 0) + 1);
        }

        PriorityQueue<Node> pq = new PriorityQueue<>();
        for (Map.Entry<String, Integer> e : m.entrySet()) {
            pq.add(new Node(e.getKey(), e.getValue()));
        }

        List<String> result = new ArrayList<>();
        while (k-- > 0) {
            result.add(pq.poll().word);
        }

        return result;
    }
}
```
