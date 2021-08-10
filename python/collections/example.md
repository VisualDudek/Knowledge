# example

## print most common items 

there is a catch, there is more than one most common so if U use `Counter.most_common(1)` it will provide only one that was first encontered

```python
from collections import Counter

# c = Counter('barbbbacadabra')
c = Counter('abcde')

print(c)
v = c.most_common(1)[0][1]
print(v)

d = dict(c)
nd = dict(filter(lambda x: x[1] == v, d.items()))

print(sorted(nd.keys()))
```

