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

