---
description: 'based on: https://realpython.com/primer-on-python-decorators/#further-reading'
---

# Decorators

* Decorators are implemented using closures
* why using `functools.wraps()` istead of my own clousure?

## Simple Decorators

Use decorators in a simpler way with @ symbol, sometimes called the "pie" syntax.

```bash
def my_decorator(func):
    def wrapper():
        print("Something is happening before the function is called.")
        func()
        print("Something is happening after the function is called.")
    return wrapper

def say_whee():
    print("Whee!")

# Syntactic Sugar -> @ symbol
say_whee = my_decorator(say_whee)
```

### Returning Values From Decorated Functions

```bash
def do_twice(func):
    def wrapper_do_twice(*args, **kwargs):
        func(*args, **kwargs)
        return func(*args, **kwargs)
    return wrapper_do_twice
```

## A Few Real World Examples

### Template

```bash
import functools

def decorator(func):
    @functools.wraps(func)
    def wrapper_decorator(*args, **kwargs):
        # Do something before
        value = func(*args, **kwargs)
        # Do something after
        return value
    return wrapper_decorator
```

### Timing Functions

```bash
import functools
import time

def timer(func):
    """Print the runtime of the decorated function"""
    @functools.wraps(func)
    def wrapper_timer(*args, **kwargs):
        start_time = time.perf_counter()
        value = func(*args, **kwargs)
        end_time = time.perf_counter()
        run_time = end_time - start_time
        print(f"Finished {func.__name__!r} in {run_time:.4f} secs")
        return value
    return wrapper_timer

@timer
def waste_some_time(num_times):
    for _ in range(num_times):
        sum([i**2 for i in range(10000)])
```

### Debugging Code

```bash
import functools

def debug(func):
    """Print the function signature and return value"""
    @functools.wraps(func)
    def wrapper_debug(*args, **kwargs):
        args_repr = [repr(a) for a in args]
        kwargs_repr = [f"{k}={v!r}" for k, v in kwargs.items()]
        signature = ", ".join(args_repr + kwargs_repr)
        print(f"Calling {func.__name__}({signature})")
        value = func(*args, **kwargs)
        print(f"{func.__name__!r} returned {value!r}")
        return value
    return wrapper_debug
```

### Registering Plugins

```bash
import random
PLUGINS = dict()

def register(func):
    """Register a function as a plug-in"""
    PLUGINS[func.__name__] = func
    return func

@register
def say_hello(name):
    return f"Hello {name}"

@register
def be_awesome(name):
    return f"Yo {name}, together we are the awesomest!"

def randomly_greet(name):
    greeter, greeter_func = random.choice(list(PLUGINS.items()))
    print(f"Using {greeter!r}")
    return greeter_func(name)
```

## Fancy Decorators

### Decorating Classes

**Decorate the methods of a class**. Some commonly used decorators that are even built-ins Python are `@classmethod` `@staticmethod` and `@property` 

#### Example using built-in class decorators

```python
class Circle:
    def __init__(self, radius):
        self._radius = radius

    # decorator turns radius method into a "getter" for a read-only
    #attribute with the same name.
    @property
    def radius(self):
        """Get value of radius"""
        return self._radius

    # mutable property: it can be set to a different value
    #However, by defining a setter method, we can do some 
    #error testing to make sure it's not set to a nonsensical
    #negative value.
    @radius.setter
    def radius(self, value):
        """Set radius, raise error if negative"""
        if value >= 0:
            self._radius = value
        else:
            raise ValueError("Radius must be positive")

    # an immutable property, properties without .setter() methods
    #can't be changed. Even though it is defined as a method, 
    #it can be retrieved as an attribute without parentheses.
    @property
    def area(self):
        """Calculate area inside circle"""
        return self.pi() * self.radius**2

    # regular method
    def cylinder_volume(self, height):
        """Calculate volume of cylinder with circle as base"""
        return self.area * height

    # is a class method. It’s not bound to one particular instance 
    #of Circle. Class methods are often used as factory methods 
    #that can create specific instances of the class.
    @classmethod
    def unit_circle(cls):
        """Factory method creating a circle with radius 1"""
        return cls(1)

    # static method. It’s not really dependent on the Circle class, 
    #except that it is part of its namespace. 
    #Static methods can be called on either an instance or the class.
    @staticmethod
    def pi():
        """Value of π, could use math.pi instead though"""
        return 3.1415926535
```

**Decorate the whole class.** This is , for example, done in the new `dataclasses` module in Python 3.7

```python
from dataclasses import dataclass

@dataclass
class PlayingCard:
    rank: str
    suit: str = 'abc'
    
# This is the same as:
def __init__(self, rank: str, suit: str = 'abc'):
```

### Nesting Decorators

```python
@debug
@do_twice
def greet(name):
    print(f"Hello {name}")
```

Think about this as the decorators being executed in the order they are listed. In other words, `@debug` calls `@do_twice` which calls `greet()` or `debug(do_twice(greet()))`

### Decorators With Arguments

Sometimes, it's useful to pass arguments to your decorators. For instance, `@do_twice` could be extended to a `@repeat(num_times)` decorator.

```python
def repeat(num_times):
    def decorator_repeat(func):
        @functools.wraps(func)
        def wrapper_repeat(*args, **kwargs):
            for _ in range(num_times):
                value = func(*args, **kwargs)
            return value
        return wrapper_repeat
    return decorator_repeat
```

all you need is that `repeat(num_times=4)` to return a function object that can act as a decorator!

### Both Please

decorators can be used both with and without arguments. when a decorator uses arguments, you need to add an extra outer function. The challenge is for your code to figure out if the decorator has been called with or without arguments.

 \(...\) hard times \(...\)

### Stateful Decorators

Sometimes, it’s useful to have **a decorator that can keep track of state**. As a simple example, we will create a decorator that counts the number of times a function is called.

we talked about pure functions returning a value based on given arguments. Stateful decorators are quite the opposite, where the return value will depend on the current state, as well as the given arguments.

 Recall that the decorator syntax `@my_decorator` is just an easier way of saying `func = my_decorator(func)`. Therefore, if `my_decorator` is a class, it needs to take `func` as an argument in its `.__init__()` method. Furthermore, the class needs to be [callable](https://docs.python.org/reference/datamodel.html#emulating-callable-objects) so that it can stand in for the decorated function.

 For a class to be callable, you implement the special `.__call__()` method:

## Exercises

* Show that simple decorator do not inherit original func name and doc string, show solution how to fix it.
* Expand Syntactic Sugar
* Write decorator that register func in PLUGINS dict

