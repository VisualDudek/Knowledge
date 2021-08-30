# Array

## Summary

* left - right pointer -&gt; indicate for while loop
* try to sort

## Three Number Sum

* if finally you will get Time Complexity of O\(n\*\*2\) it does not matter if parallel sort with O\(n log n\)
* remember to do not stop after first gues and increse/decrese left/right pointer

```text
# Algo
[-8, -4, 0, 1, 2, 3, 5, 12]
  ^   ^- left pointer    ^- right pointer
  main loop iterator
```

## Smallest Diff

```text
# pseudo-code
# INPUT array A and array B

sort A and B
set pointer for A and B to 0
while in boud of A and B:
if diff = 0 -> winner
if abs(dif) < curMin -> update curMin and res

if dif < 0 -> move pointer for A # dif = A - B
else, implicite dif > 0 -> move pointer B

return res 
```

## First Duplicate Value

* if array has only &lt;1...N&gt; elements you can use trick with fliping sign of given idx
* hash table is too esasy try solve it with Time: O\(n\) and Space O\(1\)

```text
# pseudo-code

iterate over array
check if number under current number-idx is < 0 if yes -> winner
else: make it negative, 
  that is the way we can mark that we have already seen number == current idx 
```

## Merge Overlapping Intervals

* key is sort over first param. \(start of interval\)

```text
# pseuso-code

sort over the first param

create result array and add first interval

iterate over array of intervals:
compare last result with iter: if overlaps -> modify last res
if not: -> append iter to the result array
```

