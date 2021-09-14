# Minimum Deletion Cost to Avoid Repeating Letters

Given a string `s` and an array of integers `cost` where `cost[i]` is the cost
of deleting the `i`th character is `s`, return the minimum cost of deletions
such that there are no two identical letters next to each other.

Notice that you will delete the chosen characters at the same time, after
deleting a character, the costs of deleting other characters will not change.

## Hints

1. Runs need to be identified and costs amongst the runs handled.

## Solutions

### Track your runs

This solution does what seems to be the obvious approach, track your runs to
compute your deletions. The only real wrinkle to understand here is that
because you are trying to minimize deletion cost you need to record the
`runMax` and `runSum` and your deletion cost for a run will be
`runSum - runMax`. When you detect that a run is over, you enter the values
for deletions amongst that run and then reset. Be careful here, don't forget
to handle the potential for a final run at the end of the input. This is
handled outside the loop.

The time complexity for this solution is O(n), where n is the length of the
input string. The space complexity is O(1).

```java
class Solution {
    public int minCost(String s, int[] cost) {
        int sum = 0;
        boolean inRun = false;
        int c = cost[0];
        int runSum = c;
        int runMax = c;
        char prior = s.charAt(0);
        for (int i = 1; i < s.length(); i++) {
            char next = s.charAt(i);
            c = cost[i];
            if (next == prior) {
                inRun = true;
                runSum += c;
                runMax = Math.max(runMax, c);
            } else {
                if (inRun) {
                    sum += runSum - runMax;
                    inRun = false;
                }
                runSum = c;
                runMax = c;
                prior = next;
            }
        }
        if (inRun) {
            sum += runSum - runMax;
        }
        return sum;
    }
}
```
