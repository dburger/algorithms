# Random Pick Index

Given an array of integers with possible duplicate, randomly output the index
of a given target number. You can assume that the given target number must exist
in the array.

**Note:**

The array size can be very large. Solutions that use too much space will not
pass the judge.

This is listed as a medium in leeter. I would say this is wrong. This should
be categorized as easy.

## Hints

1. If you record the locations of the target in an array then a simple random
   number choice can be used to translate to the index in the input array.

## Solutions

### Index translation

This solution follows the suggestion in the first hint. The time and space
complexity here are both O(n). There are 2 O(n) passes through the array
to first count the number of items matching target and then to fill their
values into another array. The space complexity is O(n) to hold up to the
n indices for thee items matching target.

```java
class Solution {
    private int[] nums;
    private Random random = new Random();

    public Solution(int[] nums) {
        this.nums = nums;
    }

    public int pick(int target) {
        int count = 0;
        for (int i = 0; i < this.nums.length; i++) {
            if (this.nums[i] == target) count++;
        }
        int fill = 0;
        int[] indices = new int[count];
        for (int i = 0; i < this.nums.length; i++) {
            if (this.nums[i] == target) indices[fill++] = i;
        }
        int pick = random.nextInt(count);
        return indices[pick];
    }
}
```

### Index translation with speed tweak

This solution is similar to the above and has the same O(n) time complexity.
The space complexity here is O(1), however, as it uses the second pass to
advance to and then return the correct index. In practice it should be slightly
faster than the prior solution as it will usually not make the entire second
pass.

```java
class Solution {
    private int[] nums;
    private Random random = new Random();

    public Solution(int[] nums) {
        this.nums = nums;
    }

    public int pick(int target) {
        int count = 0;
        for (int i = 0; i < this.nums.length; i++) {
            if (this.nums[i] == target) count++;
        }
        int pick = random.nextInt(count) + 1;
        for (int i = 0; i < this.nums.length; i++) {
            if (this.nums[i] == target) {
                if (--pick == 0) return i;
            }
        }

        // This is for the compiler, it won't get here.
        return -1;
    }
}
```
