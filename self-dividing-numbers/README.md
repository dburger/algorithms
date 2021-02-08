# Self Dividing Numbers

A self-dividing number is a number that is divisible by every digit it contains.

For example 128 is a self-dividing number because `128 % 1 == 0`,
`128 % 2 == 0`, and `128 % 8 == 0`.

Also, a self-dividing number is not allowed to contain the digit zero for
obvious reasons.

Given a lower and upper number bound, inclusive, output a list of every
self-dividing number.

## Hints

1. This one seems rather straightforward with a brute force approach. I don't
   know of any math tricks that could make this faster.

## Solutions

### Brute it up

This solution follows the obvious brute force approach. We iterate through the
numbers and for each one we check if it is self dividing. `0` is rejected,
numbers between `0` and `10` are accepted. For higher numbers we pull of the
digits one at a time and check for divisibility.

Where `n` is the number of numbers involved (`right - left + 1`), the space
complexity of this algorithm is O(n), if potentially all the numbers all
self-dividing. The time complexity is O(n * m) where `m` is the number of
digits in the largest number.

```java
class Solution {
    public List<Integer> selfDividingNumbers(int left, int right) {
        List<Integer> result = new ArrayList<>();
        for (int i = left; i <= right; i++) {
            if (selfDividing(i)) {
                result.add(i);
            }
        }
        return result;
    }

    private boolean selfDividing(int num) {
        if (num == 0) {
            return false;
        }
        if (num < 10) {
            return true;
        }
        int digits = num;
        while (digits > 0) {
            int digit = digits % 10;
            digits /= 10;
            if (digit == 0 || num % digit > 0) {
                return false;
            }
        }
        return true;
    }
}
```
