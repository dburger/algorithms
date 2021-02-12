# Palindrome Number

Given an integer `x`, return `true` if `x` is a palindrome integer.

An integer is a **palindrome** when it reads the same backward as forward. For
example, `121` is a palindrome while `123` is not.

**Follow up:** Could you solve it wihtout converting the integer to a string.

## Hints

1. The converting to string approach should be rather trivial.
1. Per the follow up, if you strip off one digit from the tail at a time, and
   accumulate the partial number, how could you tell when it matched?

## Solutions

### Converting to string

For completeness, the trivial convert to string approach.

The time complexity here is O(n). The space complexity, due to converting to a
string, is also O(n).

```java
class Solution {
    public boolean isPalindrome(int x) {
        // No palindrome if negative.
        if (x < 0) return false;
        // Is palindrome if 0-9.
        if (x < 10) return true;
        // No palindrome if trailing zero.
        if (x % 10 == 0) return false;
        String value = String.valueOf(x);
        int mid = value.length() / 2;
        int last = value.length() - 1;
        for (int i = 0; i < mid; i++) {
            if (value.charAt(i) != value.charAt(last - i)) return false;
        }
        return true;
    }
}
```

### Converting to string with alternative index handling approach

This is the same solution as above, with the index crossover handled
differently. This is good for folks that are confused on what `mid` should
be above. Here we avoid that conversation completely by just tracking `start`
and `end` and terminate if `start >= end`.

The time and space complexity follow as above.

```java
class Solution {
    public boolean isPalindrome(int x) {
        // No palindrome if negative.
        if (x < 0) return false;
        // Is palindrome if 0-9.
        if (x < 10) return true;
        // No palindrome if trailing zero.
        if (x % 10 == 0) return false;
        String value = String.valueOf(x);
        int start = 0;
        int end = value.length() - 1;
        while (start < end) {
            if (value.charAt(start++) != value.charAt(end--)) return false;
        }
        return true;
    }
}
```

### Strip, Horner's and check

Ok, this is clever as hell. Here we take care of the initial edge cases as
above. After that, we strip off the trailing digits and use
[Horner's method](https://en.wikipedia.org/wiki/Horner%27s_method)
to accumulate the reversed partial. We can jump out of the loop when `reversed`
is no longer less than what is left of the stripped input `x`. At that point,
if the number of digits in the original `x` was even, remaining `x` will equal
`reversed`. If the number of digits in the original `x` was odd, the comparison
can be made against `reversed / 10`, removing the middle digit.

The time complexity for this approach remains O(n), but the space complexity is
O(1).

```java
class Solution {
    public boolean isPalindrome(int x) {
        // No palindrome if negative.
        if (x < 0) return false;
        // Is palindrome if 0-9.
        if (x < 10) return true;
        // No palindrome if trailing zero.
        if (x % 10 == 0) return false;

        int reversed = 0;
        while (x > 0 && reversed < x) {
            int digit = x % 10;
            x /= 10;
            reversed = reversed * 10 + digit;
        }

        return reversed == x || reversed / 10 == x;
    }
}
```
