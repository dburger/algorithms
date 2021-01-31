# Valid Perfect Square

Given a **positive** integer `num`, write a function which returns True if
`num` is a perfect square else False.

**Follow up:** Do not use any built-in library function such as `sqrt`.

## Hints

1. Obviously solving without regards to the follow up should make this trivial.
1. Per the follow up, a brute force solution could just iterate over integers
   until the square hits `num` exactly or goes over. But would this solution
   pass the leeter timers?
1. So also with regards to the follow up, perhaps instead of brute forcing
   through every integer you could cut down on that search space. How would you
   do that?

## Solutions

### Using built ins

This is just for fun. Using built in functions makes this trivial and is not
what the purpose of the problem is. I show it here however for entertainment.

```java
class Solution {
    public boolean isPerfectSquare(int num) {
        double result = Math.sqrt(num);
        return result == Math.floor(result);
    }
}
```

### Brute force

Here we brute force by attempting to increment up until the solution is found
or we go over the target `num`. This is the solution implied from the second
hint.

This solution does not pass leeters time limit! This solution has time
complexity O(sqrt(n)) and space complexity O(1).

```java
class Solution {
    public boolean isPerfectSquare(int num) {
        if (num < 2) {
            return true;
        }
        int i = 2;
        int squared = i * i;
        while (squared < num) {
            i++;
            squared = i * i;
        }
        return squared == num;
    }
}
```

### Binary search to it.

OK, so the brute force approach up to the solution does not pass leeters time
limit. What can we do? Well, considering only perfect squares for a given `num`
greater than one it is clear that square root of that number lies somewhere
between 2 and `num / 2`. That we can binary search this range towards the
solution.

This solution hits the golden 0 ms time completion from the leeter grader. The
time complexity is O(log(sqrt(n)) and space complexity is O(1). Note that using
a recursive binary search would increase the time complexity to O(log(sqrt(n))
to hold the stack frames of the recursion.

Oh! Almost forgot. There is a nasty but common trick to getting this solution to
work. Note that although the input is of type `int` the `mid` and `sq` variables
used are of type `long`. This is because squaring proposed solutions can lead
to overflow and thus throw off the solution. Using longs for these variables
avoids the overflows.

```java
class Solution {
    public boolean isPerfectSquare(int num) {
        if (num < 2) {
            return true;
        }
        int lo = 2;
        int hi = num / 2;
        while (lo <= hi) {
            long mid = lo + (hi - lo) / 2;
            long sq = mid * mid;
            if (sq < num) {
                lo = (int) mid + 1;
            } else if (sq > num) {
                hi = (int) mid - 1;
            } else {
                return true;
            }
        }
        return false;
    }
}
```
