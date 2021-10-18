# Built-in Types

## set

Curly braces or the `set()` function can be used to create sets. Note: to create an empty set you have to use `set()`, not `{}`; the latter creates an empty dictionary, a data structure that we discuss in the next section.

### set.union() or |

## dict

* from Python 3.7 legacy dict is also `OrderedDict`
* keys need to be hashable, see how many diff. kind of objects can be keys in dict.

```python
a = dict(one=1, two=2, three=3)
b = {'one': 1, 'two': 2, 'three': 3}
c = dict(zip(['one', 'two', 'three'], [1, 2, 3]))
d = dict([('two', 2), ('one', 1), ('three', 3)])
e = dict({'three': 3, 'one': 1, 'two': 2})
a == b == c == d == e # True
```

```python
d[('abc', 123)] = "tuples can be"

class Foo:
    def __init__(self, value):
        self.value
        
my_object = Foo(5)
d[my_object] = "instance of class can be"
```

The objects returned by `dict.keys()` , `dict.values()` and `dict.items()` are _view objects_.

**x in dictview - **if looking for items x should be a tuple, `k, v in dictview`

**get**(_key\[, default]_) - in that way `KeyError` is never raised.

**update**(\[_other_]) - Insert multiple items at once with key/value pairs but also list of tuples.

**pop**(_key\[, default]_)

**copy**() - shallow copy

**fromkeys**(_iterable\[, value]_) - tip: if you need distinct values use a dict comprehension instead.

**dict comprehension** - `{x: x**2 for x in (2, 4, 6)}`
