# Arrays and Strings

## 1.1 **Is Unique:**
    Implement an algorithm to determine if a string has all unique characters.
    What if you cannot use additional data structures?

### Solution 1

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

### Solution 2

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

### Solution 3

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

### What if you can't use additional data structures?

This extra constraint given in the problem can be accomplished in a couple
of ways. First, you could just do brute force and do a comparison of every
character to every other characters. This would result in an alogorithm that
didn't use additional data structures but run in `O(n^2)` time. Another
possible approach would be to sort the characters and then check for a
repeat of a neighbor. This would require `O(n * log(n))` for the sort.

## 1.2 **Check Permutation:**
    Given two strings, write a method to decide if on is a permutation of the
    other.

### Solution 1

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

### Solution 2

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

## 1.3 **URLify:**
    Write a method to replace all spaces in a string with `%20`. You may assume
    that the string has sufficient space at the end to hold the additional
    characters, and that you are given the "true" length of the string. (Note:
    If implementing in Java, please use a character array so that you can
    perform this operation in place.)

### Solution 1

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

## 1.4 **Palindrome Permutation:**
    Given a string, write a function to check if it is a permutation of a
    palindrome. A palindrome is a word or phrase that is the same forwards
    or backwards. A permutation is a rearrangement of letters. The palindrome
    does not need to be limited to just dictionary words.

### Solution 1

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

## 1.5 **One Away:**
    There are three types of edits that can be performed on strings:
    insert a character, remove a character, or replace a character.
    Given two strings, write a function to check if they are one edit
    (or zero edits) away.

### Solution 1

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

## 1.6 **String Compression:**
    Implement a method to perform basic string compression using the counts
    of repeated characters. For example, the string `aabcccccaaa` would
    become `a2b1c5a3`. If the "compressed" string would not become smaller
    than the original string, your method should return the original string.
    You can assume the string has only uppercase and lowercase letters (a-z).

### Solution 1

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

## 1.7 **Rotate Matrix:**
    Given an image represented by an `NxN` matrix, where each pixel in the
    image is 4 bytes, write a method to rotate the image by 90 degrees. Can
    you do this in place?

### Solution 1

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

## 1.8 **Zero Matrix:**
    Write an algorithm such that if an element in an `NxN` matrix is 0, its
    entire row and column are set to 0.

### Solution 1

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

## 1.9 **String Rotation:**
    Assume you have a method `isSubstring` which checks if one word is a
    substring of another. Given two strings `s1` and `s2`, write code to
    check if `s2` is a rotation of `s1` using only one call to `isSubstring`.

### Solution 1

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

## Linked Lists

## 2.1 **Remove Dups:**
    Write code to remove duplicates from an unsorted linked list.
    How would you solve this problem if a temporary buffer is not
    allowed?

### Solution 1

This solution works by keeping `prev` and `curr` pointers. We advance
`curr` through the linked list and set it as the `next` pointer in
`prev` when it represents a new value for the list. We recognize new
values with the usage of a `Set`. If it does not represent a new value,
we skip it.

The time complexity of this algorithm is `O(n)` where `n` is the size
of the linked list. The space complexity is `O(1)`.

```java
void removeDups(Node head) {
    if (head == null) {
        return;
    }
    Set<Integer> s = new HashSet<>();
    s.add(head.val);
    Node prev = head;
    Node curr = head.next;
    while (curr != null) {
        if (s.add(curr.val)) {
            prev.next = curr;
            prev = curr;
            curr = curr.next;
        } else {
            curr = curr.next;
        }
    }
    prev.next = null;
}
```

### Solution 2

To solve for the temporary buffer is not allowed case (our `Set` is not
allowed), we can solve the problem by using two pointers. In this approach
we use a `curr` pointers and `runner` pointers. We advance `runner` ahead
of `curr` and use it to skip duplicates of the value in `curr`.

While this reduces the space complexity to `O(1)`, the time complexity becomes
`O(n^2)`.

