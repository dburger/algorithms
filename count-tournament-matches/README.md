# Count of Matches in Tournament

You are given an integer `n`, the number of teams in a tournament has strange
rules:

* If the current number of teams is **even**, each team gets paired with another
  team. A total of `n / 2` matches are played, and `n / 2` teams advance to the
  next round.
* If thee current number of teams is **odd**, one team randomly advances in the
  tournament, and the rest gets paired. A total of `(n - 1) / 2` matches are
  played, and `(n - 1) / 2 + 1` teams advance to the next round.

Return *the number of matches played* in the tournament until a winner is
decided.

## Hints

1. The obvious tact here is to think recursion.
1. Is it though - can you do it without?

## Solutions

### Recursive

Careful, the recursive solution is easy but the edge case of a tournament of
1 team must be handled.

```java
class Solution {
    public int numberOfMatches(int n) {
        if (n == 1) {
            return 0;
        } else if (n % 2 == 1) {
            int round = (n - 1) / 2;
            return round + numberOfMatches(round + 1);
        } else {
            return n - 1;
        }
    }
}
```

### Direct

While the recursive solution is cute, the even branch doesn't recurse. And,
since the number of games required to elminate all but 1 team is the number
of teams minus one, the implementation is trivial.

```java
class Solution {
    public int numberOfMatches(int n) {
        if (n % 2 == 1) {
            int round = (n - 1) / 2;
            // All but one team plays in a first round. Then
            // There will be round + 1 teams left.
            // == round + (round + 1) - 1
            return 2 * round;
        } else {
            return n - 1;
        }
    }
}
```
