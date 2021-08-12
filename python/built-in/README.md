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

`==` operator compares the values, while the `is` operator compares the identities \(i.e. memory addresses\)

## dir\(\)

**dir**\(\[_object_\]\)

Without arguments, return the list of names in the current local scope. With an argument, attempt to return a list of valid attributes for that object.

```python
import functools

dir(functools) # show the names in the struct module
```

## input\(\)

**input**\(\[_prompt_\]\)

## reversed\(\)

**reversed**\(_seq_\)

 Return a reverse [iterator](https://docs.python.org/3/glossary.html#term-iterator). _seq_ must be an object which has a [`__reversed__()`](https://docs.python.org/3/reference/datamodel.html#object.__reversed__) method or supports the sequence protocol \(the [`__len__()`](https://docs.python.org/3/reference/datamodel.html#object.__len__) method and the [`__getitem__()`](https://docs.python.org/3/reference/datamodel.html#object.__getitem__) method with integer arguments starting at `0`\).

## str\(\)

str.**zfill**\(_width_\)

Return a copy of the string left filled with ASCII `0` digits to make a string of length _width_.

str.**center**\(_width_\[, _fillchar_\]\)

## zip\(\)

**zip**\(_\*iterables_\) -&gt; iterator of tuples

U may want to `dict()` or `list()` iterator



