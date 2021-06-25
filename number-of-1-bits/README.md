# Number of 1 Bits

Write a function that takes an integer and returns the number of '1' bits it has (also known
as the *Hamming weight*).

## Hints

1. Shift a one bit across testing at each position.
1. Shift `n` across testing against 1 while `n` is still non-zero.
1. Magic bit fiddling knowledge.

## Solutions

### Shifting 1 from left to right

Solution has O(1) time and space complexity. In practice this is slower than the following
solutions because it looks at all 32 positions.

```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int count = 0;
        int bit = 1;
        for (int i = 0; i < 32; i++) {
            // Careful, this is unsigned int, can't use > 0.
            if (((bit << i) & n) != 0) {
                count++;
            }
        }
        return count;
    }
}
```

### Shifting n

Solution has O(1) time and space complexity. This solution stops after it processes the highest
bit, thus making it faster than the prior solution in practice.

```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int count = 0;
        // Careful, n is signed int, must use != 0.
        while (n != 0) {
            if ((n & 1) != 0) {
                count++;
            }
            n >>>= 1;
        }
        return count;
    }
}
```

### Magic bit fiddlers

Solution has O(1) time and space complexity. This is faster than the prior solutions in practice,
as it eliminates a 1 bit in each iteration. Thus the iterations match the number of bits and no
more.

```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int count = 0;
        // Careful, must be != 0 java using signed int.
        while (n != 0) {
            count++;
            n &= (n - 1);
        }
        return count;
    }
}
```
