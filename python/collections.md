# collections

## OrderedDict

remembers the order entries were added

```python
d = OrderedDict.fromkeys('abcde')
d.move_to_end('b')
''.join(d.keys()) # 'acdeb'  LIFO

d.move_to_end('b', last=False)
''.join(d.keys()) # 'bacde'   FIFO
```

## defaultdict

class collections.**defaultdict**\(\[_default\_factory\[, ...\]\]_\)

dict subclass that calls a factory function to supply missing values

## Counter

