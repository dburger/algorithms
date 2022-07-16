# Count Primes

Given an integer `n`, return the number of primes that are strictly less than
`n`.

## Hints

1. Get a sieve. It will be fun.

## Solutions

### Sieve of Eratosthenes

This problem is easily solved with the
[Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes)
approach. In this approach we use an array to mark whether or not a given
number is prime. When we encounter a prime we count it, and we mark all of
its multiples as not being prime.

The only tricky parts to this problem are some optimization and avoiding
overflow. An optimization can be had when marking the multiples of prime.
If a value `p` is prime then the next multiple of `p` to mark is `p * p`.
Why isn't it `2 * p` or any multiple of `p` less than `p * p`. Well, it is
simply because that other value would need to be less than `p` to be less
than `p * p` and we have already marked the multiples for values less than
`p`. Overflow is avoided by marking the boundary where the last prime could
potentially need to mark its multiples. By not descending into the marking
of multiples when we cross this boundary we avoid overflowing the `int` as
well.

```java
class Solution {
    public int countPrimes(int n) {
        // We'll use false to mean prime, true not prime,
        // then we don't have to initialize this further.
        boolean[] sieve = new boolean[n];
        int count = 0;
        double boundary = Math.sqrt(n);
        for (int i = 2; i < n; i++) {
            if (!sieve[i]) {
                count++;
                // If i is greater than boundary, the i * i is greater
                // than n, however, it may also overflow. This check
                // avoids that overflow.
                if (i > boundary) {
                    continue;
                }
                for (int j = i * i; j < n; j += i) {
                    // A multiple of a prime, mark not prime.
                    sieve[j] = true;
                }
            }
        }
        return count;
    }
}
```
