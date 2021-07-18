# Top k Frequent Elements

Given an integer array `nums` and an integer `k`, return the `k` most frequent
elements. You may return the answer in any order.

**Follow up:** Your algorithm's time complexity must be better than
O(n * log(n)), where n is the array's size.

## Hints

1. A solution that doesn't target the follow up is not hard. Using a `Map`
   or `Multiset` to count and then sorting by the counts might do.
1. To hit the follow up you will need to pull inspiration from the world of
   qsort...but you don't need to sort. This falls in the class of partition
   problems.

## Solutions

### Count, sort, and get me out of here

The solution, while not attempting to achieve the follow up, works by first
mapping the `nums` into a `counts` map. The `Map.Entry` of this map are then
moved into a list. This list is sorted (with custom comparator) by the counts
in decreasing order. Then simply the first `k` elements can be returned.

The time complexity of this solution is O(n * log(n)) in order to sort the
list. The space complexity of this solution is O(n) for holding the `counts`
and list `l`.

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> m = new HashMap<>();
        for (int n : nums) {
            m.put(n, m.getOrDefault(n, 0) + 1);
        }
        List<Map.Entry<Integer, Integer>> l = new ArrayList<>(m.entrySet());
        Collections.sort(l, (o1, o2) -> o2.getValue() - o1.getValue());
        int[] top = new int[k];
        for (int i = 0; i < k; i++) {
            top[i] = l.get(i).getKey();
        }
        return top;
    }
}
```

### Partition baby

This is the partition algorithm approach. Here we build up a `counts` map and
an array of `uniques`. These are passed into a partition sort algorithm that
recurses down to the `k` position. Finally the last `k` elements of the now
sorted array are returned as the result.

Note that underneath this is the standard partition algorithm. Twists here are
the precomputation of `counts` and `uniques` and how the elements are sorted
by their associated value in `counts`.

The time complexity of this algirthm is O(n) in the average case and O(n^2) in
the degenerate case where the chosen pivot only reduces the array by 1. The
space complexity of this algorithm is O(n) as necessitated by the holding of
`counts` and `uniques`. Also, in theory, the recursive call stack is O(n) in
the degenerate case.

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        // Grab the counts for each num.
        Map<Integer, Integer> counts = new HashMap<>();
        for (int n : nums) {
            counts.put(n, counts.getOrDefault(n, 0) + 1);
        }
        // Now fill an array with just the uniques.
        int[] uniques = new int[counts.size()];
        int i = 0;
        for (int n : counts.keySet()) {
            uniques[i++] = n;
        }

        // Now partition sort out at k positions from the end.
        partition(uniques, counts, uniques.length - k, 0, uniques.length - 1);
        // Finally return those last k elements.
        int[] result = Arrays.copyOfRange(uniques, uniques.length - k, uniques.length);
        return result;
    }

    // Classic partition algorithm. It is qsort only to the k position.
    private void partition(int[] uniques, Map<Integer, Integer> counts, int k, int lo, int hi) {
        if (lo == hi) {
            return;
        }
        int fill = lo;
        int pval = counts.get(uniques[hi]);
        for (int i = lo; i < hi; i++) {
            if (counts.get(uniques[i]) < pval) {
                int temp = uniques[fill];
                uniques[fill++] = uniques[i];
                uniques[i] = temp;
            }
        }
        int temp = uniques[hi];
        uniques[hi] = uniques[fill];
        uniques[fill] = temp;

        if (fill < k) {
            partition(uniques, counts, k, fill + 1, hi);
        } else if (fill > k) {
            partition(uniques, counts, k, lo, fill - 1);
        }
    }
}
```
