# OOP

## show dunder and display help

```python
dir(list)

help(list.__add__)
```

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

## @classmethod Multiple Class constructors

essentialy it says that the method can be called on a class instead of an instantiated object

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

## how to avoid immediate initailization

How to avoid immediate initaialization after importing module? BC when U import below module Database\(\) obj will be initalized.

```python
class Database:
    # the database implementation
    pass
    
database = Database()
```

```python
class Databse:
    # the database implementation
    pass
    
database = None

def initialize_database():
    global database
    database = Database()
```

## extending built-ins

```python
class LongNameDict(dict):
    def longest_key(self):
        longest = None
        for key in self:
            if not longest or len(key) > len(longest):
                longest = key
        return longest
```

```python
# usage:
logkeys = LongNameDict()
longkeys['hello'] = 1
longkeys['longest yet'] = 5
longkeys['hello2'] = 'world'
longkeys.longest_key()  # 'longest yet'
```

## \_\_lt\_\_

silly class that can be sorted based on either a string or a number

* method _less than_, should return bool

```python
class WierdSortee:
    def __init__(self, string, number, sort_num):
        self.string = string
        self.number = number
        self.sort_num = sort_num
        
    def __lt__(self, object):
        if self.sort_num:
            return self.number < object.number
        return self.string < object.string
```

## Mixin

A mixin is a superclass that is not intended to exist on its own, but is meant to be inherited by some other class to provide extra functionality.

* mixin can operate on other class atributes -&gt; atributes names must be the same

```python
# setup
class Contact():
    def __init__(self, name, email):
        self.name = name
        self.email = email
        
# mixin
class MailSender:
    def send_mail(self, message):
        print("Sending mail to" + self.email)
        # Add e-mail logic here
        
# usage
class EmailableContact(Contact, MailSender):
    pass
```

```python
# mixin in action
e = EmailableContact("Marcin", "email@gmail.com")
e.send_mail("Hello mate")
```

## Abstract base classes

ABCs define a set of methods and properties that a class must implement in order to be considered a duck-type instance of that class. The class can extend the abstract base class itself in order to be used as an instance of that class, but it must supply all the appropriate.

## dataclass

Really nice syntax sugar, decorator function if you need plain function use `make_dataclass`

* instances can be compared when using: `@dataclass(order=True)`

```python
from dataclasses import dataclass

@dataclass
class InventoryItem:
    """Class for keeping track of an item in inventory"""
    name: str
    unit_price: float
    quantity_on_hand: int = 0
```

```python
# mutable object as default value
@dataclass
class C:
    mylist: list[int] = field(default_factory=list)
```

### type hinting

```python
@dataclass
class Node:
    value: int
    left: InitVar["Node"] = None
    
# MISTAKE
    left: IntVar[Node] = None   # This self ref of note will not work
                                #see PEP 484: The problem of forward declaration
```

## dataclasses.make\_dataclass\(\)

