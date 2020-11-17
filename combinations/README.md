# Combinations

Given two integers `n` and `k`, return all possible combinations of
`k` numbers out of `1...n`.

## Hints

1. Could make the list and then use the "consumed and remaining"
   approach.
1. Could iterate through a binary number and choose the bits that are set.
1. Walk like a number?

## Solutions

### Consumed and remaining

This is a rather wonky approach. In this approach you literally create the
candidate list, and then use recursion to keep track of "consumed" and
"remaining" items. Here, to keep from adding the same item twice, the lists
are sorted and a set is used. This works, but it is a somewhat awkward
solution.

```java
class Solution {
  public List<List<Integer>> combine(int n, int k) {
    Set<List<Integer>> result = new HashSet<>();
    List<Integer> consumed = new ArrayList<>();
    List<Integer> remaining = new ArrayList<>();
    for (int i = 1; i < n + 1; i++) {
      remaining.add(i);
    }
    combine(k, consumed, remaining, result);
    return new ArrayList<>(result);
  }

  private void combine(int k, List<Integer> consumed, List<Integer> remaining,
      Set<List<Integer>> result) {
    if (consumed.size() == k) {
      Collections.sort(consumed);
      result.add(consumed);
      return;
    }
    for (int i = 0; i < remaining.size(); i++) {
      List<Integer> newConsumed = new ArrayList<>();
      newConsumed.addAll(consumed);
      List<Integer> newRemaining = new ArrayList();
      newRemaining.addAll(remaining);
      newConsumed.add(newRemaining.remove(i));
      combine(k, newConsumed, newRemaining, result);
    }
  }
}
```

### Watching bits

```java
class Solution {
    public List<List<Integer>> combine(int n, int k) {
        int max = (int) Math.pow(2, n);
        List<List<Integer>> result = new ArrayList<>();
        for (int i = 0; i < max; i++) {
            if (countSetBits(i) == k) {
                List<Integer> l = new ArrayList<>();
                int mask = 1;
                int offset = 1;
                while (mask < max) {
                    if ((mask & i) > 0) {
                        l.add(offset);
                    }
                    mask = mask << 1;
                   offset += 1;
                }
                result.add(l);
            }
        }
        return result;
    }

    private int countSetBits(int n) {
        int count = 0;
        while (n > 0) {
            n &= (n - 1);
            count++;
        }
        return count;
    }
}
```

### Iteration walk like a number

These indices are pretty hard to get right. First seed the first combination,
and then "walk" to the rest.

```java
class Solution {
    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> comb = new ArrayList<>();
        // Initialize the first combination. Note that this is one based.
        for (int i = 1; i <= k; i ++) {
            comb.add(i);
        }
        int i = k - 1;
        // Loop until the first element reaches its target. Since one based
        // here, the +2 instead of +1.
        while (comb.get(0) < n - k + 2) {
            result.add(new ArrayList<>(comb));
            // Find next increment position, which will be lower than its max
            // for that position, and increment it.
            while (i > 0 && comb.get(i) == n - k + i + 1) {
                i--;
            }
            comb.set(i, comb.get(i) + 1);
            // And increment the rest that follow up one.
            while (i < k - 1) {
                comb.set(i + 1, comb.get(i) + 1);
                i++;
            }
        }
        return result;
    }
}
```
