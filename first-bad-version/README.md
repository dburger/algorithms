# First Bad Version

You are a product manager and currently leading a team to develop a new product.
Unfortnately, the latest version of your product fails the quality check. Since
each version is developed based on the prevoius version, all the versions after
a bad version are also bad.

Suppose you have `n` versions `[1, 2, ..., n]` and you want to find out the
first bad one, which causes all of the following ones to be bad.

You are given an API `bool isBadVersion(version)` which returns whether
`version` is bad. Implement a function to find the first bad version. You should
minimize the number of calls to the API.

## Hints

1. I mean if some kind of modified binary search doesn't come to mind it should.

## Solutions

### Binary search away

A modified binary search is implemented here. If the midpoint is bad we continue
to the left to see if we can find one lower. If the midpoint is not bad we know
that the starting point for bad versions must be to the right. If we our new
`lo` is higher than `lowest` we know there is no reason to continue the
algorithm.

The time complexity of this solution is `O(log(n))`. The space complexity is
`O(1)`.

```java
public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int lo = 1;
        int hi = n;
        int lowest = Integer.MAX_VALUE;
        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            if (isBadVersion(mid)) {
                lowest = mid;
                hi = mid - 1;
            } else {
                lo = mid + 1;
                if (lo > lowest) {
                    break;
                }
            }
        }
        return lowest;
    }
}
```
