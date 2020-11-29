# Roman to Integer

Roman numerals are represented by seven different symbols:
`I`, `V`, `X`, `L`, `C`, `D` and `M`.

|Symbol|Value|
|---|---|
|I|1|
|V|5|
|X|10|
|L|50|
|C|100|
|D|500|
|M|1000|

For example, `2` is written as `II` in Roman numeral, just two one's added
together. `12` is written as `XII`, which is simply `X + II`. The number `27`
is written as XXVII,
which is `XX + V + II`.

Roman numerals are usually written largest to smallest from left to right.
However, the numeral for four is not `IIII`. Instead, the number four is written
as `IV`. Because the one is before the five we subtract it making four. The same
principle applies to the number nine, which is written as `IX`. There are six
instances where subtraction is used:

* `I` can be placed before `V` (5) and `X` (10) to make 4 and 9.
* `X` can be placed before `L` (50) and `C` (100) to make 40 and 90.
* `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.


Given a roman numeral, convert it to an integer.

## Hints

1. Nothing special here, just handle all the cases.

## Solutions

### Handle the cases

```java
class Solution {
    public int romanToInt(String s) {
        int n = s.length();
        int val = 0;
        for (int i = 0; i < n; i++) {
            char next = i + 1 < n ? s.charAt(i + 1) : ' ';
            switch (s.charAt(i)) {
              case 'I':
                  if (next == 'V') {
                      val += 4;
                      i++;
                  } else if (next == 'X') {
                      val += 9;
                      i++;
                  } else {
                      val += 1;
                  }
                  break;
              case 'V':
                  val += 5;
                  break;
              case 'X':
                  if (next == 'L') {
                      val += 40;
                      i++;
                  } else if (next == 'C') {
                      val += 90;
                      i++;
                  } else {
                      val += 10;
                  }
                  break;
              case 'L':
                  val += 50;
                  break;
              case 'C':
                  if (next == 'D') {
                      val += 400;
                      i++;
                  } else if (next == 'M') {
                      val += 900;
                      i++;
                  } else {
                      val += 100;
                  }
                  break;
              case 'D':
                  val += 500;
                  break;
              case 'M':
                  val += 1000;
                  break;
            }
        }
        return val;
    }
}
```
