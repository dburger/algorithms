# Missing Number

Given an array `nums` containing `n` distinct numbers in the range `[0, n]`,
return the only number in the range that is missing from the array.

**Follow up:** Could you implement a solution using only O(1) extra space
and O(n) runtime complexity?

## Hints

1. The wording on this is a little weird. It boils down to the range of numbers
   that can be used is `[0, n]`, which is `n + 1` numbers, but the array is only
   size `n`. You could use another array to mark what you have.
1. Think about how some math can solve this.

## Solutions

Sorting and then iterating to the missing element is also an easy solution here.
Because it has worse time complexity that these given solutions I have not
included it here.

### Mark and iterate

Here we merely use another array to mark what is there, and then use an
iteration to return the index of what is missing.

This solution has O(n) time and space complexity.

```java
class Solution {
    public int missingNumber(int[] nums) {
        int[] record = new int[nums.length + 1];
        for (int n : nums) {
            record[n] = 1;
        }
        for (int i = 0; i < record.length; i++) {
            if (record[i] == 0) return i;
        }
        return -1;
    }
}
```

## Use the maths

Here we use the formula for the sum of the firt n numbers, `(n * (n + 1)) / 2`,
to get the value for what the total would be if all the numbers were there.
The subtracting the actual sum gives the missing number.

This solution is O(n) time complexity and O(1) space complexity.

```java
class Solution {
    public int missingNumber(int[] nums) {
        int sum = 0;
        for (int n : nums) {
            sum += n;
        }
        int n = nums.length;
        return (n * (n + 1)) / 2 - sum;
    }
}
```

## Use the maths part two

More magic of math. xor is commutative and associative. Therefore, if you merely
start with `n` and then xor that with each element and its index, the missing
number will be left over.

```java
class Solution {
    public int missingNumber(int[] nums) {
        int val = nums.length;;
        for (int i = 0; i < nums.length; i++) {
            val = val ^ i ^ nums[i];
        }
        return val;
    }
}
```
