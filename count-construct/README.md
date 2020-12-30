# Count Construct

Write a function `countConstruct(target, wordBank)` that accepts a `target`
string and an array of strings. The function should return the
**number of ways** that the `target` can be constructed by concatenating
elements of the `wordBank` array. You may reuse elements of `wordBank` as
many times as needed.

## Hints

1. Recursion that attempts to reduce target by peeling off a matching prefix is
   one obvious way to go. In this case, counts of matches must be aggregated.

## Solutions

### Recursive target reduction

This follows the hint and provides a solution that reduces the target by
matching a prefix and then recurses on the reduces string. Note that in this
implementation we actually don't reduce `target` but instead pass an advancing
`index` starting position. This avoids string manipulations that would damage
the running time of the algorithm.

```java
class Solution {
  public int countConstruct(String target, String[] wordBank) {
    return countConstruct(target, 0, wordBank);
  }

  public int countConstruct(String target, int index, String[] wordBank) {
    if (index == target.length()) {
      return 1;
    }

    int result = 0;

    for (String w : wordBank) {
      if (target.indexOf(w, index) == index) {
        result += countConstruct(target, index + w.length(), wordBank);
      }
    }

    return result;
  }
}
```

### Memoized recursive target reduction

This is the same solution as above with a memoizer thrown in.

```java
class Solution {
    public int countConstruct(String target, String[] wordBank) {
        return countConstruct(target, 0, wordBank, new HashMap<>());
    }

    public int countConstruct(String target, int index, String[] wordBank, Map<Integer, Integer> memo) {
        if (memo.containsKey(index)) {
            return memo.get(index);
        } else if (index == target.length()) {
            return 1;
        }

        int result = 0;

        for (String w : wordBank) {
            if (target.indexOf(w, index) == index) {
                result += countConstruct(target, index + w.length(), wordBank, memo);
            }
        }

        memo.put(index, result);
        return result;
    }
}
```
