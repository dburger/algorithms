1.1 **Is Unique:**
    Implement an algorithm to determine if a string has all unique characters.
    What if you cannot use additional data structures?

## Solution 1

This obvious solution uses a `Set` to track if a character has been seen
before.

The time complexity of this solution is `O(n)` where `n` is the length of
the string `s`. The space complexity is also `O(n)` as we need to hold the
input characters in the `Set`.

```java
boolean allUnique(String s) {
    Set<Character> s = new HashSet<>();
    for (char c : s.toCharArray()) {
        if (!s.add(c)) {
            return false;
        }
    }
    return true;
}
```

## Solution 2

If we can assume a restricted character set we can dispense with the `Set`
and just use an array of boolean values. The time complexity remains the
same as above but the space complexity is reduced to `O(1)`.

```java
private static boolean allUnique(String s) {
    boolean[] seen = new boolean[256];
    for (int c : s.toCharArray()) {
        if (seen[c]) {
            return false;
        }
    }
    return true;
}
```

## Solution 3

Looking at the prior solution, assuming a limited character set, we can
return immediately if the number of characters in the input string is
bigger than the character set. This takes the time complexity down to
`O(1)` as the loop will never execute more than the character set size
number of times.

```java
private static boolean allUnique(String s) {
    if (s.length() > 256) {
        return false;
    }
    boolean[] seen = new boolean[256];
    for (int c : s.toCharArray()) {
        if (seen[c]) {
            return false;
        }
    }
    return true;
}
```

## What if you can't use additional data structures?

This extra constraint given in the problem can be accomplished in a couple
of ways. First, you could just do brute force and do a comparison of every
character to every other characters. This would result in an alogorithm that
didn't use additional data structures but run in `O(n^2)` time. Another
possible approach would be to sort the characters and then check for a
repeat of a neighbor. This would require `O(n * log(n))` for the sort.

1.2 **Check Permutation:**
    Given two strings, write a method to decide if on is a permutation of the
    other.

# Solution 1

An easy way to solve this problem is to sort the two input strings and compare
the result. If they are permutations of one another the sorted strings will
be equal.

The time complexity of this solution is `O(a * log(a) + b * log(b))` where
`a` is the length of the first string and `b` is the length of the second.

```java
boolean isPerm(String a, String b) {
    if (a.length() != b.length()) {
        return false;
    }
    char[] ca = a.toCharArray();
    char[] cb = b.toCharArray();
    Arrays.sort(ca);
    Arrays.sort(cb);
    return Arrays.equals(ca, cb);
}
```

# Solution 2

This more efficient solution works by creating a counting map for one
string and then subtracting back out of the counting map with the other.
If a value is not present, or if it reduces the count to a negative value,
false is returned. Otherwise, true is returned.

The time complexity of this solution is `O(n + m)` where `n` and `m` are
the lengths of the two different strings. The space complexity is `O(m)`
where `m` is the length of string `b`, as the counts for this string
are kept in the counting map.

Note that if we assume a restricted character set we could use a boolean
array instead of the `Map`. This reduces the space complexity to `O(1)`.

```java
private static boolean isPerm(String a, String b) {
    Map<Character, Integer> counts = new HashMap<>();
    for (char c : b.toCharArray()) {
        counts.put(c, counts.getOrDefault(c, 0) + 1);
    }
    for (char c : a.toCharArray()) {
        Integer old = counts.get(c);
        if (old == null || old == 0) {
            return false;
        }
        counts.put(c, old - 1);
    }
    return true;
}
```

1.3 **URLify:**
    Write a method to replace all spaces in a string with `%20`. You may assume
    that the string has sufficient space at the end to hold the additional
    characters, and that you are given the "true" length of the string. (Note:
    If implementing in Java, please use a character array so that you can
    perform this operation in place.)

## Solution 1

This solution works by first counting the spaces. This benefits us by figuring
out where the end of the resultant string is. Thus we can start in reverse and
avoid overwriting the string. That is we are writing into the extra spaces that
are indicated in the problem statement. The second pass copies the values back
into the character array. When the source character is a space, we copy the
three values of `%20` and decrement destination accordingly.

The time complexity of this solution is `O(n)` where `n` is the length of
the `input` character array. This comes from making two passes through the
input. The space complexity is `O(1)` as the space used to solve the problem
is not dependent on the input size.

