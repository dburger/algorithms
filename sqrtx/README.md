# Sqrt(x)

Given a non-negative integer `x`, compute and return the square root of x.

Since the return type is an integer, the decimal digits are **truncated**,
and only **the integer part** of the result is returned.

## Hints

1. Since you are probably not supposed to use the built in math functions,
   you could attempt to binary search to the solution. The complexity left
   is that you need to return the floor of the result.

## Solutions

### Binary search

Note the only setting `ans` if we are below `x`. Also note the usage of
`long` here, to avoid overflow problems.

```java
class Solution {
    public int mySqrt(int x) {
        if (x == 0 || x == 1) {
          return x;
        }
        long lo = 1;
        long hi = x;
        long ans = 0;
        while (lo <= hi) {
          long mid = lo + (hi - lo) / 2;
          long squared = mid * mid;
          if (squared == x) {
            return (int) mid;
          } else if (squared > x) {
            hi = mid - 1;
          } else {
            lo = mid + 1;
            // We can only use answer if we were lower, as the problem
            // specifies to truncate the result.
            ans = mid;
          }
        }
        return (int) ans;
    }
}
```
