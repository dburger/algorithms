# Pascal's Triangle II

Given an integer `rowIndex`, return the rowIndex row of the Pascal's triangle.

## Hints

1. This is a rather straight forward problem if you know about the relationship
   between Pascal's triangle and combinations. Problem is, any usage of this
   approach given the tests on leeter will result in overflows that will fail
   the test. So one way around this is by using `BigInteger` in java.
2. The obvious approach with `BigInteger` described above will work, but will
   look miserable in the leeter grader. Instead of calculating each item in the
   row look at the relationship between adjacent entries.

## Solutions

### Brute with BigInteger

This approach follows the first hint. Due to the calculation of the factorials,
this solution is quite slow compared to alternatives. Note that the rows of
Pascal's table are symmetric, and this solution does only compute half of the
row and fills in both halves at the same time.

The slowness comes from the computation of the factorials in the combinations
equation. With `n` as the value of `rowIndex`, each entry does an O(n)
computation of the factorial. This is done `n` times. Thus the time complexity
is O(n^2).

It is possible to make this computation leaner by factoring the problem.
Specifically, in the combinations formula:

```
C(n, k) = n! / (k! * (n - k)!)
```

The `n!` and `(n - k)!` pieces can be factored leaving this as:

```
C(n, k) = (n * (n - 1) * (n - 2) * ... (k + 1)) / k!
```

While this is a nice little tweak that speeds things up slightly, in practice it
does not change the time complexity.

```java
class Solution {
    public List<Integer> getRow(int rowIndex) {
        Integer[] result = new Integer[rowIndex + 1];
        int start = 0;
        int end = rowIndex;
        result[start++] = result[end--] = 1;
        while (start <= end) {
            result[start] = result[end] = combs(rowIndex, start);
            start++;
            end--;
        }
        return Arrays.asList(result);
    }

    private int combs(int n, int k) {
        BigInteger numerator = fact(n);
        BigInteger denominator = fact(k).multiply(fact(n - k));
        return numerator.divide(denominator).intValue();
    }

    private BigInteger fact(int n) {
        BigInteger result = BigInteger.ONE;
        while (n > 1) {
            result = result.multiply(BigInteger.valueOf(n--));
        }
        return result;
    }
}

```

### Relationship between entries

For this solution we look at the relationship between entries to avoid computing
the combinations from scratch. For example, say for the 3rd of the triangle we
would have the entries:

```
[C(3, 0), C(3, 1), C(3, 2), C(3, 3)]
```

Let's take a look at the actual computations involved for the first two entries,
they are:

```
3! / (0! * (3 - 0)! == 3! / (0! * 3!)
```

and

```
3! / (1! * (3 - 1)!) == 3! / (1! * 2!)
```

As you can see in the example, the numerator remains unchanged. The denominator
is where the changes occur. In the `k!` piece, each new entry introduces a next
higher factor. In the `(n - k)!` piece, each new entry loses its high factor.
For position i of the row, this is equivalent to multiplying the prior entry by:

```
(n + 1 - i) / i
```

The numerator performs the function of the `(n - k)!` piece losing its high
factor. The denominator performs the function of the `k!` piece introducing its
next high factor.

This solution has time and space complexity O(n).

```java
import java.math.BigInteger;

class Solution {
    public List<Integer> getRow(int rowIndex) {
        Integer[] result = new Integer[rowIndex + 1];
        int start = 0;
        int end = rowIndex;
        result[start++] = result[end--] = 1;
        int prior = 1;
        while (start <= end) {
            // Note the casts here to prevent overflow, and then back to int.
            int next = (int) ((long) prior * (rowIndex + 1 - start) / start);
            result[start++] = result[end--] = next;
            prior = next;
        }
        return Arrays.asList(result);
    }
}
```
