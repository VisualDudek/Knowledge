# itertools

* If U want lexicographic sorted order in output than sort the input seq.
* If you want to create your own iterator, you just have to implement the dunder `next` and `iter` methods

## user defined iterator

If you want to create your own iterator, you just have to implement the dunder next and iter methods

```python
class Backward():
    def __init__(self, data):
        self.data = data
        self.index = len(self.data)
        
    def __iter__(self):
        return self
        
    def __next__(self):
        if(self.index == 0):
            raise StopIteration
        else:
            self.index -= 1
            return self.data[self.index]
```

```python
# usage
bw = Backward([1,2,3,4,5])
for elem in bw:
    print(elem)
```

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

## islice\(\)

itertools.**islice**\(_iterable_, _start_, _stop_\[, _step_\]\) -&gt; iterator

Problem that is solves: if you need only part of iterator you would probably have to enumerate\(\) it and store result in list? 