```java
void urlify(char[] input, int len) {
    int numSpace = 0;
    for (int i = 0; i < len; i++) {
        if (input[i] == ' ') {
            numSpace++;
        }
    }
    int dest = len + 2 * numSpace;
    len--;
    for (; len > -1; len--) {
        char c = input[len];
        if (c == ' ') {
            input[dest] = '0';
            input[dest - 1] = '2';
            input[dest - 2] = '%';
            dest -= 3;
        } else {
            input[dest] = c;
            dest--;
        }
    }
}
```

1.4 **Palindrome Permutation:**
    Given a string, write a function to check if it is a permutation of a
    palindrome. A palindrome is a word or phrase that is the same forwards
    or backwards. A permutation is a rearrangement of letters. The palindrome
    does not need to be limited to just dictionary words.

## Solution 1

Think of the character counts in a palindrome. Even character counts can be
split to the left and right of center. There can be 0 or 1 odd character
counts for a middle character. This solution exploits this fact by first
noting the counts, and then seeing how many odd counts there are.

Note that this solution assumes a limited character set and thus uses a
boolean array. Alternatively a counting `Map` could be used with only slightly
more complicated code.

The time complexity of this solution is `O(n)` where `n` is the size of the
input string. The space complexity for this limited character set approach
is `O(1)`. For the `Map` approach, it would be `O(n)`.

```java
boolean isPalindromePerm(String input) {
    int[] counts = new int[128];
    for (int c : input.toCharArray()) {
        counts[c]++;
    }

    int odds = 0;
    for (int i = 0; i < counts.length; i++) {
        if (counts[i] % 2 == 1) {
            odds++;
        }
    }
    return odds == 0 || odds == 1;
}
```

1.5 **One Away:**
    There are three types of edits that can be performed on strings:
    insert a character, remove a character, or replace a character.
    Given two strings, write a function to check if they are one edit
    (or zero edits) away.

## Solution 1

There are three different types of edits that are valid. A key simplification
is that one of them can be eliminated. Remove a character from the longer
is equivalent to insert a character for the shorter. Thus we can get by
by breaking this into two cases. Here we do the `oneReplace` and `oneInsert`
cases. If the strings are of equal lengths we attempt `oneReplace`. This
is merely done by counting diffs and the positive case being where we find
one diff. For the `oneInsert` case, we iterate on the shorter until we find
the differing character. At this point we begin a second iteration where we
skip a character in the longer string and try to get to the end of both strings
with the rest all being equal. If we do, then one insert before the differing
position in the shorter string would make them equal.

The time complexity of this solution is `O(n)` where `n` is the length of
the longer string. The space complexit is `O(1)`.

```java
boolean oneEditAway(String s1, String s2) {
    if (s1.length() == s2.length()) {
        return s1.equals(s2) || oneReplace(s1, s2);
    } else if (s1.length() == s2.length() - 1) {
        return oneInsert(s1, s2);
    } else if (s2.length() == s1.length() - 1) {
        return oneInsert(s2, s1);
    } else {
        return false;
    }
}

boolean oneReplace(String s1, String s2) {
    int diffs = 0;
    for (int i = 0; i < s1.length(); i++) {
        if (s1.charAt(i) != s2.charAt(i)) {
            diffs++;
            if (diffs == 2) {
                return false;
            }
        }
    }
    return true;
}

boolean oneInsert(String shorter, String longer) {
    int i = 0;
    while (i < shorter.length() && shorter.charAt(i) == longer.charAt(i)) {
        i++;
    }
    while (i + 1 < longer.length() && shorter.charAt(i) == longer.charAt(i + 1)) {
        i++;
    }
    return i == shorter.length();
}
```

1.6 **String Compression:**
    Implement a method to perform basic string compression using the counts
    of repeated characters. For example, the string `aabcccccaaa` would
    become `a2b1c5a3`. If the "compressed" string would not become smaller
    than the original string, your method should return the original string.
    You can assume the string has only uppercase and lowercase letters (a-z).

## Solution 1

Here we can proceed through the string keeping track of the `last` character
and the `count` of how many of those we have seen in a row. When `c` is
not equal to `last`, we add its translation to the string builder. We keep
doing this until we either finish the string or determine that we already
have a string builder larger than the orginal string.

The time complexity of this algorithm is `O(n)` where `n` is the length of
the input string. This comes from the need to iterate through the input.
The space complexity is also `O(n)`. The `StringBuilder` may need to hold
up to `n` characters.

