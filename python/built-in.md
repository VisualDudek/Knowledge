# Built-in

## id\(\)

**id**\(_object_\)

Two objects with non-overlapping lifetimes may have the same `id()` value.

```python
a = [1,2,3]
b = a
id(a) == id(b) # True
a[0] = 5
id(a) == id(b) # still True

# Be carefull with mutable objects
```

## dir\(\)

**dir**\(\[_object_\]\)

Without arguments, return the list of names in the current local scope. With an argument, attempt to return a list of valid attributes for that object.

```python
import functools

dir(functools) # show the names in the struct module
```

## str\(\)

str.**zfill**\(_width_\)

Return a copy of the string left filled with ASCII `0` digits to make a string of length _width_.

## zip\(\)

**zip**\(_\*iterables_\) -&gt; iterator of tuples

U may want to `dict()` or `list()` iterator



