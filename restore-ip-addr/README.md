# Restore IP Addresses

Given a string `s` containing only digits, return all possible valid IP
addresses that can be obtained from `s`. You can return them in any order.

A valid IP address consists of exactly four integers, each integer is between
`0` and `255`, separated by single dots and cannot have leadding zeros. For
example "0.1.2.201" and "192.168.1.312" are validi IP addresses and
"0.011.255.245", "192.168.312", and "192.168@1.1" are invalid IP addresses.

## Hints

1. This problem fits a common pattern of "consuming" a string, with recursive
   calls being a good way to process each segment.

## Solutions

### Recursive with backtracking

Here we go with a recursive backtracking approach. That is we try to formulate
a list of four segements that consumes the entire input string. Backtracking
occurs when we remove a segment to go back to a prior segment and add another
digit.

This problem is easy to screw up with off by one indexing! In particular pay
note to the formulation of the segments and the substring calls to do so.

```java
class Solution {
    public List<String> restoreIpAddresses(String s) {
        List<String> acc = new ArrayList<>();
        rip(s, 0, new ArrayList<>(), acc);
        return acc;
    }

    private void rip(String s, int pos, List<String> parts, List<String> acc) {
        if (parts.size() == 4) {
            acc.add(String.join(".", parts));
            return;
        }

        parts.add("");
        int segment = parts.size() - 1;
        for (int i = pos + 1; i <= pos + 4 && i <= s.length(); i++) {
            String part = s.substring(pos, i);
            if (remainingPossible(s, i, parts) && isValid(part)) {
                parts.set(segment, part);
                rip(s, i, parts, acc);
            }
        }
        parts.remove(segment);
    }

    /**
     * Returns whether length of remaining string remaining is valid for the number
     * of segments yet to be formed.
     */
    private boolean remainingPossible(String s, int pos, List<String> parts) {
        int segments = 4 - parts.size();
        int characters = s.length() - pos;
        return characters >= segments && characters <= segments * 3;
    }

    /**
     * Returns whether the digits in {@code s} are a valid ip segment.
     */
    private boolean isValid(String s) {
        if (s.length() > 1 && s.charAt(0) == '0') {
            return false;
        }
        int val = Integer.parseInt(s);
        return val > -1 && val < 256;
    }
}
```
