# Find Kth Bit in Nth Binary String

Given two positive integers `n` and `k`, the binary string `sn` is formed as
follows:

- `s1 = "0"`
- `s2 = s1 + "1" + reverse(invert(s1))`
- ...

## Hints

1. Brute actually doesn't time out.
2. Oh my, right or left of the middle?

## Solutions

### You brute.

You can brute force this fairly easily. By using a `StringBuilder` you keep
string instantiation to a minimum. Note how the reverse and invert bits is
done in a single loop that walks through the previous string backwards and
flips the bits.

The time complexity of this solution is 2 TODO(dburger). The space complexity is O(2^n).

```java
class Solution {
    public char findKthBit(int n, int k) {
        StringBuilder buf = new StringBuilder();
        buf.append("0");
        for (int i = 2; i <= n; i++) {
            int end = buf.length() - 1;
            buf.append("1");
            for (int j = end; j > -1; j--) {
                buf.append(buf.charAt(j) == '1' ? '0' : '1');
            }
        }
        return buf.charAt(k  - 1);
    }
}
```

### Magic of recursion

The above solution is just a pure implementation of the description of how the
subsequent strings are formed. With no "magic", we can't expect it to naturally
be performant. With the magic of recursion we can implement a fairly simple
algorithm that greatly speeds it up.

This algorithm works by first checking if we are at the base case of `n == 1`.
If we are, we can simply return that string. If we are not, we calculate what
the middle point is given the `n`. If `k` corresponds to that middle point,
we are done, we can return `'1'`. If `k < m`, we recurse on the front without
the need to change `k`. That is because the front half of each string is equal
to the prior string. If `k > m`, we recurse on the tail with an adjustment of
`k`. `k` must be adjusted because for each string the back half is the prior
string reversed. Likewise, the bit must be flipped.

The time complexity of this solution is O(n). This is because `n` is reduced by
one at worst in each recursive call until it reaches the base case of `1`.
Likewise, the space complexity is also O(n) to hold the stack frames in these
recursive calls.

```java
class Solution {
    public char findKthBit(int n, int k) {
        if (n == 1) {
            return '0';
        }

        int m = (int) Math.pow(2, n - 1);

        if (k == m) {
            return '1';
        } else if (k < m) {
            return findKthBit(n - 1, k);
        } else {
            return findKthBit(n - 1, m - (k - m)) == '1' ? '0' : '1';
        }
    }
}
```
