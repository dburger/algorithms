# Count Servers that Communicate

You are given a map of a server center, represented as an `m x n` matrix `grid`,
where 1 means that onn that cell there is a server and a 0 means that it hasn no
server. Two servers are said to communicate if they are on the same row or on
the same column.

Return the number of servers that communicate with any other server.

## Hints

1. The trick here seems to be able to some way record what you are seeing to
   assist in the count while avoiding double counting. The solution will at
   a minimum be O(m * n) as you will need to at least examine each cell.

## Solutions

### Two pass census and count

I call this census and count because this solution takes a first pass to
record a description of the contents of the grid in two arrays. A row
array holds the counts for each while and a col array holds the counts
for each column. This allows a second pass to trivially count those servers
with another server on the same row or column.

This solution has O(m * n) time complexity as it requires two complete
traversals of thee `m * n` grid. The space complexity is O(m) + O(n) which
would be the order of whichever is larger, the row or column count. This
is because of the arrays used to track the respective counts.

It doesn't seem like this should have been a leeter medium. Perhaps easy.

```java
class Solution {
    public int countServers(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        int[] rows = new int[m];
        int[] cols = new int[n];

        // First pass, record the row and column counts.
        for (int row = 0; row < m; row++) {
            for (int col = 0; col < n; col++) {
                if (grid[row][col] == 1) {
                    rows[row]++;
                    cols[col]++;
                }
            }
        }

        // Second pass, if position has server and row or column
        // shows > 1 server, then can communicate, increase count.
        int count = 0;
        for (int row = 0; row < m; row++) {
            for (int col = 0; col < n; col++) {
                if (grid[row][col] == 1 && (rows[row] > 1 || cols[col] > 1)) {
                    count++;
                }
            }
        }
        return count;
    }
}
```
