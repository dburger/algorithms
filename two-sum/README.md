# Two Sum

Given an array of integers `nums` and an integer `target`, return indices of the
two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not
use the same element twice.

You can return the answer in any order.

## Hints

1. A brute force solution could iterate all pairs and check.
   This would be O(n^2).
1. A more efficient O(n) solution could map the integers and thus
   efficiently check for the complement.

## Solutions

### Brute force

The brute force solution is O(n^2). This is not a good final solution
for an interview.

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] + nums[j] == target) {
                    return new int[] {i, j};
                }
            }
        }
        return new int[2];
    }
}
```

### Map solution

This map solution is O(n).

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> m = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int val = nums[i];
            int complement = target - val;
            if (m.containsKey(complement)) {
                return new int[] {i, m.get(complement)};
            } else {
                m.put(val, i);
            }
        }
        return new int[2];
    }
}
```