```java
void removeDups(Node head) {
    if (head == null) {
        return;
    }
    Node curr = head;
    while (curr != null) {
        Node runner = curr;
        while (runner.next != null) {
            if (runner.next.val == curr.val) {
                runner.next = runner.next.next;
            } else {
                runner = runner.next;
            }
        }
        curr = curr.next;
    }
}
```

## 2.2 **Return Kth to Last:**
    Implement an algorithm to find the kth to last element of a singly
    linked list.

### Solution 1

No need to get fancy here. The simplest algorithm has an optimum `O(n)`
time complexity and `O(1)` space complexity where `n` is the length of the
list. We just use two passes. The first pass determines the count. The
second pass retrieves the correct element.

```java
Node kth(Node head, int k) {
    int count = 0;
    Node curr = head;
    while (curr != null) {
        count++;
        curr = curr.next;
    }

    int target = count - k;
    curr = head;
    while (target > 0) {
        curr = curr.next;
        target--;
    }
    return curr;
}
```

### Solution 2

Can we do this with recursion? Why yes, yes we can. Here we implement a
recursive approach by returning a `NodePosition` object that contains a
node and its position from the end. At the end of the list we include a
`null` node and `0` in this object. We replace the `node` and increment
`position` until `position == k`. At that point we merely pass the correct
node back up the call stack.

The time complexity of this solution is `O(n)` where `n` is the length of
the list. Likewise, the space complexity is also `O(n)`. This comes from
the stack frames that recurse out to the end of the list.

```java
class NodePosition {
    Node node;
    int position;

    NodePosition(Node node, int position) {
        this.node = node;
        this.position = position;
    }
}

Node kth(Node head, int k) {
    NodePosition np = kthNode(head, k);
    return np.node;
}

NodePosition kthNode(Node node, int k) {
    if (node == null) {
        return new NodePosition(null, 0);
    } else {
        NodePosition np = kthNode(node.next, k);
        if (np.position < k) {
             np.position++;
             np.node = node;
        }
        return np;
    }
}
```

### Solution 3

A final obvious solution avoids the double pass of the first solution by
walking one pointer out `k` hops in front of another, and then advancing
both until the first pointer hits the end of the list. At that point the
second pointer is on the kth node.

This solution has time complexity `O(n)` and space complexity `O(1)`.


```java
Node kth(Node head, int k) {
    Node n1 = head;
    Node n2 = head;
    while (k > 0) {
        n1 = n1.next;
        k--;
    }
    while (n1 != null) {
        n1 = n1.next;
        n2 = n2.next;
    }
    return n2;
}
```

## 2.3 **Delete Middle Node:**
    Implement an algoirthm to delete a node in the middle (i.e., any node but
    the first and last node, no necessarily the exact middle) of a singly
    linked list, given only access to that node.

### Solution 1

This can't be done, right? Well no it can't. Not if you literally mean to
delete the node. But what if you allow one to copy values. Then yes, yes it
can be done.

Here we copy the value of the next node into the deleted node and then skip
that next node.

The time and space complexity of this solution is `O(1)`.

```java
void deleteNode(Node node) {
    if (node == null || node.next == null) {
        throw new IllegalArgumentException("Invalid node passed to deleteNode");
    }
    node.val = node.next.val;
    node.next = node.next.next;
}
```

## 2.4 **Partition:**
    Write code to partition a linked list around a value x, such that all nodes
    less than x come before all nodes greater than or equal to x. If x is
    contained within the list, the values of x only need to be after elements
    less than x. The partition element x can appear anywhere in the "right
    partition"; it does not need to appear between the left and right
    partitions.

### Solution 1

Not much complexity here but a lot of bookeeping. We keep pointers for the
head and current of the left and right partitions and then piece them together
at the end. Note that the possibility of empty lists must be handled.

The time complexity of this solution is `O(n)` where `n` is the length of the
input list. The space complexity is `O(1)`.

