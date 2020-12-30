# All Construct

Write a function `allConstruct(target, wordBank)` that accepts a `target`
string and an array of strings. The function should return a 2D array
containing **all of the ways** that the `target` can be constructed by
concatenating elements of the `wordBank` array. Each element of the 2D
array should represent one combination that constructs the `target`. You
may reuse elements of `wordBank` as many times as needed.

## Hints

1. Recursion reduction of the `target` input by matching prefixes will do
   it. The "path" to the solution must be tracked and an accumlator added
   to when a base case is achieved.

## Solutions

### Recursive target reduction

This solution follows the hint above.

```java
class Solution {
    public List<List<String>> allConstruct(String target, String[] wordBank) {
        List<List<String>> acc = new ArrayList<>();
        allConstruct(target, 0, wordBank, new ArrayList<>(), acc);
        return acc;
    }

    public void allConstruct(String target, int index, String[] wordBank,
        List<String> path, List<List<String>> acc) {
        if (index == target.length()) {
            // We have reached the base case, accumulate the current path.
            acc.add(new ArrayList<>(path));
            return;
        }

        for (String w : wordBank) {
            if (target.indexOf(w, index) == index) {
                // Prefix match, add to the path and recurse.
                path.add(w);
                allConstruct(target, index + w.length(), wordBank, path, acc);
                path.remove(w);
            }
        }
    }
}
```

### Recursive target reduction no explicit accumulator

Here we take the approach from the prior solution, and remove the accumulator.
Instead of adding to an accumulator when we reach a base case, we return the
partial solution of list with an empty list in it. The caller appends the
particular prefix word match into that list. When the recursion unfolds back
to the top level the lists all fully formed.

```java
class Solution {
    public List<List<String>> allConstruct(String target, String[] wordBank) {
        return allConstruct(target, 0, wordBank);
    }

    public List<List<String>> allConstruct(String target, int index, String[] wordBank) {
        if (index == target.length()) {
            // We have reached the base case, the empty string can be matched
            // by choosing no words from the word bank, return a list with
            // an empty list.
            List<List<String>> result = new ArrayList<>();
            result.add(new ArrayList<>());
            return result;
        }

        List<List<String>> results = new ArrayList<>();

        for (String w : wordBank) {
            if (target.indexOf(w, index) == index) {
                // Prefix match, get the solutions from the reduced case, and
                // then add w to the solution.
                List<List<String>> r = allConstruct(target, index + w.length(), wordBank);
                for (List<String> l : r) {
                    l.add(w);
                }
                results.addAll(r);
            }
        }

        return results;
    }
}
```

### Memoized ecursive target reduction no explicit accumulator

This is the above solution with a memoizer thrown in. Note the important use
of the copy function here. Without it, the memoized values are mutable and
they get mutated via later calls - thus destroying the integrity of the cache
and the solution.

```java
class Solution {
    public List<List<String>> allConstruct(String target, String[] wordBank) {
        Map<Integer, List<List<String>>> memo = new HashMap<>();
        return allConstruct(target, 0, wordBank, memo);
    }

    public List<List<String>> allConstruct(String target, int index, String[] wordBank,
            Map<Integer, List<List<String>>> memo) {
        if (memo.containsKey(index)) {
            return copy(memo.get(index));
        } else if (index == target.length()) {
            // We have reached the base case, the empty string can be matched
            // by choosing no words from the word bank, return a list with
            // an empty list.
            List<List<String>> result = new ArrayList<>();
            result.add(new ArrayList<>());
            return result;
        }

        List<List<String>> results = new ArrayList<>();

        for (String w : wordBank) {
            if (target.indexOf(w, index) == index) {
                // Prefix match, get the solutions from the reduced case, and
                // then add w to the solution.
                List<List<String>> r = allConstruct(target, index + w.length(), wordBank, memo);
                for (List<String> l : r) {
                    l.add(w);
                }
                results.addAll(r);
            }
        }

        memo.put(index, copy(results));
        return results;
    }

    private List<List<String>> copy(List<List<String>> value) {
        List<List<String>> copy = new ArrayList<>();
        for (List<String> l : value) {
            copy.add(new ArrayList<>(l));
        }
        return copy;
    }
}
```
