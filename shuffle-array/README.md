# Shuffle an Array

Given an integer array `nums`, design an algorithm to randomly shuffle the
array.

Implement the `Solution` class:

* `Solution(int[] nums)` Initializes the ojbect with the integer array nums.
* `int[] reset()` Resets the array to its original configuration and returns
   it.
* `int[] shuffle()` Returns a random shuffling of the array.

## Hints

1. There is very specific common algorithm for this that runs in O(n) time.
   You really should know it.

## Solutions

### Fisher Yates shuffle

Just a basic Fisher Yates shuffle.

```java
class Solution {
    private int[] nums;
    private Random rand = new Random();

    public Solution(int[] nums) {
        this.nums = nums;
    }

    /** Resets the array to its original configuration and return it. */
    public int[] reset() {
        return this.nums;
    }

    /** Returns a random shuffling of the array. */
    public int[] shuffle() {
        int[] shuffled = new int[nums.length];
        System.arraycopy(this.nums, 0, shuffled, 0, nums.length);
        for (int i = 0; i < shuffled.length - 1; i++) {
            int pick = rand.nextInt(shuffled.length - i) + i;
            int temp = shuffled[i];
            shuffled[i] = shuffled[pick];
            shuffled[pick] = temp;
        }
        return shuffled;
    }
}
```
