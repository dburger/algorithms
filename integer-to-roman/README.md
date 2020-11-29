# Integer to Roman

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


Given an integer, convert it to an roman numeral.

## Hints

1. Think of storing the numerals with a corresponding value array and
   then choosing the largest value you can subtract from what is left.
   Special cases can be handled as seen.

## Solutions

### Handle the cases

```java
class Solution {
    private String repeat(char c, int n) {
        String result = "";
        for (int i = 0; i < n; i++) {
            result += c;
        }
        return result;
    }

    public String intToRoman(int num) {
        char[] syms = {'I', 'V', 'X', 'L', 'C', 'D', 'M'};
        int[] vals = {1, 5, 10, 50, 100, 500, 1000};
        String result = "";
        while (num > 0) {
          for (int i = vals.length - 1; i > -1; i--) {
            int val = vals[i];
            if (val <= num) {
              if (num == 4) {
                result += "IV";
                num -= 4;
              } else if (num == 9) {
                result += "IX";
                num -= 9;
              } else if (val == 10 && num / val == 4) {
                result += "XL";
                num -= 40;
              } else if (val == 50 && num / 10 == 9) {
                result += "XC";
                num -= 90;
              } else if (val == 100 && num / val == 4) {
                result += "CD";
                num -= 400;
              } else if (val == 500 && num / 100 == 9) {
                result += "CM";
                num -= 900;
              } else {
                int units = num / val;
                num = num % val;
                result += repeat(syms[i], units);
              }
            }
          }
        }
        return result;
    }
}
```
