# collections

## OrderedDict

remembers the order entries were added, OrderedDict maintains the order of insertion.

```python
d = OrderedDict.fromkeys('abcde')
d.move_to_end('b')
''.join(d.keys()) # 'acdeb'  LIFO

d.move_to_end('b', last=False)
''.join(d.keys()) # 'bacde'   FIFO
```

## defaultdict

class collections.**defaultdict**\(\[_default\_factory\[, ...\]\]_\)

dict subclass that calls a factory function to supply missing values, Usually a Python dict throws a `KeyError` if you try to get an item with a key that is not currently in the dictionary. The `defaultdict` in contrast will simply create any item that you try to access \(provided of course they do not exist yet\). To create such a "default" item, it calls the function object that you pass to the constructor.

* `d = defaultdict(int)` int wskazuje na typ value w parze key:value
* dla value int `d[k] += 1` ale dla value list `d[k].append(obj)` uwaga na to

```python
# do not overkill via default_factory, there are simple solutions:

d = defaultdict(int)
for x in keyword.kwlist:
    d[x] = len(x)
```

## Counter

counter.**most\_common**\(\[_n_\]\) -&gt; list

Elements with e**qual counts are ordered in the order first encountered.**

## namedtuple\(\)

**namedtuple**\(_typename_, _field\_names_, \*, _rename=False, defaults=None, module=None_\)

return a new tuple subclass named typename and positional or keyword arguments.

* can unpack like a regular tuple

```python
Point = namedtuple('Point', ['x', 'y'])
p = Point(11, y=22)
#can unpack
x, y = p
p[0] == p.x
```

