# Find All Anagrams in a String

Given two strings `s` and `p`, return an array of all the start indices of
`p`'s anagrams in `s`. You may return the answer in any order.

An **anagram** is a word or phrase formed by rearranging the letters of a
different word or phrase, using all the original letters exactly once.

## Hints

1. Note that we are limited to lowercase english letters here. So a map can be
   used to hold the counts of `p` but an array is easy and faster.

## Solutions

### Brute me

This solution is basically a brute force approach that holds a `counts` array
and then attempts to zero that out from each starting position.

This works but the efficiency is not good. The time complexity here is O(n*m)
where n is the length of `s` and m is the length of `p`. The space complexity
is O(n) to hold the potential solutions list.

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        int plen = p.length();
        int[] counts = new int[26];
        for (Character c : p.toCharArray()) {
            counts[c - 'a']++;
        }

        List<Integer> result = new ArrayList<>();
        for (int i = 0; i < s.length(); i++) {
            int[] copy = counts.clone();
            int slen = 0;
            for (int j = i; j < s.length(); j++) {
                char c = s.charAt(j);
                int pos = c - 'a';
                int val = copy[pos];
                if (val < 1) {
                    break;
                }
                copy[pos]--;
                slen++;
            }
            if (slen == plen) {
                result.add(i);
            }
        }
        return result;
    }
}
```

### Slide me windows

This solution presents a classic sliding window solution algorithm. We start
off like we did in the previous solution, establishing the counts for each
character in `p`. Then we start the sliding window loop. The loop maintains
`l` and `r` for the left and right side of the window. `r` is exclusive. In
each loop we increment `r` and check if we have gotten too wide. If we have
then we increment `l`. Then we "consume" our new right side character.
Consuming the character is done by decrementing that character's count. If it
has gone negative, we increment `l`, and "un-consume" the corresponding
character, until we are back to `0`. We know we have a solution when the window
is the width of `p`. Think about this carefully. This follows from having done
`plen` decrements and our algorithm making sure that they have only gone to
`0`. None of them are allowed to go negative. The sliding window loop
terminates when `r` has hit the end of `s`.

This time complexity of this solution is O(n) where n is the length of `s`.
This stems from incrementing `r` and potentially `l` in each loop while moving
them to the end of `s`. The space complexity I suppose is O(n) as there could
be up to n entries in the solution. The space complexity for everything
other than what is required to hold the result in O(1).

```java
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        int slen = s.length();
        int plen = p.length();
        if (slen < plen) {
            return Collections.emptyList();
        }

        int[] counts = new int[26];
        for (int i = 0; i < plen; i++) {
            counts[p.charAt(i) - 'a']++;
        }

        List<Integer> result = new ArrayList<>();
        int l = 0;
        int r = 0;
        while (r < slen) {
            int rchar = s.charAt(r) - 'a';
            r++;
            if (r - l > plen) {
                counts[s.charAt(l) - 'a']++;
                l++;
            }
            counts[rchar]--;
            while (counts[rchar] < 0) {
                counts[s.charAt(l) - 'a']++;
                l++;
            }
            if (r - l == plen) {
                result.add(l);
            }
        }

        return result;
    }
}
```
