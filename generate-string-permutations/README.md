# Generate String Permutations

Write a function to generate and return all permutations of an input string.

## Hints

1. This is another classic algorithm example. Start off by thinking of
   having a "consumed" and "remaining" buckets from the string and use
   recursion to move from the remaining to consumed. When remaining is
   empty, you have another permutation.

## Solutions

A solution is not difficult but can be optimized quite a bit. Here is
a beginning solution that uses strings and makes more new strings than
it needs to.

### Strings and more strings

This one uses the "consumed" and "remaining" approach noted above. It
does not attempt to optimize string creation so it is slower than the
ones that follow. It should be easier to understand, however. Note the
use of a wrapper function to set up an accumulator for the recursion.
Note this is O(n!) as it needs to produce each permutation.

```java
class Solution {
    private List<String> permute(String s) {
        List<String> acc = new ArrayList<>();
        String consumed = "";
        permute(consumed, s, acc);
        return acc;
    }

    private void permute(String consumed, String remaining, List<String> acc) {
        if (remaining.length() == 0) {
            acc.add(consumed);
            return;
        }

        for (int i = 0; i < remaining.length(); i++) {
            String c = consumed + remaining.charAt(i);
            String r = remaining.substring(0, i) + remaining.substring(i + 1);
            permute(c, r, acc);
        }
    }
}
```

### Mutate instead of create

The following solution is still O(n!), as above, but takes pains to mutate
existing data structures instead of creating new ones. This makes it faster
in practice although with the O(n!) it will still fall down quickly with
long string lengths.

Here we use a `StringBuilder` instead of string to hold the "consumed"
and "remaining" variables. Note how we "fix up" these structures on the
return from the recursive call.

```java
class Solution {
    private List<String> permute(String s) {
        List<String> acc = new ArrayList<>();
        StringBuilder consumed = new StringBuilder();
        StringBuilder remaining = new StringBuilder(s);
        permute(consumed, remaining, acc);
        return acc;
    }

    private void permute(StringBuilder consumed, StringBuilder remaining, List<String> acc) {
        if (remaining.length() == 0) {
            acc.add(consumed.toString());
            return;
        }

        for (int i = 0; i < remaining.length(); i++) {
            char c = remaining.charAt(i);
            consumed.append(c);
            remaining.deleteCharAt(i);
            permute(consumed, remaining, acc);
            consumed.deleteCharAt(consumed.length() - 1);
            remaining.insert(i, c);
        }
    }
}
```

### Boolean flags instead of mutate

Finally another alternative is to use a boolean array to track used characters
instead of the approach of mutating "remaining". Many available examples use
this approach. In practice it doesn't appear to beat the approach above.

```java
class Solution {
    private static List<String> permute(String s) {
        List<String> acc = new ArrayList<>();
        StringBuilder consumed = new StringBuilder();
        permute(consumed, s, new boolean[s.length()], acc);
        return acc;
    }

    private static void permute(StringBuilder consumed, String input, boolean[] used, List<String> acc) {
        if (consumed.length() == input.length()) {
            acc.add(consumed.toString());
            return;
        }

        for (int i = 0; i < input.length(); i++) {
            if (used[i]) {
                continue;
            }

            consumed.append(input.charAt(i));
            used[i] = true;
            permute(consumed, input, used, acc);
            consumed.deleteCharAt(consumed.length() - 1);
            used[i] = false;
        }
    }
}
```
