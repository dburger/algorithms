# Add Digits

Given an integer `num`, repeatedly add all its digits until the result has
only one digit, and return it.

## Hints

1. Straightforward should work, but make sure you understand the problem.
1. Mathematical tricks?

## Solutions

### As specified my lord

This solution just executes exactly as the problem is described. A helper
function `sum` is added to produce the summation of the digits. This function
is repeatedly called until the result produced is less than 10.

The space complexity of this solution is O(1). That is the only space allocated
is for the stack frames to invoke `sum` and the `int` to keep track of the new
sum. This does not vary with the size of the input. The time complexity is a
bit more tricky and is O(log(n)).

```java
class Solution {
    public int addDigits(int num) {
        while (num > 9) {
            num = sum(num);
        }
        return num;
    }

    private int sum(int num) {
        int sum = 0;
        while (num != 0) {
            sum += num % 10;
            num /= 10;
        }
        return sum;
    }
}
```

### Math nerds apply

Digital sums cycle around with each additional unit. For example:

- `10 == 1`
- `11 == 2`
- ...
- `56 == 2`
- `57 == 3`

The reason is fairly obvious as each additional unit adds one to the
subsequent sum that cascades down to the result. This value rolls over when
the digital root goes from `9` to `1`. Since any number where the digits add
to `9` is divisible by 9, we can use this trick to get an O(1) time complexity
and space complexity solution:

```java
class Solution {
    public int addDigits(int num) {
        if (num == 0) {
            return 0;
        } else if (num % 9 == 0) {
            return 9;
        } else {
            return num % 9;
        }
    }
}
```
