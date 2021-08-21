# Number of Burgers with No Waste of Ingredients

Given two integers `tomatoSlices` and `cheeseSlices`, the ingredients of
different burgers are as follows:

*   **Jumbo Burger**: 4 tomato slices and 1 cheese slices
*   **Small Burger**: 2 tomato slices and 1 cheese slice

Return `[total_jumbo, total_small]` so that the number of remaining
`tomatoSlices` equal to 0 and the number of remaining `cheeseSlices` equal
to 0. If it is not possible to make the remaining `tomatoSlices` and
`cheeseSlices` equal to 0 return `[]`.

## Hints

1. If you look closely all you have here is two equations in two unknowns. It
   is hard to imagine this should be a leeter medium.

## Solutions

### Two equations, two unknowns

We will be given `tomatoSlices` and `cheeseSlices`. In terms of `jumbos` and
`smalls` we have.

```
4 * jumbos + 2 * smalls = tomatoSlices
jumbos + smalls = cheeseSlices
```

Here we solve using substitution. Solving for `smalls`:

```
smalls = cheeseSlices - jumbos
```

and then substituting into the other equation and solving for jumbos:

```
4 * jumbos + 2 * (cheeseSlices - jumbos) = tomatoSlices
           2 * jumbos + 2 * cheeseSlices = tomatoSlices
                              2 * jumbos = tomatoSlices - 2 * cheeseSlices
			          jumbos = tomatoSlices / 2 - cheeseSlices
```

Thus if we compute `jumbos` we can just use that in the equation for `smalls`
and we have both values. The only remaining twist is that we can short
circuit the heck out of here if:

*   `tomatoSlices` is not even as both size burgers take an even number of
    tomato slices. If this value is odd, there is no solution.
*   either `tomatoSlices` or `cheeseSlices` is computed to be a negative. No
    such real world meat burgers exist.

The time and space complexity of this solution is O(1).

```java
class Solution {
    public List<Integer> numOfBurgers(int tomatoSlices, int cheeseSlices) {
        List<Integer> result = new ArrayList<>();
        if (tomatoSlices % 2 == 1) {
            return result;
        }
        int jumbos = tomatoSlices / 2 - cheeseSlices;
        int smalls = cheeseSlices - jumbos;
        if (jumbos < 0 || smalls < 0) {
            return result;
        }
        result.add(jumbos);
        result.add(smalls);
        return result;
}
```
