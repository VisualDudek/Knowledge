---
description: Why using @classmethod
---

# @classmethod

### src

* [Ptyhon classmethod](https://www.programiz.com/python-programming/methods/built-in/classmethod) @programiz
* Python's Instance, Class, and Static Methods Demystified [link](https://realpython.com/instance-class-and-static-methods-demystified/) @RealPython
* The Factory Method Pattern and Its Implementation in Python [link](https://realpython.com/factory-method-python/) @RealPython

## MAIN

The difference between a static methods and classmethod is:

* Static knows nothing about the class and just deals with the parameters
* Class method works with the class since its parameter is always the class itself

The class method can be called both by the class and its object:

```python
Class.classmethod()
# or even
Class().classmethod()
```

Factory Methods - design pattern, are those methods wich return a class object \(like constructor\) for different use cases.

```python
class Person:
    def __init__(self, name, age):
        self.name
        self.age
        
    @classmethod
    def fromBirthYear(cls, name, birthYear):
        return cls(name, date.today().year - birthYear)
```

above we have two class instance creator

* constructor `__init__`
* `fromBirthYear` method

#### Correct instance creation in inheritance

```python
@staticmethod
def fromFathersAge(name, fatherAge, fatherPersonDiff):
    return Person(name, date.today().year - fatherAge + fatherPersonDiff)
    
class Man(person):
    sex = 'Male'
```

```python
man0 = Man.fromBirthYear('John', 1985)
isinstance(man0, Man) -> True
man1 = Man.fromFathersAge('John', 1965, 20)
isinstance(man1, Man) -> False
```

