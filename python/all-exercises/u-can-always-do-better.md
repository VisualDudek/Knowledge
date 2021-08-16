# U can always do better

## nan

* based on hackerrank Iterables and Iterators
* you are operating on idx but in this case it is easier to do combination direct on chars

```python
from itertools import combinations

n = int(input())
s = input().split()
k = int(input())

helper = [idx for idx, c in enumerate(s) if c == 'a']

it = combinations(range(n), k)
i = 0
n = 0
for item in it:
    n += 1
    if any([idx in helper for idx in item]):
        i += 1

print(i/n)
```

```python
# better
from itertools import combinations

_,s,n = input(),input().split(),int(input())
t = list(combinations(s,n))
f = [i for i in t if 'a' in i]
print(len(f)/len(t))
```

## do not how describe it ???

```python
from itertools import product

k, m = map(int, input().split())
n = (list(map(int, input().split()))[1:] for _ in range(k))
results = map(lambda x: sum(i**2 for i in x) % m, product(*n))
print(max(results))
```

