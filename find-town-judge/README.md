# Find the Town Judge

In a town, there are `n` people labeled from `1` to `n`. There is a rumor that
one of these people is secretly the town judge.

If the town judge exists, then:

1. The town judge trusts nobody.
1. Everbody (except for the town judge) trusts the town judge.
1. There is exactly one person that satisfies properties **1** and **2**.

You are given an array `trust` where `trust[i] = [ai, bi]` representing that
the person labeled `ai` trusts the person labeled `bi`.

Return the label of the town judge if the town judge exists and can be
identified, or return `-1` otherwise.

## Hints

1. Seems like a simple counting problem?

## Solutions

### Count and check that judicial wreck

Here we just keep counts:

1. of people trusted by `i` in `trusts`
1. of people trusting `i` in `trusted`

The when check by looking for an `i` that trusts `0` times and is trusted
`n - 1` times.

The time complexity of this solution is O(n) where n is the length of the
input. This comes from the iteration to collect the counts. The space
complexity is also O(n). This comes for the sizing of the counting arrays
`trusts` and `trusted`.

```java
class Solution {
    public int findJudge(int n, int[][] trust) {
        // Count of people trusted by person i.
        int[] trusts = new int[n + 1];
        // Count of people number of people trusting person i.
        int[] trusted = new int[n + 1];
        for (int i = 0; i < trust.length; i++) {
            int person = trust[i][0];
            int personTrusted = trust[i][1];
            trusts[person]++;
            trusted[personTrusted]++;
        }

        for (int i = 1; i <= n; i++) {
            if (trusts[i] == 0 && trusted[i] == n - 1) {
                return i;
            }
        }
        return -1;
    }
}
```

### Think of a graph

This is a solution that will be hard to come up with if you've already solved
it with simple counting as above. The prior solution has O(n) time complexity
and O(n) space complexity. While this won't beat it in either category, in
practice it uses half the space as it uses one counting array instead of two.
The trick is to think in terms of graph theory. We count the in-degree and
out-degree of each word. An in-degree vote for `i` is someone trusting `i`. An
out-degree vote for `i` is `i` trusting someone. If we count the in-degree
votes as a `+1` and out-degree votes as a `-1` then the judge will have a
final count of `n - 1`.

```java
class Solution {
    public int findJudge(int n, int[][] trust) {
        int[] counts = new int[n + 1];
        for (int i = 0; i < trust.length; i++) {
            int[] vote = trust[i];
            // vote for vote[1]
            counts[vote[1]]++;
            // voted by vote[0]
            counts[vote[0]]--;
        }
        for (int i = 1; i < counts.length; i++) {
            if (counts[i] == n - 1) {
                return i;
            }
        }
        return -1;
    }
}
```
