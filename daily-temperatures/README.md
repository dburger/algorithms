# Daily Temperatures

Given an array of integers `temperatures` representing daily temperatures,
return an array `answer` such that `answer[i]` is the number of days you have
to wait after the `ith` day to get a warmer temperature. If there is no future
day for which this is possible, keep `answer[i] == 0` instead.

## Hints

1. As we advance we either immediately get an answer or have to keep going. Any
   value can result in an answer for a number of values to the left. What data
   structure can handle this?

## Solutions

### Get back to the stack

Using a stack gives a very nice approach to this problem. We advance through
the daily temperatures. With each temperature, if the stack is empty we push
it on the stack. If the stack is not empty we compare it to the top of the
stack. If the value represents an increase we can "mark" that prior value. We
continue doing this until we mark all the values or push the new value on the
stack. Note that the stack never has an increasing value on it, and new values
have the potential to clear part or all of the stack.

The time complexity of this solution is O(n) where `n` is the length of the
input `temperatures`. This comes from having to iterate through the
temperatures up to twice. Once to introduce a value to the stack and another
time to clear it. The space complexity is also O(n) to hold the stack.

```java
class Solution {
    class Node {
        int day;
        int temp;
        Node(int day, int temp) {
            this.day = day;
            this.temp = temp;
        }
    }

    public int[] dailyTemperatures(int[] temperatures) {
        Deque<Node> d = new LinkedList<>();
        int result[] = new int[temperatures.length];
        Arrays.fill(result, 0);
        for (int day = 0; day < temperatures.length; day++) {
            Node curr = new Node(day, temperatures[day]);
            while (!d.isEmpty()) {
                Node prior = d.peekLast();
                if (prior.temp < curr.temp) {
                    result[prior.day] = day - prior.day;
                    d.removeLast();
                } else {
                    break;
                }
            }
            d.addLast(curr);
        }
        return result;
    }
}
```
