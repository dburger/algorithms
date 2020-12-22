# Palindrome Partitioning

Given a string `s`, partition `s` such that every substring of the partition is
a **palindrome**. Return all possible plaindrome partitionings of `s`.

## Hints

1. Find a palindrome at the front of a string and recurse on the tail until
   exhausted?

## Solutions

### Find and recurse

This is a common pattern for solving these types of problems that work by
exhausting an input by recursing on it and storing results in an accumulator.

```java
class Solution {
    public List<List<String>> partition(String s) {
        List<List<String>> acc = new ArrayList<>();
        partition(s, 0, new ArrayList<>(), acc);
        return acc;
    }

    private void partition(String s, int start, List<String> parts, List<List<String>> acc) {
        if (start == s.length()) {
            acc.add(new ArrayList<>(parts));
            return;
        }
        for (int i = start + 1; i <= s.length(); i++) {
            String part = s.substring(start, i);
            if (isPalindrome(s, start, i)) {
                parts.add(part);
                partition(s, i, parts, acc);
                parts.remove(parts.size() - 1);
            }
        }
    }

    private boolean isPalindrome(String s, int start, int end) {
        while (start < end) {
            if (s.charAt(start++) != s.charAt(--end)) {
                return false;
            }
        }
        return true;
    }
}
```
