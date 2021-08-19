# unittest

## MAIN

#### src

* [pydocs](https://docs.python.org/3/library/unittest.html)

### .mock

#### src

* [pydocs](https://docs.python.org/3/library/unittest.mock-examples.html?highlight=mock) - getting started
* see for later - mock request
* Understanding the Python Mock Object Library @RealPython [link](https://realpython.com/python-mock-library/)

What is the difference between `mock` vs `patch` ?

* stackoverflow  [Mocking a class: Mock\(\) or patch\(\)?](https://stackoverflow.com/questions/8180769/mocking-a-class-mock-or-patch)

## assert stmt

* it is not part of unittest module but keep in mind

```python
assert True  # nothing happens

assert False, 'smth went wrong'  # will raise AssertionError: smth went wrong
```

## VSCode

1. run: Python: Configure Tests
   1. all setting are stored in settings.json, those are the same args U can run from CLI