```java
void append(StringBuilder buf, char c, int count) {
    buf.append(c);
    if (count > 1) {
        buf.append(count);
    }
}

String tryCompress(String s) {
    if (s.length() == 0 || s.length() == 1) {
        return s;
    }
    StringBuilder buf = new StringBuilder();
    char last = s.charAt(0);
    int count = 1;
    for (int i = 1; i < s.length() && buf.length() < s.length(); i++) {
        char c = s.charAt(i);
        if (c == last) {
            count++;
        } else {
            append(buf, last, count);
            count = 1;
            last = c;
        }
    }
    append(buf, last, count);
    return buf.length() < s.length() ? buf.toString() : s;
}
```

1.7 **Rotate Matrix:**
    Given an image represented by an `NxN` matrix, where each pixel in the
    image is 4 bytes, write a method to rotate the image by 90 degrees. Can
    you do this in place?

## Solution 1

This is tricky and because I haven't ran this code I almost certainly made
an indexing error below. This works by storing the "bottom left" in a temporary
variable and then going around the matrix copying the rotate value into its
new slot. The `temp` variable takes care of the final copy for each round.

How do you figure out the indexing? Well, `j` moves faster than `i`. Think of
which row or column index is moving faster for each position of the matrix.
That is, top left, top right, bottom right, and bottom left. For the top left,
we are moving from right to left before we move down a row. Thus the faster
`j` would be used in the column index and the slower `i` would be used in the
row index.

The time complexity of this solution is `O(n)` where `n` is the dimension of
the input square matrix. The space complexity is `O(1)`.

```java
void rotate(int[][] matrix) {
    int size = matrix.length;
    for (int i = 0; i < size; i++) {
        for (int j = 0; j < size; j++) {
            // Record the bottom left corner.
            int temp = matrix[size - 1 - j][i];
            // Bottom left = bottom right.
            matrix[size - 1 - j][i] = matrix[size - 1 - i][size - 1 - j];
            // Bottom right = top right.
            matrix[size - 1 - i][size - 1 - j] = matrix [j][size - 1 - i];
            // Top right = top left.
            matrix [j][size - 1 - i] = matrix[i][j];
            // Top right = recorded bottom left.
            matrix[i][j] = temp;
        }
    }
}
```

1.8 **Zero Matrix:**
    Write an algorithm such that if an element in an `NxN` matrix is 0, its
    entire row and column are set to 0.

## Solution 1

Here we solve the problem with a two pass approach. In the first pass we flag
a boolean array if a row or column should be "zeroed out." In the second pass
we do the actual zeroing.

The time complexity of this solution is `O(n)` where `n` is the dimension of
the matrix. The space complexity is also `O(n)` to hold the flagging arrays.

```java
void zeroOut(int[][] input) {
    boolean[] rows = new boolean[input.length];
    boolean[] cols = new boolean[input[0].length];
    for (int row = 0; row < input.length; row++) {
        for (int col = 0; col < input.length; col++) {
            if (input[row][col] == 0) {
                rows[row] = true;
                cols[col] = true;
            }
        }
    }

    for (int row = 0; row < rows.length; row++) {
        if (rows[row]) {
            for (int col = 0; col < cols.length; col++) {
                input[row][col] = 0;
            }
        }
    }

    for (int col = 0; col < cols.length; col++) {
        if (cols[col]) {
            for (int row = 0; row < rows.length; row++) {
                input[row][col] = 0;
            }
        }
    }
}
```

1.9 **String Rotation:**
    Assume you have a method `isSubstring` which checks if one word is a
    substring of another. Given two strings `s1` and `s2`, write code to
    check if `s2` is a rotation of `s1` using only one call to `isSubstring`.

## Solution 1

I consider this problem one of those "spot the trick" problems. The trick
is concatenating the possibly rotated string `s2` to itself. If that string is a
rotation of the other string `s1` then `s1` will be found in this concatenation.

The solution is `O(n)` time complexity where `n` is the length of `s2`. The
space complexity is `O(n)`, as we have to make a string twice as long to do
the concatenation.

```java
boolean isRotation(String s1, String s2) {
    if (s1.length() != s2.length()) {
        return false;
    }
    String haystack = s2 + s2;
    return haystack.contains(s1);
}
```