```java
Node partitionList(Node head, int val) {
    Node leftHead = null;
    Node leftCurr = null;
    Node rightHead = null;
    Node rightCurr = null;

    while (head != null) {
        if (head.val < val) {
            if (leftHead == null) {
                leftHead = leftCurr = head;
            } else {
                leftCurr.next = head;
                leftCurr = head;
            }
        } else {
            if (rightHead == null) {
                rightHead = rightCurr = head;
            } else {
                rightCurr.next = head;
                rightCurr = head;
            }
        }
        head = head.next;
    }

    if (leftHead == null) {
        leftHead = rightHead;
    } else if (rightHead == null) {
        leftCurr.next = null;
    } else {
        leftCurr.next = rightHead;
        rightCurr.next = null;
    }
    return leftHead;
}
```

## 2.5 **Sum Lists:**
    You have two numbers represented by a linked list, where each node contains
    a single digit. The digits are stored in reverse order, such that the 1's
    digit is at the head of the list. Write a function that adds the two
    numbers and returns the sum as a linked list. Suppose the digits are stored
    in forward order. Repeat the above problem.

### Solution 1

Solving this problem can be done by iterating through the lists and adding the
correct value to a `sum` variable. What is the correct value? Well the first
value would be a "one's" digit, the second a "ten's" digit, and so on. The
returned list can be reconstructed with the typical mod 10 and divide by ten
peeling off of each digit.

The time complexity of this solution is `O(n)` where `n` is the length of the
longer list. The space complexity is `O(1)`.

```java
Node sumLists(Node n1, Node n2) {
    int sum = 0;
    int i = 0;
    while (n1 != null || n2 != null) {
        int mult = (int) Math.pow(10, i++);
        if (n1 != null) {
            sum += mult * n1.val;
        }
        if (n2 != null) {
            sum += mult * n2.val;
        }
        n1 = n1.next;
        n2 = n2.next;
    }
    Node head = new Node(null, sum % 10);
    Node curr = head;
    sum /= 10;
    while (sum > 0) {
        curr.next = new Node(null, sum % 10);
        sum /= 10;
        curr = curr.next;
    }
    return head;
}
```

### Solution 2 (forward order)

