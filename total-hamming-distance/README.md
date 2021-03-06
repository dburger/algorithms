# Total Hamming Distance

The **Hamming distance** between two integers is the number of positions at
which the corresponding bits are different.

Write a function to determine the total Hamming distance between all pairs
of given numbers.

## Hints

1. Brute force is rather simple to do but requires you to understand two bit
   manipulation concepts:
   * Determine bits that are different
   * Count bits
1. To get beyond brute force a leap is required. Think about how you could form
   a solution by starting with counting how many numbers have each bit.

## Solutions

### Brute force

Brute force is rather straight forward. Note the xor here to reduce to
just the different bits and the `countBits` counting technique. Note that this
implementation, while correct, will get **time limit exceeded** on leeter.

```java
class Solution {
    public int totalHammingDistance(int[] nums) {
      int sum = 0;
      for (int i = 0; i < nums.length; i++) {
        for (int j = i + 1; j < nums.length; j++) {
          sum += countBits(nums[i] ^ nums[j]);
        }
      }
      return sum;
    }

    private int countBits(int n) {
      int i = 0;
      while (n != 0) {
        n = n & (n - 1);
        i++;
      }
      return i;
    }
}
```

### Counting bits

The counting bits approach is pretty easy once you see the possibility.
By counting the numbers that have each bit, then each bits contribution
to the sum is `cnt * (nums.length - cnt)`.  This is because for each
number that has the bit it contributes 1 for each number that doesn't
have the bit.

Note that this solution collapses what might be easier to read as two
separate loops into one. This can be done because as soon as we are
finished with a bit we can add in its contribution to the sum.

```java
class Solution {
    public int totalHammingDistance(int[] nums) {
        int len = nums.length;
        int sum = 0;
        for (int i = 0; i < 32; i++) {
          int mask = 1 << i;
          int b = 0;
          for (int j = 0; j < len; j++) {
            if ((nums[j] & mask) > 0) {
              b++;
            }
          }
          sum = sum + b * (len - b);
        }
        return sum;
    }
}
```
