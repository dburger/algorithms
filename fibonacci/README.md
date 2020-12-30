# Fibonacci

Implement a function to return
[Fibonacci numbers](https://en.wikipedia.org/wiki/Fibonacci_number). Here
following more modern conventions we define F(0) == 0, F(1) == 1.

## Hints

1. This is like intro to recursion and dynamic programming 101.

## Solutions

### Brute force recursion

```java
class Solution {
    public int fibonacci(int i) {
        return i < 2 ? i : fibonacci(i - 1) + fibonacci(i - 2);
    }
}
```

### Memoized recursion

```java
class Solution {
    public int fibonacci(int i) {
        Map<Integer, Integer> memo = new HashMap<>();
        return fibonacci(i, memo);
    }

    public int fibonacci(int i, Map<Integer, Integer> memo) {
        if (memo.containsKey(i)) {
            return memo.get(i);
        } else if (i < 2) {
            return i;
        } else {
            int result = fibonacci(i - 1, memo) + fibonacci(i - 2, memo);
            memo.put(i, result);
            return result;
        }
    }
}
```

### Dynamic programming table

```java
class Solution {
    public int fibonacci(int i) {
        if (i < 2) {
            return i;
        }
        int[] table = new int[i + 1];
        table[0] = 0;
        table[1] = 1;
        for (int j = 2; j <= i; j++) {
            table[j] = table[j - 1] + table[j - 2];
        }
        return table[i];
    }
}
```
