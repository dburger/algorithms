# Bulls and Cows

You are playing the
[Bulls and Cows](https://en.wikipedia.org/wiki/Bulls_and_Cows) game with your
friend.

You write down a secret number and ask your friend to guess what the number is.
When your friend makes a guess, you provide a hint with the following info:

* The number of "bulls", which are digits in the guess that are in the correct
  position
* The number of "cows", which are digits in the guess that are in your secret
  number but are located in the wrong position. Specifically, the non-bull
  digits in the guess that could be rearranged such that they become bulls.

Given the secret number `secret` and your friend's guess `guess`, return the
*hint for your friend's guess*.

The hint should be formatted `"xAyB"` where `x` is the number of bulls and `y`
is the number of cows. Note that both `secret` and `guess` may contain
duplicate digits.

## Hints

1. This seems fairly straightforward. You can walk through the strings doing two
   things:
   * Count the bulls
   * For digits that are not bulls, store a digit frequency count
   Then when you exit that loop, the number of bulls is known, and the number of
   cows can be determined from your frequency count.

## Solutions

### Frequency count

```java
class Solution {
    public String getHint(String secret, String guess) {
        int bulls = 0;
        int[] secretFreq = new int[10];
        int[] guessFreq = new int[10];
        for (int i = 0 ; i < secret.length(); i++) {
            char sc = secret.charAt(i);
            char gc = guess.charAt(i);
            if (sc == gc) {
                bulls++;
            } else {
                secretFreq[sc - '0']++;
                guessFreq[gc - '0']++;
            }
        }
        int cows = 0;
        for (int i = 0; i < 10; i++) {
            cows += Math.min(secretFreq[i], guessFreq[i]);
        }
        return bulls + "A" + cows + "B";
    }
}
```
