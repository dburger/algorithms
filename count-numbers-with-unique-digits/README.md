# Count Numbers with Unique Digits

Given a **non-negative** integer n, count all numbers with unique digits x,
where 0 <= x <= 10^n.

## Hints

1. Think in terms of a number with n digits. How many ways are there to pick
   the first digit, the second digit, ..., the nth digit. In this way you can
   develop an answer using combinatorics.
1. Going along with the hint above, `10^n` will include numbers with n digits,
   n - 1 digits, down to 1. Recursion may help here.

## Solutions

### Recursive combinatorics

This solution follows the hint and uses combinatorics to compute an answer.
To see how it works consider an invocation for `n == 4`. In that case the call
first computes the result for 4 digit numbers, that is from 1000 to 9999. The
computation is `9 * 9 * 8 * 7`. That is there are 9 ways to choose the first
digit (1-9), 9 ways to choose the second digit (all of 0-9 except first digit),
8 ways to choose the third digit (all of 0-9 except first and second digits),
and 7 ways to choose the last digit (all of 0-9 except first three digits).
The recursive call is then invoked for `n == 3`.

```java
class Solution {
    public int countNumbersWithUniqueDigits(int n) {
        // 10^n has a run of n digit numbers. For example 10^2
        // has numbers from 10 to 99. Here we count those numbers
        // and use recursion to count the result.
        int result = 1;
        // First digit can be 9 different, not 0.
        int mult = 9;
        for (int i = 0; i < n; i++) {
            // Second digit can't be the same as the first digit,
            // but can be zero, thus we stay at 9 for first
            // two rounds.
            if (i > 1) {
                mult--;
            }
            result *= mult;
        }
        // Recursion counts the numbers for n - 1 digits.
        return n == 0 ? result : result + countNumbersWithUniqueDigits(n - 1);
    }
}
```

### Iterative combinatorics

This is the same as the above solution but uses iteration instead of recursion.

```java
class Solution {
    public int countNumbersWithUniqueDigits(int n) {
        // 10^n has a run of n digit numbers. For example 10^2
        // has numbers from 10 to 99. Here we count those numbers
        // and use a loop to sum the result.
        int result = 1;
        while (n > 0) {
            // First digit can be 9 different, not 0.
            int mult = 9;
            int nres = 1;
            for (int i = 0; i < n; i++) {
                // Second digit can't be the same as the first digit,
                // but can be zero, thus we stay at 9 for the first
                // two rounds.
                if (i > 1) {
                    mult--;
                }
                nres *= mult;
            }
            result += nres;
            n--;
        }
        return result;
    }
}
```
