# All in one

## dis Module

### x = y = 5

```python
import dis
dis.dis('x=y=5')
'''
  1           0 LOAD_CONST               0 (5)  # place on stack 5
              2 DUP_TOP                         # duplicate top so now
                                                #we have 5 and 5 on stack
              4 STORE_NAME               0 (x)  # store top of stack in x
              6 STORE_NAME               1 (y)  # store top of stack in y
              8 LOAD_CONST               1 (None)
             10 RETURN_VALUE
'''
```

### foo = foo\[0] = \[0]

{% embed url="https://stackoverflow.com/questions/32156515/python-multiple-assignment-statements-in-one-line" %}

### a, b = a\[b] = { }, 5

## functions

functions in Python are first class objects, which means you can pass them to other functions as arguments, return them from other functions as values, and store them in variables and data structures.

```python
def myfunc(a, b):
    return a + b
    
funcs = [myfunc]
funcs[0](2,3)
```

## hashlib

`hashlib.sha256(b'asdf').hexdigest()`

## Operator precedence

[https://docs.python.org/3/reference/expressions.html](https://docs.python.org/3/reference/expressions.html)\\

## return None; usecase dis lib

each func in python has implicite return None at the end

```python
import dis

def x():
    pass
    
>>> dis.dis(x)
0 LOAD_CONST    (None)
2 RETURN_VALUE
```

## path of script

```python
import os

SCRIPT_DIR = os.path.dirname(os.path.realpath(__file__))
```

## dict().get as a Alternative to if/elif/else

switch case, prototypy musza byc te same np, ta sama liczba argumentow.

```python
class foo():
    def __init__(self):
        choice = input('Choose number:')
        {
            '1': self.a,
            '2': self.b,
        }.get(choice, self.default)()

        # ^^^--- similar to:
        if choice == '1':
            self.a()
        elif choice == '2':
            self.b()
        else:
            self.default()

    def a(self):
        print(f'a func')

    def b(self):
        print(f'b func')

    def default(self):
        print(f'default')

    
if __name__ == "__main__":
    test = foo()
```

### emulating Switch/Case

* one downsize is that each call of dispatch\_dict new instance of dict is created, can be mitigated via storing it in var to avoid crating it each time

```python
def dispatch_dict(operator, x, y):
    reutrn {
        'add': lambda: x + y,
        'sub': lambda: x - y,
        'mul': lambda: x * y,
    }.get(operator, lambda: None)()
```

## Shallow and Deep Copy

issue is pass by reference

copy.**copy**(_x_)

copy.**deepcopy**(_x\[, memo]_)

## f-String

```python
f'{number:b}'
f'{number:>10}'
n = 10
f'{number:>{n}}'
```

first format than conversion: `{var:^10x}` NOT `{var:x^10}` bc this means fill, to sum up fill, align, conversion

{% embed url="https://docs.python.org/3/library/string.html#formatspec" %}

## for-loop syntax sugar

```python
for x in alist:
    #do smth with x
    
it = alist.iterator()
while it.has_next():
    x = it.next()
    #do something with x
```

## Python Keywords

Only two methods `keyword.iskeyword(s)` and `keyword.kwlist`

```python
import keyword
print(keyword.kwlist)
```

## pyenv

```
# You need to install also pyenv-virtualenv
#remember to install build dependencies before attempting to install a new Python version
pyenv versions    # List all Python versions available to pyenv
pyenv install --list     # To create a virtual env, you need first to make sure
#that you have a suitable interpreter installed

pyenv shell 3.8.0    # Set the shell-specific Pyhon version
pyenv virtualenv 3.8.0 my-data-project    # Create virtual env
pyenv local my-data-project    # Set virtual env for this dir and auto-load

# add pyenv plugin into .zshrc
```

## Underscores

**Separating Digits of Number**

```python
x = 1_000
y = 1000
a == b # True
```

## unpack

```python
first, *rest = [1,2,3,4,5,6]
```

## generators

* zletą jest możliwość pobierania danych wejściowych dowolnej wielkości

```python
# example
def index_words_iter(text):
    if text:                    # nice trick to have bgn value of 0
        yeld 0
    for index, letter in enumerate(text):
        if letter == ' ';
            yield index + 1
```

```python
# any size of input
def index_file(handle):
    offset = 0
    for line in handle:
        if line:
            yeld offset
        for letter in line:
            offset += 1
            if letter == ' ':
                yield offset
```

## string interning

* u can help yourself with `dis` module to see if string was optimized

```python
a = ''.join(['a','b','c'])
b = 'a' + 'b' + 'c'
c = 'abc'
```

## string Module

string.**ascii\_letters**

The concatenation of the `ascii_lowercase` and `ascii_uppercase`

## textwrap Module

textwrap.**wrap**(_text_, _width=70_, _\*\*kwargs_) -> _list_

## Ternary operator

keep in mind that expr1 mentioned below can be `assignment` or `return`

I really like tuple use case! :)

```python
# <expr1> if <conditional_expr> else <expr2>

age = 12
s = 'minor' if age < 21 else 'adult'

# Python program to demonstrate ternary operator 
a, b = 10, 20
  
# Use tuple for selecting an item 
print( (b, a) [a < b] ) 
  
# Use Dictionary for selecting an item 
print({True: a, False: b} [a < b]) 
  
# lamda is more efficient than above two methods 
# because in lambda  we are assure that 
# only one expression will be evaluated unlike in 
# tuple and Dictionary 
print((lambda: b, lambda: a)[a < b]()) 
```

## Walrus operator :=

* just do not compute expresion twice
* need to be inside parenthesies ( )
* have the lowest operator precedence
* advantage in comprehance structures, where your condition is external function

```python
if func(4) > 5:
    print(func(4)) # finc() is expensive so I should not call it twice
    
# better
storage = func(4)
if storage > 5:
    print(storage) # code lines overhead
    
# better
if ( x := func(4) ) > 5:
    print(x)
```

```python
# advanced use-case
def get_batches(count, size)
    return count // size

found = {name: get_batches(stock.get(name, 0), 8)
        for anme in order
        if get_batches(stock.get(name, 0), 8)}
    
# here comes warlus
found = {name: batches for name in order
        if (batches := get_batches(stock.get(name, 0), 8))}  
```

## Zen

```python
import this
# also
import antigravity
```
