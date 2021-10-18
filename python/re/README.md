# regex

## intro

```python
import re

pattern = r'pattern'

match = re.findall(pattern, test_string)

print("Number of matches :", len(match))
```

## re.sub()

re.**sub**(_pattern, repl, string, count=0, flags=0_)

Return the string obtained by replacing the leftmost non-overlapping occurrences of _pattern_ in _string_ by the replacement _repl_.  If the pattern isn’t found, _string_ is returned unchanged. _repl_ can be a string or a function.

```python
import re

def solve(s):
    return re.sub(r'[A-Za-z0-9]+',lambda mo: mo.group(0).capitalize(), s)
```
