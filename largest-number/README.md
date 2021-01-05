# Largest Number

Given a list of non-negative integers `nums`, arrange them such that they form
the largest number.

**Note:** The result may be very large, so you need to return a string instead
of an integer.

## Hints

1. There is a aha trick here in how to arrange the strings.

## Solutions

### Aha trick

Here we use the aha trick. The aha is that if you take two of these strings
say `s1` and `s2`, and you consider `o1 = s1 + s2` and `o2 = s2 + s1`, and
choose the one that makes the larger number, this will be the proper sort
order.

A wrinkle here is an edge case of all zeroes. In that case the leading
character will be a '0' and the answer is "0".

We have an O(n) conversion to strings here, followed by an O(n\*log(n))
sort, followed by a final O(n) string concatenation. Thus the time
complexity of this solution is O(n\*log(n)).

```java
class Solution {
    public String largestNumber(int[] nums) {
        String[] snums = new String[nums.length];
        for (int i = 0; i < nums.length; i++) {
            snums[i] = String.valueOf(nums[i]);
        }

        Arrays.sort(snums, (s1, s2) -> {
            String o1 = s1 + s2;
            String o2 = s2 + s1;
            return o2.compareTo(o1);
        });

        StringBuilder buf = new StringBuilder();
        for (String s : snums) {
            buf.append(s);
        }

        return buf.charAt(0) == '0' ? "0" : buf.toString();
    }
}
```

### Aha trick with streams

Same as above, but using streams, for practice. Note that the stream approach,
in practice, is slower than the above approach.

```java
class Solution {
    public String largestNumber(int[] nums) {
        String result = Arrays.stream(nums)
            .mapToObj(String::valueOf)
            .sorted((s1, s2) -> (s2 + s1).compareTo(s1 + s2))
            .collect(Collectors.joining());
        return result.charAt(0) == '0' ? "0" : result;
    }
}
```
