# Contains Duplicates

Given an integer array `nums`, return `true` if any value appears **at least
twice** in the array, and return `false` if every element is distinct.

## Hints

1. Hmmm, too easy for O(n).

## Solutions

### Set and forget

Here we merely use a `Set` to detect duplicates.

This solution has a time complexity of O(n) to go through `nums`. The space
complexity is also O(n) to hold the duplicate detecting `Set`.

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> s = new HashSet<>();
        for (int n : nums) {
            if (!s.add(n)) {
                return true;
            }
        }
        return false;
    }
}
```
