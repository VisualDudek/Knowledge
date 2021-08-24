# collections

* must see example with deque

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

class collections.**defaultdict**\(_default\_factory=None,/\[, ... \]_\)

dict subclass that calls a factory function to supply missing values, Usually a Python dict throws a `KeyError` if you try to get an item with a key that is not currently in the dictionary. The `defaultdict` in contrast will simply create any item that you try to access \(provided of course they do not exist yet\). To create such a "default" item, it calls the function object that you pass to the constructor.

* `d = defaultdict(int)` int wskazuje na typ value w parze key:value
* dla value int `d[k] += 1` ale dla value list `d[k].append(obj)` uwaga na to

```python
# do not overkill via default_factory, there are simple solutions:

d = defaultdict(int)
for x in keyword.kwlist:
    d[x] = len(x)
```

### defaultdict with default value 1

```python
d = defaultdict(lambda:1)

# short but not exact solution is that it works underhood:
d = {}
d = defaultdict(lambda:0, d)
```

```python
def a():
    return 1
    
d = defaultdict(a)

# more
def constant_factory(value):
    return lambda: value
    
d = defaultdict(constant_factory('<missing>'))
d.update(name='John', action='ran')
print(f'{name} {action} to {location}')
# >>> 'John ran to <missing>'
```

### with user default\_factory

```python
num_items = 0

def tuple_coutner():
    global num_items
    num_items += 1
    return (num_items, [])
    
d = defaultdict(tuple_counter)

d['a'][1].append("hello")
```

## Counter

counter.**most\_common**\(\[_n_\]\) -&gt; list

Elements with e**qual counts are ordered in the order first encountered.**

## namedtuple\(\)

**namedtuple**\(_typename_, _field\_names_, \*, _rename=False, defaults=None, module=None_\)

return a new tuple subclass named typename and positional or keyword arguments.

* can unpack like a regular tuple
* odrobine przypomina POD z positional and kwargs boost
* \(python doc\) Named tuples are especially useful for assigning field names to result tuples returned by the csv or sqlite3 modules, SEE: **usage with map `somenamedtuple._make(iterable)`**
* can return dict which maps field names to their corresponding values
  * `somenamedtuple._asdict()`
* powerfull default option, remember that it need to be iterable e.g. list of values

```python
Point = namedtuple('Point', ['x', 'y'])
p = Point(11, y=22)
#can unpack
x, y = p
p[0] == p.x
```

```python
EmployeeRecord = namedtuple('EmployeeRecord', 'name, age, title, department, paygrade')

import csv
for emp in map(EmployeeRecord._make, csv.reader(open("employees.csv", "rb"))):
    print(emp.name, emp.title)
```

```python
# defaults values
Data = namedtuple('Data', ['cmd', 'arg'], defaults=[0])
```

