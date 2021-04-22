# Single Number

Given a **non-empty** array of integers `nums`, every element appears *twice*
except for one. Find that single one.

**Follow up:** Could you implement a solution with a linear runtime complexity
and without using extra memory?

## Hints

1. Solutions that count via a multiset like approach work.
1. There are solutions that track sums that work.
1. There is a wizard approach using operators.

## Solutions

### Multiset

Here we use a multiset approach to just count each integer in a first pass,
and then identify the element with a count of `1` in the second pass. Note
the usage of the `counts.put(n, counts.getOrDefault(n, 0) + 1);` to get the
map to easily function as a
[Multiset](https://guava.dev/releases/18.0/api/docs/com/google/common/collect/Multiset.html). This is a very common technique in these problems.

The time complexity of this solution is O(n) as it makes two passes through
the list. The space complexity is O(n) to hold the counts.

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

### Set counting

Sets can be used to get something similar to the above without the explicit
counting. Here, when each number is encountered, it is either put in the set
if it is not present or removed if it is. The single number in the set will
be the answer.

The time and space complexity of this solution is O(n).

```java
class Solution {
    public int singleNumber(int[] nums) {
        Set<Integer> s = new HashSet<>();
        for (int n : nums) {
            if (s.contains(n)) {
                s.remove(n);
            } else {
                s.add(n);
            }
        }
        return s.iterator().next();
    }
}
```

### Tracking totals

In this solution we use a set to help us keep two running totals as we iterate
through the numbers. In the `sum` variable we keep track of the total. In the
`setSum` variable we keep track of the sum of the unique numbers added to the
set. At the end we can merely take the overall twice times the `setSum` and
subtract the `sum`. The value we be the element that was not repeated twice.

The time complexity and space complexity of this solution is O(n).

```java
class Solution {
    public int singleNumber(int[] nums) {
        int sum = 0;
        int setSum = 0;
        Set<Integer> s = new HashSet<>();
        for (int n : nums) {
            sum += n;
            if (s.add(n)) {
                setSum += n;
            }
        }
        return 2 * setSum - sum;
    }
}
```

### Wizard xor

Aha, use some math! If you merely xor all the numbers together the value at the
end will be the number that was not repeated. All of the other numbers we
"cancel each other out."

The time complexity of this solution is O(n) as we make a pass through the
input `nums`. The space complexity of this solution is O(1) as we only use
a single `int` to accumulate the `result`. In practice this algorithm, while
still O(n), kicks the pants off the above solutions (in a relative way :-)).

```java
class Solution {
    public int singleNumber(int[] nums) {
        int result = nums[0];
        for (int i = 1; i < nums.length; i++) {
            result ^= nums[i];
        }
        return result;
    }
}
```
