# Additive Number

Additive number is a string whose digits can form additive sequence.

A valid additive sequence must contain **at least** three numbers. Except for
the first two numbers, each subsequent number in the sequence must be the sum of
the preceding two.

Given a string containing only digits `'0' - '9'`, write a function to determine
if it's an additive number.

**Note:** Numbers in the additive sequence **cannot** have leading zeroes, so
sequence `1, 2, 03` or `1, 02, 3` is invalid.

## Hints

1. An obvious strategy here is to consume the front end of the string and
   recurse on the remaining, backtracking as needed.

## Solutions

### Recursion with backtracking

This solution follows the hint above. A number is extracted from a prefix of the
string. It is then checked as to whether or not it is a suitable candidate to
form an addditive number. It is suitable if we don't have 2 numbers in the path
yet, or if it is the sum of the last two numbers in the path. This result is
then recursed upon with the remaining part of the input string `num`. If we hit
the end of the string and find that we have at least 3 numbers in the path, we
are done and have succeeded in finding an additive numbers. If the recursive
call fails, we backtrack by taking the last number we added to the path off the
path, and then trying again with the next longer prefix.

```java
import java.math.BigInteger;

class Solution {
    public boolean isAdditiveNumber(String num) {
        return ian(num, 0, new ArrayList<>());
    }

    private boolean ian(String num, int index, ArrayList<BigInteger> path) {
        if (index == num.length()) {
            return path.size() > 2;
        }

        for (int i = index + 1; i <= num.length(); i++) {
            String part = num.substring(index, i);
            if (part.length() > 1 && part.charAt(0) == '0') {
                return false;
            }
            BigInteger val = new BigInteger(part);
            int size = path.size();
            if (size < 2 || path.get(size - 2).add(path.get(size - 1)).equals(val)) {
                path.add(val);
                boolean result = ian(num, i, path);
                if (result) {
                    return true;
                }
                path.remove(size);
            }
        }
        return false;
    }
}
```
