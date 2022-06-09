# Network Delay Time

You are given a network of `n` nodes, labeled from `1` to `n`. You are also
given `times`, a list of travel times as directed edges `times[i] = (u, v, w)`
where `u` is the source node, `v` is the target node, and `w` is the time it
takes for a signal to travel from source to target.

We will send a signal from a given node `k`. Return the **minimum** time it
takes for all the `n` nodes to receive the signal. If it is impossible for all
the `n` nodes to receive the signal, return `-1`.

## Hints

1. Is there a classic graph algorithm hidden in there?

## Solutions

### Dijkstra to the rescue.

This problem can be solved with Dijkstra's Shortest Path Algorithm. It works by
putting the nodes in a priority queue and then removing them in a greedy
manner. That is the one with the shortest distance is removed, marked as that
nodes minimum travel time, and then its neighbors are entered in the priority
queue with their distances. A `visited` set is used to track nodes that have
already been visited and do not need to be processed again. This greedy method
works because the node that is closest cannot possibly be reached in a shorter
distance through a node with a further distance.

The time complexity for this solution is O(n * log(n)) where n is the size of the
input array `times`. This comes from having enqueue and dequeue all the nodes
into the priority queue where each such operation is O(log(n)). The space complexity
for this solution is O(n). This comes from creating the edge map as well as
handling the space for the priority queue.

```java
class Solution {
    class Edge {
        int source;
        int target;
        int time;

        Edge(int source, int target, int time) {
            this.source = source;
            this.target = target;
            this.time = time;
        }
    }

    class Node implements Comparable {
        int source;
        int time;

        Node(int source, int time) {
            this.source = source;
            this.time = time;
        }

        @Override
        public int compareTo(Object o) {
            Node other = (Node) o;
            return this.time - other.time;
        }
    }

    public void display(Map<Integer, List<Edge>> m) {
        for (Map.Entry<Integer, List<Edge>> e : m.entrySet()) {
            System.out.print(e.getKey() + ": ");
            for (Edge edge : e.getValue()) {
                System.out.print(edge.target + ", ");
            }
            System.out.println();
        }
    }

    public int networkDelayTime(int[][] times, int n, int k) {
        Map<Integer, List<Edge>> m = new HashMap<>();
        for (int[] t : times) {
            int source = t[0];
            int target = t[1];
            int time = t[2];
            List<Edge> l = m.get(source);
            if (l == null) {
                l = new ArrayList<>();
                m.put(source, l);
            }
            l.add(new Edge(source, target, time));
        }

        Set<Integer> visited = new HashSet<>();
        int[] result = new int[n + 1];
        Arrays.fill(result, Integer.MIN_VALUE);
        PriorityQueue<Node> pq = new PriorityQueue<>();
        pq.add(new Node(k, 0));
        while (!pq.isEmpty()) {
            Node node = pq.remove();
            if (visited.contains(node.source)) {
                continue;
            }

            visited.add(node.source);
            result[node.source] = node.time;

            for (Edge e : m.getOrDefault(node.source, Collections.emptyList())) {
                if (visited.contains(e.target)) {
                    continue;
                }

                pq.add(new Node(e.target, node.time + e.time));
            }
        }

        int max = Integer.MIN_VALUE;
        for (int i = 1; i < result.length; i++) {
            int time = result[i];
            if (time == Integer.MIN_VALUE) {
                return -1;
            } else if (time > max) {
                max = time;
            }
        }

        return max;
    }
}
```
