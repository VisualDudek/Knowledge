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

* stackoverflow  [Mocking a class: Mock() or patch()?](https://stackoverflow.com/questions/8180769/mocking-a-class-mock-or-patch)

## Mistakes

* if U do not include `if ... main ... unittest.main()` and run from CLI plain `python -m unittest` no test will be run but if you run `python -m unittest discover -v` will be ok

## assert stmt

* it is not part of unittest module but keep in mind

```python
assert True  # nothing happens

assert False, 'smth went wrong'  # will raise AssertionError: smth went wrong
```

## Skipping tests and expected failures

@unittest.**skip**(_reason_str_)

@unittest.**skipIf**(_condition_, _reason_str_)

@unittest.**skipUnless**(_condition_, _reason_str_)

@unittest.**expectedFailure           **#not supported by vscode

## unittest.main()

`main()` accept kwargs e.g. `unittest.main(verbosity=2)`

## VSCode

1. run: Python: Configure Tests
   1. all setting are stored in settings.json, those are the same args U can run from CLI
2. remember that you need import func/obj that need to be tested
