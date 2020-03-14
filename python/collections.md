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

## Counter

counter.**most\_common**\(\[_n_\]\) -&gt; list

Elements with e**qual counts are ordered in the order first encountered.**

