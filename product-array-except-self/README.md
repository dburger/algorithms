# Product of Array Except Self

Given an array `nums` of n integers where n > 1, return an array `output`
such that `output[i]` is equal to the product of all the elements of `nums`
except `nums[i]`.

**Constraint:** It's guaranteed that the product of the elements of any prefix
or suffix of the array (including the whole array) fits in a 32 bit integer.

**Note:** Please solve it **without division** and in O(n).

## Hints

1. The obvious way to solve it without division involves nested loops and is
   O(n^2). This is not a good interview solution but you may want to mention
   it.
2. The solution takes an insight - can you build up parts of the solution and
   put them together?

## Solutions

### Prefix and suffix arrays

The trick here is to make arrays that contain the multiplication before i and
after i, and then use them to construct the solution.

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;

        // Will hold all multiplications before i.
        int[] prefix = new int[n];
        prefix[0] = 1;
        for (int i = 1; i < n; i++) {
          prefix[i] = prefix[i - 1] * nums[i - 1];
        }

        // Will hold all multiplications after i.
        int[] suffix = new int[n];
        suffix[n - 1] = 1;
        for (int i = n - 2; i > -1; i--) {
          suffix[i] = suffix[i + 1] * nums[i + 1];
        }

        int result[] = new int[n];
        for (int i = 0; i < n; i++) {
          result[i] = prefix[i] * suffix[i];
        }

        return result;
    }
}
```

### Prefix array only

This solution is much like the one above but dispenses with the
suffix multiplication array. It is built on the fly.

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;

        // Will hold all multiplications before i.
        int[] prefix = new int[n];
        prefix[0] = 1;
        for (int i = 1; i < n; i++) {
            prefix[i] = prefix[i - 1] * nums[i - 1];
        }

        // Will hold all multiplications after i in loop below.
        int suffix = 1;

        int result[] = new int[n];
        result[n - 1] = prefix[n - 1];

        for (int i = n - 1; i > -1; i--) {
            if (i < n - 1) {
                  suffix *= nums[i + 1];
            }
            result[i] = prefix[i] * suffix;
        }
        return result;
    }
}
```
