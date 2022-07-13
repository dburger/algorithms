# Most Stones Removed with Same Row or Column

On a 2D plane, we place `n` stones at some integer coordinate points. Each
coordinate point may have at most one stone.

A stone can be removed if it shares either **the same row or the same column**
as another stone that has not been removed.

Given an array of `stones` of length `n` where `stones[i] = [xi, yi]`
represents the location of the `ith` stone, return the largest possible
number of stones that can be removed.

## Hints

1. This problem is best though of as a graph.

## Solutions

### Union-Find

As noted in the hint this problem is best approached thinking about graphs.
If we consider each node in the same row or column to be connected, we end
up with a number of connected components. Each connected component can be
treated like a tree, and thus leaves can be erased one at a time until you
arrive at the root. Thus for each connected component in the graph you can
remove stones until only one stone of that component remains.

Well that is cool and all but how does this get us to a solution? Turns
out, there is a data structure that fits this situation perfectly. It is
called Union-Find. With Union-Find it is possible to quickly union groups
and find which group a member belongs to.

In the code below we start by putting each node in its own group. Then we
iterate through the stones choosing a representative for each row and
column. Then we union each stone into its representative row and column.

This leaves us with the computed Union-Find. To get the number of moves
that can be made we take the number of total stones and subtract the
number of connected components. Since each connected component will leave
one stone behind this is the correct answer.

```java
class Solution {
    class UnionFind {
        int[] parents;

        UnionFind(int num) {
            this.parents = new int[num];
            // Each element starts off in its own group.
            for (int i = 0; i < num; i++) {
                this.parents[i] = i;
            }
        }

        int find(int x) {
            // Recursively go up till we find the representative for
            // x's group. This will be the element that is its own parent.
            int parent = this.parents[x];
            if (parent == x) {
                return x;
            }
            return find(parent);
        }

        void union(int a, int b) {
            // Attach a's representative to b's representative.
            this.parents[find(a)] = find(b);
        }
    }

    public int removeStones(int[][] stones) {
        UnionFind uf = new UnionFind(stones.length);

        int maxRow = -1;
        int maxCol = -1;
        for (int[] stone : stones) {
            maxRow = Math.max(maxRow, stone[0]);
            maxCol = Math.max(maxCol, stone[1]);
        }

        int[] rowReps = new int[maxRow + 1];
        int[] colReps = new int[maxCol + 1];
        Arrays.fill(rowReps, -1);
        Arrays.fill(colReps, -1);

        for (int i = 0; i < stones.length; i++) {
            int[] stone = stones[i];
            int row = stone[0];
            int col = stone[1];

            int rowRep = rowReps[row];
            int colRep = colReps[col];

            if (rowRep == -1) {
                rowReps[row] = i;
            } else {
                uf.union(rowRep, i);
            }

            if (colRep == -1) {
                colReps[col] = i;
            } else {
                uf.union(colRep, i);
            }
        }

        // Count connected components by counting unique parents.
        Set<Integer> s = new HashSet<>();
        for (int i = 0; i < stones.length; i++) {
            s.add(uf.find(i));
        }

        return stones.length - s.size();
    }
}
```
