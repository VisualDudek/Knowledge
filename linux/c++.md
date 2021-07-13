# C++

## hello, world

```cpp
#include <cstdio>

int main() {
    printf("Hello, world!");
    return 0;
}
```

* _.cpp_ extension
* single entry point called the `main` function
* exit code `return 0;`
* `printf` is defined in the `cstdio` library

## Compiler

1. preprocessor
2. compiler
3. linker

## if

```cpp
if (boolean-expression) {
    statement1;
    statement2;
    --snip--
}

if (boolean-expression) stmt-1;
else if (boolean-2) stmt-2;
else stmt-3;
```

## Functions

```cpp
return-type function_name(par-type1 par_name1, par-type2 par_name2) {
    --snip--
    return return_value;
}
```

## printf

{% hint style="info" %}
another option to put smth on standard output is `cout` which is part of `iostream` library
{% endhint %}

### format specifiers

`%x` hexadecimal and `%o` octal representation.

## Comments

```cpp
// This comment is on its own line

/*
 * This is a commment
 * with multiple
 * lines, Don't forget to close
 */
```

## libraries

### cstdio

* printf

### iostream

* cout

### string

## literal

A literal is a hardcoded value in a program:

`binary` uses the prefix `0b` 

`octal` -&gt; `0`

`decimal` -&gt; this is the default

`hexadecimal` uses the prefix `0x`

{% hint style="info" %}
Integer literal can contain any number of single quotes ' for readability. These are completely ignored by the compiler. For example, `1000000` and `1'000'000` are both integer equal to one million.
{% endhint %}

## debugging, gdb

? need to compile with debug support, using `-g` flag.

`$ g++ main.cpp -g`

## other

### declaration vs initialization

Declaration, generally, refers to the introduction of a new name in the program. For example, you can _declare_ a new function by describing it's "signature":

```cpp
void foo();
class klass;
struct ztruct;
int x;
```

Initialization refers to the "assignment" of a value, at construction time. 

