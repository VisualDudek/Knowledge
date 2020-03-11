# Names and Scopes

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

\`\`

