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

