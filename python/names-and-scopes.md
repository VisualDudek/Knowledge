# Names and Scopes

## Based on

Real Python Scope & the LEGB Rule [link](https://realpython.com/python-scope-legb-rule/#using-enclosing-scopes-as-closures)

## Names and Scopes

 the **scope** of a name defines the area of a program in which you can unambiguously access that name, such as variables, functions, objects, and so on. A name will only be visible to and accessible by the code in its scope.

 a Python scope determines where in your program a name is visible. Python scopes are implemented as _dictionaries_ that map names to objects. These _dictionaries_ are commonly called **namespaces**. These are the concrete mechanisms that Python uses to store names. They’re stored in a special attribute called `__dict__`.

```python
import sys
sys.__dict__.keys()

# This returns a list with all the names defined at the top level of the module.
#In this case, you can say that __dict__ holds the namespase of sys
```

You can reference names in at least two different ways:

1. dot notation `module.name`
2. subscription operation on `__dict__` in the form `module.__dict__['name']`

 You can inspect the names and parameters of a function using `.__code__`, which is an attribute that holds information on the function’s internal code.

```python
def cube(base):
     result = base ** 3
     print(f'The cube of {base} is: {result}')
     
cube.__code__.co_varnames
```

 Notice that the names in `builtins` are always loaded into your global Python scope with the special name `__builtins__`, as you can see in the following code: `dir(__builtins__)`
