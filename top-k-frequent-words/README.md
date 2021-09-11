# Top k Frequent Words

Given an array of strings `words` and an integer `k`, return the `k` most
frequent strings.

Return the answer sorted by the frequency from highest to lowest. Sort the
words with the same frequency by their lexicographical order.

## Hints

1. Start with a count of each word and then enter the word/count values into
   a standard data structure. Which one?

## Solutions

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
