# Camelcase Matching

A query word matches a given `pattern` if we can insert **lowercase** letters
to the pattern word so that it equals the `query`. (We may insert each character
at any position, and may insert 0 characters.)

Given a list of `queries`, and a `pattern`, return an `answer` list of booleans,
where `answer[i]` is true if and only if `queries[i]` matches the `pattern`.

## Hints

1. This seems like it should be a leeter easy rather than a medium. Using index
   pointers to advance through the query and pattern in an attempt to exhaust
   both should do the trick.

## Solutions

### Exhaust the query and pattern

Here a query index pointer `q` and pattern index pointer `p` are used to advance
through the query and pattern space. Whenever a lowercase letter is seen in the
query it can be skipped as this equates to the insertion into the pattern space
per the description. Other than that, uppercase letters must match in order or
there is no match.

If m is the length of the query and n is the length of the pattern, the time
complexity here is O(m). This comes from the worst case of iterating to the
end of the query. At this point you have either already rejected things, found
there is a match, or will reject things because pattern characters are left
over.

```java
class Solution {
    public List<Boolean> camelMatch(String[] queries, String pattern) {
        List<Boolean> result = new ArrayList<>();
        for (String query : queries) {
           result.add(camelMatch(query, pattern));
        }
        return result;
    }

    private boolean camelMatch(String query, String pattern) {
        int q = 0;
        int p = 0;
        while (q < query.length() && p < pattern.length()) {
            char qchar = query.charAt(q);
            char pchar = pattern.charAt(p);
            if (qchar == pchar) {
                // Match, advance both indices.
                q++;
                p++;
            } else if (qchar >= 'a' && qchar <= 'z') {
                // The query character is lower case, treat as insert
                // lowercase letter into pattern case.
                q++;
            } else {
                // The query character is upper case and did not match.
                return false;
            }
        }

        // At this point we have either consumed the query or the
        // pattern or both. The query could have trailing lowercase
        // letters that would be treated as the insert case.
        while (q < query.length()) {
            char qchar = query.charAt(q);
            if (qchar < 'a' || qchar > 'z') return false;
            q++;
        }

        return q == query.length() && p == pattern.length();
    }
}
```
