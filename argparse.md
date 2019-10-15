# argparse

## Main

`os.path.isdir(path)`

Return `True` if path is an existing directory.

#### The Basics

```python
import argparse
parser = argparse.AgrumentParser()
args = parser.parse_args()
```

#### Displaying Help

```python
# Create the parser
my_parser = argparse.ArgumentParser(prog='mysl',
    usage=f'{prog}s [options] path',
    description='List the content of folder',
    epilog='Text to display after the argument help',
)
```

#### Customizing Prefix Chars

By default, the standard prefix char is the dash `-` character. can be customize by using the `prefix_chars` keyword.

#### Setting Prefix Chars for Files That Contain Arguments

`fromfile_prefix_chars='@'`

\`\`

