# Built-in Types

## dict

```python
a = dict(one=1, two=2, three=3)
b = {'one': 1, 'two': 2, 'three': 3}
c = dict(zip(['one', 'two', 'three'], [1, 2, 3]))
d = dict([('two', 2), ('one', 1), ('three', 3)])
e = dict({'three': 3, 'one': 1, 'two': 2})
a == b == c == d == e # True
```

The objects returned by `dict.keys()` , `dict.values()` and `dict.items()` are _view objects_.

**x in dictview -** if looking for items x should be a tuple

**get**\(_key\[, default\]_\) - in that way `KeyError` is never raised.

**update**\(\[_other_\]\) - Insert multiple items at once with key/value pairs but also list of tuples.

**pop**\(_key\[, default\]_\)

