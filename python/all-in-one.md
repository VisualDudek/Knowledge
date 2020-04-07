# All in one

## dict\(\).get as a Alternative to if/elif/else 

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

## Shallow and Deep Copy

copy.**copy**\(_x_\)

copy.**deepcopy**\(_x\[, memo\]_\)

## f-String

```python
f'{number:b}'
f'{number:>10}'
n = 10
f'{number:>{n}}'
```

first format than conversion: `{var:^10x}` NOT `{var:x^10}` bc this means fill, to sum up fill, align, conversion 

## Python Keywords

Only two methods `keyword.iskeyword(s)` and `keyword.kwlist`

```python
import keyword
print(keyword.kwlist)
```

## Underscores

**Separating Digits of Number**

```python
x = 1_000
y = 1000
a == b # True
```

## string Module

string.**ascii\_letters**

The concatenation of the `ascii_lowercase` and `ascii_uppercase` 

## textwrap Module

textwrap.**wrap**\(_text_, _width=70_, _\*\*kwargs_\) -&gt; _list_

## Ternary operator

keep in mind that expr1 mentioned below can be `assignment` or `return`

I really like tuple use case! :\)

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

## Zen

```python
import this
```

