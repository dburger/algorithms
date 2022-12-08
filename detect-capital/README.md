# Detect Capital

We define the usage of capitals in a word to be right when one of the following
cases holds:

- All letters in this word are capitals, like `"USA"`.
- All letters in this word are not capitals, like `"leetcode"`.
- Only the first letter in the word is capital, like `"Google"`.

Given a string `word`, return `true` if the usage of capitals is right.

## Hints

1. Wow. This is pretty much a fizz buzz.

## Solutions

### The straight story

Nothing fancy. I did this problem because I haven't done a leeter in a while
and I've been focusing on the problems in CTCI. This is just a straightforward
solution. Time complexity O(n) where `n` is the length of the string. Space
complexity O(1).

Note that there is an opportunity for a slight optimization. Let's say the
input strings were long, then checking in the loop if an invalid state has
already been reached could lead to a very early exit. In practice, with the
test cases in leeter, this results in a slow down.

```java
boolean detectCapitalUse(String word) {
    if (word.length() == 0) {
        return true;
    }
    boolean first = Character.isUpperCase(word.charAt(0));
    boolean all = first;
    boolean none = !first;
    int caps = first ? 1 : 0;
    for (int i = 1; i < word.length(); i++) {
        boolean upper = Character.isUpperCase(word.charAt(i));
        if (upper) {
            none = false;
            caps++;
        } else {
            all = false;
        }
    }
    return all || none || (first && caps == 1);
}
```
