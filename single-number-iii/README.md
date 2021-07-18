# Single Number III

Given an integer array `nums`, in which exactly two elements appear only once
and all the other elements appear exactly twice. Find the two elements that
appear only once. You can return the answer in **any order**.

You must write an algorithm that runs in linear runtime complexity and uses
only constant extra space.

## Hints

1. Are we back to xor bit fiddling again due to the "only constant extra
space"?

## Solutions

### Sets, non-compliant

This solution merely uses a `Set` to track if it has seen a number before.
Those seen the second time are removed. Those only seen one time remain.

The time complexity of this algorithm is O(n). The space complexity of this
algorithm is also O(n). Therefore it does not qualify as an official solution
due to the constaint of "only constant extra space."

```java
class Solution {
    public int[] singleNumber(int[] nums) {
        Set<Integer> once = new HashSet<>();
        for (int n : nums) {
            if (!once.add(n)) {
                once.remove(n);
            }
        }
        int[] result = new int[once.size()];
        int i = 0;
        for (int n : once) {
            result[i++] = n;
        }
        return result;
    }
}
```

### TODO: bit fiddling magic
