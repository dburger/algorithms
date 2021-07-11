# Single Number II

Given an integer array `nums` where every element appears three times except
for one, which appears exactly once. Find the single element and return it.

You must implement a solution with a linear runtime complexity and use only
constant extra space.

## Hints

1. There are a number of ways to "solve" this problem that will not satisfy
   the problem constraints. Namely, the "and use on constant extra space"
   constraint. To satisfy that you will need to understand bit magic.

## Solutions

### Count that map

Here is an obvious solution of counting in a map that is super easy to
implement.
It comes in at O(n) time complexity. The space complexity is also O(n), in
order to hold the counts in the map. Thus it does not satisfy the problem
constraints.

```java
class Solution {
    public int singleNumber(int[] nums) {
        Map<Integer, Integer> counts = new HashMap<>();
        for (int n : nums) {
            counts.put(n, counts.getOrDefault(n, 0) + 1);
        }
        for (Map.Entry<Integer, Integer> e : counts.entrySet()) {
            if (e.getValue() == 1) {
                return e.getKey();
            }
        }
        return -1;
    }
}
```

### Manipulate those sets

Here is another fairly obvious and easy to implement solution. Here we track
two sets, one that as seen things once, and another that has seen things
twice. In the end we use a set subtraction to figure out which number was only
seen once.

This solution has O(n) time complexity. This solution also does not satisfy
the problem constraints in that it has O(n) space complexity.

```java
class Solution {
    public int singleNumber(int[] nums) {
        Set<Integer> once = new HashSet<>();
        Set<Integer> twice = new HashSet<>();
        for (int n : nums) {
            if (!once.add(n)) {
                twice.add(n);
            }
        }
        once.removeAll(twice);
        return once.iterator().next();
    }
}
```

### Use some math

OK, one more non-constraint satisfying solution. Here we use a set to
determine the total sum and set sum for the input. After that is done we can
use some math to solve the problem. That is `3 * setSum` will be more than the
`sum` by `2 * element` where element is the single element. Think about it.

This solution has O(n) time complexity. Ah, but alas, we still have O(n) space
complexity to hold the `nums` in a `Set` to determine `setSum`. Thus this
solution still doesn't provide a qualifying solution.

Do not the important use of `long`s here. If these had been `int`s the
grader problems overflow and cause failures.

```java
class Solution {
    public int singleNumber(int[] nums) {
        long sum = 0;
        long setSum = 0;
        Set<Integer> s = new HashSet<>();
        for (int n : nums) {
            sum += n;
            if (s.add(n)) {
                setSum += n;
            }
        }

        return (int) ((3 * setSum - sum) / 2);
    }
}
```
