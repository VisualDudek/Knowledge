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

## reading named columns using named tuple

```python
# input
5
ID         MARKS      NAME       CLASS
1          97         Raymond    7
2          50         Steven     4
3          91         Adrian     9
4          72         Stewart    5
5          80         Peter      6
```

```python
# usecase for namedtuple
from collections import namedtuple

n, names = int(input()), input().split()
D = namedtuple('D', names)                  # <--- nice usage 
l = [D(*input().split()) for _ in range(n)] # input of data
res = sum([int(item.MARKS) for item in l])  # how to get it out

print(res/n)
```

