# itertools

## combinations\(\)

itertools.**combinations**\(_iterable, r_\) -&gt; iterator

Return _r_ length subsequences of elements from the input _iterable_.

Combinations are emitted in lexicographic sort order. So, if the input _i**terable**_ **is sorted,** the combination tuples w**ill be produced in sorted order.**

Elements are treated as unique based on their position, not on their value. So **if the input elements are unique,** there will be no repeat values in each combination.

