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

## AND, OR

The logical operators AND `&&` and OR `||` are binary.

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

As a general rule, use `%g` to print floating-point types.

There is no format specifiers for `bool`, but you can use the `int` format spec. `%d` to yield a `1` for ture and a `0` for false.

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

## literals

A literal is a hardcoded value in a program:

`binary` uses the prefix `0b` 

`octal` -&gt; `0`

`decimal` -&gt; this is the default

`hexadecimal` uses the prefix `0x`

{% hint style="info" %}
Integer literal can contain any number of single quotes ' for readability. These are completely ignored by the compiler. For example, `1000000` and `1'000'000` are both integer equal to one million.
{% endhint %}

A _character literal_ is a single, constant character. Single quotation marks \(' '\) surround all characters. If the character is any type but `char`, you must also provide a prefix: `L` for `wchar_t`, `u` for `char16_t`, and `U` for `char32_t`. For example, 'J' declares a `char` literal and `L'J'` declares a `wchar_t`.

Unicode character literals: `\u` followed by a 4-digit Unicode code or the prefix `\U` followed by an 8-digit Unicode code.

```cpp
'\u0041' // A
'\U0001F37A' // beer mug character
```

## Escape Sequences

## Types

System programmers sometimes work directly with _raw memory_, which is a collection of bits without a type. Employ the `std::byte` type, available in the `<cstddef>` header, it permits bitwise logical operations and little else.

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

### unary, binary and ternarry operator

A _unary_ operator takes a single operand: the unary negation operator `!` takes a single operand and returns its opposite.

