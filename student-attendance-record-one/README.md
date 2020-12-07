# Student Attendance Record I

You are given a string representing an attendance record for a student. The
record only contains the following three characters:

1. `A`: Absent.
1. `L`: Late.
1. `P`: Present.

A student could be rewarded if his attendance record doesn't contain
**more than one 'A' (absent)** or **more than two continuous 'L' (late)**.

You need to return whether the student could be rewarded according to his
attendance record.

## Hints

1. Not much hint here. Just loop and record your current state to figure out
   what to do.

## Solutions

### Loop and record

```java
class Solution {
    public boolean checkRecord(String s) {
        int totalA = 0;
        int consecutiveL = 0;
        char prior = ' ';
        for (int i = 0; i < s.length(); i++) {
          char c = s.charAt(i);
          if (c == 'A') {
            if (++totalA > 1) {
              return false;
            }
          } else {
            if (prior != 'L') {
              consecutiveL = 0;
            }
            if (c == 'L') {
              consecutiveL++;
              if (consecutiveL > 2) {
                return false;
              }
            }
          }
          prior = c;
        }
        return true;
    }
}
```
