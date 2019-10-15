# argparse

## Main

{% hint style="info" %}
Check for later /argparse
{% endhint %}

`os.path.isdir(path)`

Return `True` if path is an existing directory.

`vars([object])`

Return the `__dict__` attribute for a module, class, instance or any other object with `__dict__` attribute.

```python
args = my_parser.parse_args()
print(vars(args))
```

### The Basics

```python
import argparse
parser = argparse.AgrumentParser()
args = parser.parse_args()
```

### Displaying Help

```python
# Create the parser
my_parser = argparse.ArgumentParser(prog='mysl',
    usage=f'{prog}s [options] path',
    description='List the content of folder',
    epilog='Text to display after the argument help',
)
```

### Customizing Prefix Chars

By default, the standard prefix char is the dash `-` character. can be customize by using the `prefix_chars` keyword.

### Setting Prefix Chars for Files That Contain Arguments

`fromfile_prefix_chars='@'`

### Abbreviations

```python
my_parser.add_argument('--input', action='store', type=int, required=True)
my_parser.add_argumet('-i', action='store', type=int)
```

The optional parameters can always be shortened unless the abbreviation can led to an incorrect interpretation. You can always disable this feature:

`my_parser = argparse.ArgumentParser(allow_abbrev=False)`

### Name of Flag for Argument

??? Does `action='store_true'` set this arg as optional? The default value for `action` is `'store'`

{% hint style="info" %}
Syntactically, the difference between positional and optional arguments is that optional arguments start with - or --, while positional arguments don’t.
{% endhint %}

### Setting the Action

```python
# custom_action.py
import argparse

class VerboseStore(argparse.Action):
    def __init__(self, option_strings, dest, nargs=None, **kwargs):
        if nargs is not None:
            raise ValueError('nargs not allowed')
        super(VerboseStore, self).__init__(option_strings, dest, **kwargs)

    def __call__(self, parser, namespace, values, option_string=None):
        print('Here I am, setting the ' \
              'values %r for the %r option...' % (values, option_string))
        setattr(namespace, self.dest, values)

my_parser = argparse.ArgumentParser()
my_parser.add_argument('-i', '--input', action=VerboseStore, type=int)

args = my_parser.parse_args()

print(vars(args))
```

This example defines a custom action that is just linke `store` action but little more verbose.



