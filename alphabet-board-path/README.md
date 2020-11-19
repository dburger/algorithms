# Alphabet Board Path

On an alphabet board, we start at position `(0, 0)`, corresponding
to character `board[0][0]`.

Here, board = `["abcde", "fghij", "klmno", "pqrst", "uvwxy", "z"]`,
as here:

```text
a b c d e
f g h i j
k l m n o
p q r s t
u v w x y
z
```

We may make the following moves:

* `U` moves our position up one row, if the position exists on the board
* `D` moves our position down one row, if the position exists on the board.
* `L` moves our position left one column, if the position exists on the board.
* `R` moves our position right one column, if the position exists on the board.
* `!` adds the character `board[r][c]` at our current position `(r, c)` to the
  answer.

(Here, the only positions that exist on the board are positions with letters
on them.)

Return a sequence of moves that makes our answer equal to `target` in the
minimum number of moves. You may return any path that does so.

## Hints

1. The complexity here is that "z" sitting off by itself. You can do a loop
   that advances the position to the target position one move at a time,
   accumulating the answer, while paying heed to the "z" problem.

## Solutions

### Loop and poop

Here we do the obvious loop with considerations for that "z".

```java
class Solution {
    public String alphabetBoardPath(String target) {
      int row = 0;
      int col = 0;
      StringBuilder buf = new StringBuilder();
      for (char c : target.toCharArray()) {
        int[] loc = location(c);
        int newRow = loc[0];
        int newCol = loc[1];
        while (row != newRow || col != newCol) {
          // If going from row 4 to 5, but haven't changed the column yet,
          // wait for the column change so we don't go off the board.
          if (row < newRow && (row < 4 || col == newCol)) {
            buf.append("D");
            row++;
          } else if (row > newRow) {
            buf.append("U");
              row--;
          } else if (col < newCol) {
            buf.append("R");
            col++;
          } else {
            buf.append("L");
            col--;
          }
        }
        buf.append('!');
      }
      return buf.toString();
    }

    private int[] location(char c) {
          int val = c - 'a';
          int row = val / 5;
          int col = val % 5;
          return new int[] {row, col};
        }
    }
}
```
