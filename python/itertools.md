# itertools

If U want lexicographic sorted order in output than sort the input seq.

## combinations\(\)

itertools.**combinations**\(_iterable, r_\) -&gt; iterator

Return _r_ length subsequences of elements from the input _iterable_.

Combinations are emitted in lexicographic sort order. So, if the input _i**terable**_ **is sorted,** the combination tuples w**ill be produced in sorted order.**

Elements are treated as unique based on their position, not on their value. So **if the input elements are unique,** there will be no repeat values in each combination.

## cycle\(\)

itertools.cycle\(_iterable_\)

```python
a = itertools.cycle('ABC')
next(a)  # A B C A B C A ...

# or
for item in a:
    time.sleep(1)
    print(item)
```

## groupby\(\)

itertools.**groupby**\(_iterable, key=None_\) -&gt; k, g

```python
[k for k, g in groupby('AAAABBBCCDAABBB')] --> A B C D A B
[list(g) for k, g in groupby('AAAABBBCCD')] --> AAAA BBB CC D

# przykład z key=
l = [(1,2), (1,3), (4,5), (4,6)]
[(k, list(g)) for k, g in groupby(l, key=lambda x: x[0])]
```





