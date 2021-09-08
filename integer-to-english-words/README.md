# Integer to English Words

Create a non-negative integer `num` to English words representation.

## Hints

1. Focus on getting the 3 digit function correct. When you have that the full
   solution is a matter of putting together the 3 digit pieces.

## Solutions

The solution here is pretty straightforward if a bit tedious. It doesn't seem
this problem really earns a leeter "hard".

### Crank through your groups

The solution is pretty straighforward. First, get the 3 digit converter nailed
down. This is not difficult but it is slightly tedious. There is the special
casing of the teens. After that you can piece together a full solution by
splitting up the original number into three digit groups for the number of
billions, millions, thousands, and ones.

The space and time complexity of this solution is O(1). This comes from the
fact that this problem is bounded by the size of an `int`.

```java
class Solution {
    private final int[] mods = {1000000000, 1000000, 1000, 1};
    private final String[] names = {" Billion", " Million", " Thousand", ""};
    private final String[] nums = {"", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine"};
    private final String[] tees = {"", "", "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"};
    private final String[] teens = {"Ten", "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen", "Nineteen"};

    public String numberToWords(int num) {
        int[] groups = new int[mods.length];
        for (int i = 0; i < mods.length; i++) {
            groups[i] = num / mods[i];
            num = num % mods[i];
        }
        StringBuilder result = new StringBuilder();
        for (int i = 0; i < groups.length; i++) {
            StringBuilder part = toWords(groups[i]);
            if (part.length() > 0) {
                part.append(names[i]);
                if (result.length() > 0) {
                    result.append(" ");
                }
                result.append(part);
            }
        }
        return result.length() == 0 ? "Zero" : result.toString();
    }

    private StringBuilder toWords(int val) {
        int hundreds = val / 100;
        val %= 100;
        int tens = val / 10;
        val %= 10;

        StringBuilder result = new StringBuilder();
        if (hundreds > 0) {
            result.append(nums[hundreds]).append(" Hundred");
        }

        if (tens == 1) {
            if (result.length() > 0) {
                result.append(" ");
            }
            return result.append(teens[val]);
        }

        if (tens > 1) {
            if (result.length() > 0) {
                result.append(" ");
            }
            result.append(tees[tens]);
        }

        if (result.length() > 0 && val > 0) {
            result.append(" ");
        }

        return result.append(nums[val]);
    }
}
```