Doing this in forward order brings a couple of wrinkles. First, while we can
use [horner's rule](https://en.wikipedia.org/wiki/Horner%27s_method) to sum
the numbers, we must sum them independently because the lists may have different
lengths. Second, presumably the returned list must be with most significant digit
first. This means we can't use the mod 10 approach to get the digits. Instead,
we divide by the highest power of 10 used and then mod by that value. Then we
proceed to the next smaller power of 10.

The time complexity and space complexity of this solution remains the same as
the prior, at `O(n)` and `O(1)` respectively.

```java
Node sumLists(Node n1, Node n2) {
    int n1Sum = 0;
    int n2Sum = 0;
    int pows = 0;
    while (n1 != null || n2 != null) {
        if (n1 != null) {
            n1Sum = 10 * n1Sum + n1.val;
        }
        if (n2 != null) {
            n2Sum = 10 * n2Sum + n2.val;
        }
        n1 = n1.next;
        n2 = n2.next;
        pows++;
    }
    int sum = n1Sum + n2Sum;
    int divider = (int) Math.pow(10, pows--);
    Node head = new Node(null, sum / divider);
    sum %= divider;
    Node curr = head;
    while (pows > -1) {
        divider = (int) Math.pow(10, pows--);
        curr.next = new Node(null, sum / divider);
        curr = curr.next;
        sum %= divider;
    }
    return head;
}
```

## 2.6 **Palindrome:**
    Implement a function to check if a linked list is a palindrome.

### Solution 1

Obviously if you materialize the linked list into a string, this problem becomes
a trivial check if a string is a palindrome. We'll assume that isn't sufficient
for this problem. Thus, we implement a recursive solution.

In this solution we pass a `curr` node and a `NodeWrapper` down a recursive
chain. The `NodeWrapper` is used to hold `head` and then to advance head as we
back up the call chain. We first call all the way down to the end of the list,
then as we back out, we check if the `curr.val` is equal to `wrapper.curr.val`.
If it is we advance `wrapper.curr` and we return true. If it is not we return
false. A true return causes a similar execution to occur up the chain. If we
get all the way back to the first invocation with no `false`, we have a
palindrome.

Let me emphasize how neat this solution is. We use a wrapped `Node` to advance
forward through the list while the recursion backs up through the list.

```java
boolean palindrome(Node head) {
    return palindrome(head, new NodeWrapper(head));
}

boolean palindrome(Node curr, NodeWrapper wrapper) {
    if (curr == null) {
        return true;
    }
    boolean valid = palindrome(curr.next, wrapper);
    if (!valid) {
        return valid;
    }
    if (curr.val == wrapper.curr.val) {
        wrapper.curr = wrapper.curr.next;
        return true;
    } else {
        return false;
    }
}
```

### Solution 2

Another solution that I consider a genuine exercise is creating a reversed
copy of the list and then doing a straight up comparison.

The time complexity of this algorithm is `O(n)` where `n` is the length of
the list. The space complexity is also `O(n)`, necessary to hold a copy of
the list.

```java
boolean palindrome(Node head) {
    Node copy = reverseCopy(head);
    return equalLists(head, copy);
}

Node reverseCopy(Node head) {
    Node copy = new Node(null, head.val);
    head = head.next;
    while (head != null) {
        Node temp = new Node(copy, head.val);
        copy = temp;
        head = head.next;
    }
    return copy;
}

boolean equalLists(Node l1, Node l2) {
    while (l1 != null && l2 != null) {
        if (l1.val != l2.val) {
            return false;
        }
        l1 = l1.next;
        l2 = l2.next;
    }
    return l1 == null && l2 == null;
}
```

## 2.7 **Intersection:**
    Given two (singly) linked lists, determine if the two lists intersect.
    Return the intersecting node. Note that the intersection is defined based
    on reference, not value. That is, if the kth node of the first linked list
    is the exact same node (by reference) as the jth node of the second linked
    list, then they are intersecting.

### Solution 1

Well doesn't a `Map` make this problem trivial? The time and space complexity
of this solution is `O(m + n)` where `m` and `n` are the lengths of the lists.

```java
Node intersectLists(Node n1, Node n2) {
    Set<Node> nodes = new HashSet<>();
    while (n1 != null) {
        nodes.add(n1);
    }
    while (n2 != null) {
        if (!nodes.add(n2)) {
            return n2;
        }
    }
    return null;
}
```

### Solution 2

Now obviously the authors of CTCI didn't think the above solution was very
clever. So how would you solve this without the simple `Map` solution?

If two lists interset they have the same last node. So this is easy to check.
Finding the intersection is also surprisingly easy if you know the trick.
The trick is to think if the lists were the same length we could just find
the intersection by advancing one node at a time in each list. We can exploit
this by "chopping off" the front of the longer list if we find that these
lists do in fact intersect.

The time complexity of this solution is `O(n)` where `n` is the length of the
longer list. The space complexity of this solution is `O(1)`.

```java
Node intersectLists(Node n1, Node n2) {
    NodeLength n1NodeLen = lastNode(n1);
    NodeLength n2NodeLen = lastNode(n2);
    if (n1NodeLen.node != n2NodeLen.node) {
        return null;
    }

    int diff = Math.abs(n1NodeLen.length - n2NodeLen.length);
    if (diff == 0) {
        return n1NodeLen.node;
    }

    Node longer = null;
    Node shorter = null;
    if (n1NodeLen.length > n2NodeLen.length) {
        longer = n1;
        shorter = n2;
    } else {
        longer = n2;
        shorter = n1;
    }

    while (diff > 0) {
        longer = longer.next;
        diff--;
    }

    while (longer != shorter) {
        longer = longer.next;
        shorter = shorter.next;
    }
    return longer;
}

class NodeLength {
    Node node;
    int length;
    NodeLength(Node node, int length) {
        this.node = node;
        this.length = length;
    }
}

NodeLength lastNode(Node node) {
    int length = 0;
    while (node.next != null) {
        node = node .next;
        length++;
    }
    return new NodeLength(node, length);
}
```

## 2.8 **Loop Detection:**
    Given a circular linked list, implement an algorithm that returns the node
    at the beginning of the loop.

### Solution 1

This could be implemented with a simple map that tracks when a node already
encountered is encountered the second time. Since this is so trivial, we
imagine this is not what the interviewer wants.

Instead we solve with the `slow` / `fast` pointer approach. The slow and fast
pointers will intersect in the loop. Say there are `k` nodes before the loop.
Then when the `slow` pointer enters the loop it will be `LOOP_SIZE % k` ahead
of the `fast` pointer. Since the `fast` pointer catches up 1 node in each
iteration it will catch up in `LOOP_SIZE % k` iterations. Now if we take one
of the pointers back to the start of the list, and advance until they intersect,
the intersection will take place at the start of the loop.

```java
Node findLoopStart(Node head) {
    Node fast = head;
    Node slow = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (fast == slow) {
            break;
        }
    }

    if (fast == null || fast.next == null) {
        return null;
    }

    slow = head;
    while (slow != fast) {
        slow = slow.next;
        fast = fast.next;
    }
    return slow;
}
```

## Stacks and Queues

## 3.1 **:**
    Describe how you could use a single array to implement three stacks.

### Discussion

This is just ugly. You could mark out set ranges for each of the three stacks.
That is, a portion of the array for each stack and a bunch of instance variables
to record the starting positions and ending positions for each of them. I would
recommend the constructor take the number of stacks and the capacity of an
individual stack. The code is tedious but not difficult.

```java
public class MultiStack {
    final int num;
    final int capacity;
    final int[] values;
    final int[] sizes;

    public MultiStack(int num, int capacity) {
        this.num = num;
        this.capacity = capacity;
        this.values = new int[num * capacity];
        this.sizes = new int[num];
    }

    public void push(int stack, int val) {
        if (stack < 1 || stack > this.num) {
            throw new IllegalArgumentException("Indicated stack out of range: " + stack);
        }

        // zero based
        stack--;

        if (isFull(stack)) {
            throw new IllegalStateException("Stack already full: " + stack);
        }

        int offset = stack * capacity;
        this.values[offset + this.sizes[stack]] = val;
        this.sizes[stack]++;
    }

    public int pop(int stack) {
        return examine(stack, true);
    }

    public int peek(int stack) {
        return examine(stack, false);
    }

    private int examine(int stack, boolean remove) {
        if (stack < 1 || stack > this.num) {
            throw new IllegalArgumentException("Indicated stack out of range: " + stack);
        }

        // zero based
        stack--;

        if (isEmpty(stack)) {
            throw new IllegalStateException("Stack already empty: " + stack);
        }

        int offset = stack * capacity;
        int value = this.values[offset + this.sizes[stack] - 1];
        if (remove) {
            this.sizes[stack]--;
        }
        return value;
    }

    public boolean isFull(int stack) {
        return this.sizes[stack] == capacity;
    }

    public boolean isEmpty(int stack) {
        return this.sizes[stack] == 0;
    }
}
```

## 3.2 **:**
    How would you design a stack which, in addition to push and pop, has a
    function min which returns the minimum element? Push, pop and min should
    all operate in `O(1)` time.

## Solution 1

Notice that it didn't say you need to be able to remove the `min` like a `pop`.
Thus the solution is to add a `min` field to your `StackNode` that keeps track
of the minimum in the stack up to that point. This keeps all the operations at
`O(1)`.

```java
class MinStack<T extends Comparable<T>> {
    class StackNode<T> {
        final T data;
        final T min;
        final StackNode<T> next;
        StackNode(T data, T min, StackNode<T> next) {
            this.data = data;
            this.min = min;
            this.next = next;
        }
    }

    StackNode<T> top = null;

    void push(T data) {
        if (this.top == null) {
            this.top = new StackNode<>(data, data, null);
        } else {
            T min = data.compareTo(this.top.data) < 0 ? data : this.top.data;
            StackNode<T> sn = new StackNode<>(data, min, this.top);
            this.top = sn;
        }
    }

    T peek() {
        if (this.top == null) {
            throw new IllegalStateException("Stack empty on peek.");
        }
        return top.data;
    }

    T pop() {
        if (this.top == null) {
            throw new IllegalStateException("Stack empty on pop.");
        }
        T data = top.data;
        top = top.next;
        return data;
    }

    T min() {
        if (this.top == null) {
            throw new IllegalStateException("Stack empty on min.");
        }
        return this.top.min;
    }

    boolean empty() {
        return this.top == null;
    }
}
```

## 3.3 **Stack of Plates:**
   Imagine a (literal) stack of plates. If the stack gets too high, it might
   topple. Therefore, in real life, we would likely start a new stack when the
   previous statck exceeds some threshold. Implement a data structure
   `SetOfStacks` that mimics this. `SetOfStacks` should be composed of several
   stacks and should create a new stack once a previous one exceeds capacity.
   `SetOfStacks.push()` and `SetOfStacks.pop()` should behave identically to a
   single stack (that is `pop()` should return the same values as it would if
   there were just a single stack).
   Follow up: Implement a function `popAt(int)` which performs a pop operation
   on a specific sub-stack.

### Solution 1

This straightforward solution using a `List` to hold a list of `Stack`s. Most
operations are similar to a regular stack. `pop()` that removes the last element
for a stack that is not the last stack, however, will require copying the other
stacks back down over the empty slot.

```java
public class SetOfStacks<T> {
    private static class Stack<T> {
        static class StackNode<T> {
            final T data;
            final StackNode<T> next;
            StackNode(T data, StackNode<T> next) {
                this.data = data;
                this.next = next;
            }
        }

        int size;
        StackNode<T> top;
        void push(T data) {
            if (this.top == null) {
                this.top = new StackNode<>(data, null);
            } else {
                StackNode<T> sn = new StackNode<>(data, this.top);
                this.top = sn;
            }
            size++;
        }

        T peek() {
            if (this.top == null) {
                throw new IllegalStateException("Empty stack on peek.");
            }
            return this.top.data;
        }

        T pop() {
            if (this.top == null) {
                throw new IllegalStateException("Empty stack on pop.");
            }
            T data = this.top.data;
            this.top = this.top.next;
            size--;
            return data;
        }

        int size() {
            return this.size;
        }
    }

    final int capacity;
    final List<Stack<T>> stacks = new ArrayList<>();
    public SetOfStacks(int capacity) {
        this.capacity = capacity;
    }

    void push(T data) {
        Stack<T> s = null;
        if (stacks.size() == 0) {
            s = new Stack<>();
            stacks.add(s);
        } else {
            s = stacks.get(stacks.size() - 1);
            if (s.size() == capacity) {
                s = new Stack<>();
                stacks.add(s);
            }
        }
        s.push(data);
    }

    T pop() {
        return popAt(stacks.size() - 1);
    }

    T popAt(int index) {
        if (index < 0 || index >= stacks.size()) {
            throw new IllegalArgumentException("Stacks index out of range: " + index);
        }
        Stack<T> s = stacks.get(index);
        T data = s.pop();
        if (s.size() == 0) {
            stacks.remove(index);
        }
        return data;
    }

    T peek() {
        if (stacks.size() == 0) {
            throw new IllegalStateException("Stacks empty on peek.");
        }
        return peekAt(stacks.size() - 1);
    }

    T peekAt(int index) {
        if (index < 0 || index >= stacks.size()) {
            throw new IllegalStateException("Index out of range on peek.");
        }
        Stack<T> s = stacks.get(index);
        return s.peek();
    }
}
```

## 3.4 **Queue Via Stacks:**
   Implement a `MyQueue` class which implements a queue using two stacks.

### Solution 1

The main thing to get here is that a `Queue` can be implemented with two
stacks by keeping the values in a stack and when the first item is needed
you need to grab the bottom item. This can be done by pushing all the items
on another stack, and then working with the top item. This is obviously
expensive. To get the front item takes an `O(n)` shuffling of the data onto
another stack and back on the original.

```java
public class MyQueue<T> {
    Stack<T> stack = new Stack<>();
    Stack<T> aux = new Stack<>();

    void add(T data) {
        this.stack.push(data);
    }

    T remove() {
        return bottom(true);
    }

    T peek() {
        return bottom(false);
    }

    private T bottom(boolean remove) {
        if (this.stack.isEmpty()) {
            throw new IllegalStateException("Stack is empty.");
        }
        while (!this.stack.isEmpty()) {
            this.aux.add(this.stack.pop());
        }
        T data = remove ? this.aux.pop() : this.aux.peek();
        while (!this.aux.isEmpty()) {
            this.stack.add(this.aux.pop());
        }
        return data;
    }

    boolean isEmpty() {
        return this.stack.isEmpty();
    }
}
```

## 3.5 **Sort Stack:**
   Write a program to sort a stack such that the smallest items are on the top.
   You can use an additional temporary stack, but you may not copy the elements
   into any other data structure (such as an array). The stack supports the
   following operations: `push`, `pop`, `peek`, and `isEmpty`.

### Solution 1

We can sort using two stacks. Assume one of the stacks is sorted, and to help
kick things off note that an empty stack is sorted. Then in each round we pop
an item off the unsorted stack, call it `value`. Now we move `value` to its
correct position in the sorted stack. If the sorted stack is empty or `value`
is greater than the sorted stack's top, we can place it on the sorted stack.
Otherwise we pop down to the correct location and push `value` onto the sorted
stack. The items that were greater than `value` get pushed back on the unsorted
stack. Lastly we move the elements back into the source stack to get the desired
ordering.

```java
void sortStack(Stack<Integer> s) {
    Stack<Integer> sorted = new Stack<>();
    while (!s.isEmpty()) {
        int value = s.pop();
        while (!sorted.isEmpty() && value > sorted.peek()) {
            s.push(sorted.pop());
        }
        sorted.push(value);
    }

    while (!sorted.isEmpty()) {
        s.push(sorted.pop());
    }
}
```

## 3.6 **Animal Shelter:**
   An animal shelter, which holds only dogs and cats, operates on a strictly
   "first in, first out" basis. People must adopt either the "oldest" (based on
   arrival time) of all animals at the shelter or they can select whether they
   will receive a dog or a cat (and will receive the oldes animal of that
   type). They cannot select which specific animal they would like. Create the
   data strucutres to maintain this system and implement operations such as
   `enqueue`, `dequeueAny`, `dequeueDog`, and `dequeueCat`. You may use the
   built-in `LinkedList` data structure.

### Solution 1

Nothing fancy here. Just keep two `Dequeue`s, one for cats and one for dogs.
Enqueue them with an arrival stamp to when needing to choose the oldest amongst
both cats and dogs.

```java
public class AnimalShelter {
    static class Animal {
        final String name;
        final int arrival;
        Animal(String name, int arrival) {
            this.name = name;
            this.arrival = arrival;
        }
    }

    static class Cat extends Animal {
        Cat(String name, int arrival) {
            super(name, arrival);
        }
    }

    static class Dog extends Animal {
        Dog(String name, int arrival) {
            super(name, arrival);
        }
    }

    private final Deque<Dog> dogs = new LinkedList<>();
    private final Deque<Cat> cats = new LinkedList<>();
    private final AtomicInteger order = new AtomicInteger();

    void enqueuCat(String name) {
        this.cats.add(new Cat(name, order.getAndIncrement()));
    }

    void enqueuDog(String name) {
        this.dogs.add(new Dog(name, order.getAndIncrement()));
    }

    Cat dequeuCat() {
        if (this.cats.isEmpty()) {
            throw new IllegalStateException("No more cats.");
        }
        return this.cats.removeFirst();
    }

    Dog dequeuDog() {
        if (this.dogs.isEmpty()) {
            throw new IllegalStateException("No more dogs.");
        }
        return this.dogs.removeFirst();
    }

    Animal dequeuAny() {
        if (this.cats.isEmpty() && this.dogs.isEmpty()) {
            throw new IllegalStateException("No animals");
        } else if (!this.cats.isEmpty() && !this.dogs.isEmpty()) {
            Cat c = this.cats.peekFirst();
            Dog d = this.dogs.peekFirst();
            if (c.arrival < d.arrival) {
                return this.cats.removeFirst();
            } else {
                return this.dogs.removeFirst();
            }
        } else if (this.cats.isEmpty()) {
            return this.dogs.removeFirst();
        } else {
            return this.cats.removeFirst();
        }
    }
}
```
