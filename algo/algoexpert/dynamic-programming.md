# Dynamic Programming

## Max Subset Sum No Adjacent

build array with max no adjacent sums for given index then, solution for idx will be max of two options: \(1\) maxSums\[idx-1\] or \(2\) maxSums\[idx-2\] + currentNumber

```text
# pseudo-code

iterate over array
maxSums[idx] = max(maxSums[idx-1], maxSums[idx-2] + currentNumber)

# as you can see you need previous two elements of maxSums array so you need to
#provide first and second element "manualy"
```

you can do better in case of Space complexity: O\(1\)

