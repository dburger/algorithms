# Minimum Operations to Make Array Equal

You have an array `arr` of length `n` where `arr[i] = (2 * i) + 1` for all valid
values of `i` (i.e `0 <= i < n`).

In one operation, you can select two indices `x` and `y` where `0 <= x, y < n`
and substract `1` from `arr[x]` and add `1` to `arr[y]` (i.e. perform
`arr[x] -= 1` and `arr[y] += 1`). The goal is to make all of the elements of the
array **equal**. It is **gauranteed** that all of the elements of the array can
be made equal using some operations.

Given an integer `n`, the length of the array, return the *minimum number of
operations* needed to make all the elements of `arr` equal.

## Hints

1. Think of what it takes to take each element up to the same value. You could
   iterate on making the necessary changes to elements.
1. There is also a pure math approach. Think in terms of the total you have in
   a half to start with and what it needs to get to.

## Solutions

### Iterate and win

The sum of the first `n` odd integers in `n^2`. This makes the mean value `n`.
If we look at only the first half of the numbers, each one of those numbers
need to be brought up to `n`. In doing so, per the operations, a number in
the second half of the numbers can be brought down to `n`. Here we iterate
on that first half adding in the number of operations necessary to bring
that value up to n.

The time complexity of this solution is O(n). Thee space complexity is O(1).

```java
class Solution {
    public int minOperations(int n) {
        int mid = n / 2;
        int ops = 0;
        for (int i = 0; i < mid; i++) {
            // Adding in the ops to take the ith element up to n.
            ops += n - (2 * i + 1);
        }
        return ops;
    }
}
```

### Do the math

This one is pure math and it is very nice. The sum of the first `n` odd numbers
is `n^2`. This makes the mean of the `n` numbers `n^2 / n == n`. This is the
target number we want to bring all the numbers to. The sum of the `n/2` elements
in the first half of the numbers is `(n/2)^2 == n^2/4`. If these were all
brought up to `n` their sum would be `(n/2)*n == n^2/2` Thus the number of
operations required is equal to the difference
`n^2/2 - n^2/4 == 2*n^2/4 - n^2/4 == n^2/4`. In bringing up the first half with
`+1` operations, we pair with the last halfs `-1` operations. So these operations
calculated for the first half of the list are all that is needed.

This solution is O(1) time and space complexity.

```java
class Solution {
    public int minOperations(int n) {
        return (n * n) / 4;
    }
}
```
