# OOP

## Property value

* user can easily break below code in following way: `c = Circle(4); c.pi = 4`
* using `@property` we can create funcion member that return const value and can be called without parenthesis as if it was a variable.
* `property` is a build-in func

```python
class Circle():
    def __init__(self, radius):
        self.radius = radius
        self.pi = 3.14
        
    def area(self):
        return self.pi * slef.radius**2
```

```python
...
    @property
    def pi(self):
        return 3.14
```

## Methods

We can think of the method as a funciton attached to a class. The self parameter is a specific insatance of that class. When you call the method on two different objects, you are calling the same method twice, but p assing two different objects as the self parameter.

## Multiple Class constructors

* _function_ _overloading_ is not allowed in Python
* workaround: `@classmethod`

```python
class Date():
    def __init__(self, day, month, year):
        self.day = day
        self.mont = month
        self.year = year
        
    # workaround of constructor overloading
    @classmethod
    def fromString(obj, s):    # s -> dd/mm/yyyy
        day = int(s[:2])
        month = int(s[3:5])
        year = int(s[6:])
        return obj(day, month, year) 
```

## User defined iterator

based on class, SEE itertools&gt;&gt;User defined iterator

## Accessing a class as a list

you just need to implement dunder `getitem` and `setitem` methods

```python
x = l[idx]
# is the same as writing:
x = l.__getitem__(idx)

l[idx] = x
# is the same as writing:
l.__setitem__(idx,x)
```

a list whose size cannot be changed, and with index start at 1

```python
class MyList():
    def __init__(self, dimension):
        self.l = [0 for i in range(dimension)] # allocate list
        
    def __getitem__(self, idx):
        return self.l[idx-1] # map index to underhool legacy list
        
    def __setitem__(self, idx, data):
        self.l[idx-1] = data
```

