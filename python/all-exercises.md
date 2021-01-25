# All Exercises

## EXERCISES

### Built-in

* show that shallow copy of list has same reference -&gt; changing one element in copy will affect original
  * make deep copy and show diff id numbers
  * show names in copy module scope
* count backwards using range and reversed.
* input string of numerics convert to int using map
* generate random \(x, y\) int tuples and sort items \(x,y\) based on y using lambda

### Built-in Types

* create dict using zip and iterables
* create dict only keys with same value
* handle `KeyError` via exception on missing key in dict
  * via dict method
* do defaultdict exercises from collections section

1. Create dict with number as key and value as lowercase assci.

### collections

* show how defaultdict avoid raising `KeyError`
* generate tuple of \(x, y\) where x is A, B or C and y is random.choice of lower ascii letters next create dictionary with x-keys and y list of values
* init defaultdict and create dict where key is Python keyword and value is len of key, do not overkill via `default_factory`

### Closures

* create inner function with nonlocal vars, show local and nonlocal vars, create closure
  * show what name closure inherits
  * explain free var term and etymology of Closure
* create closure tat calculate mean, show `co_freevars`

### f-String

* convert int o hex and binary using f-String
* centered, centered and fill

### itertools

* generate indexes pairs for substring 

### Modules

* Print upper and lowercase letters.

### Common Errors

* `.split` insead of `.split()`

## SOLUTIONS

### Built-in Types

```python
d = { i:name for i, name in enumerate(string.ascii_lowercase) } # 1


```

