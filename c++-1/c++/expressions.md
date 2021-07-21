# EXPRESSIONS

## Summary

* built-in operators
* prefix and postfix operator the return of expresion is related to pre or post
* `typeid` form `<typeinfo>`
* overloading operator `new`
* user-defined literals
* type conversion of own user-defined types
* `constexpr`
* `voiatile`

## Operators

### Logical Operators

```cpp
// bitwise operators

// AND 
x & y // 0b1100 & 0b1010 = 0b1000
// OR
x | y // 0b1100 | 0b1010 = 0b1110
// XOR
x ^ y // 0b1100 ^ 0b1010 = 0b0110
// bitwise complement
~x // ~0b1010 = 0b0101
// bitwise left shift
x << y
// bitwise right shift
x >> y
```

### operator promotion

e.g. for unary arithmetic operators if the operand is of type `bool, char or short int` the result of the expression is an `int` 

### member access operators

```cpp
// member access operators
[ ] subscript
* indirection
& address-of
. member-of-object
-> member-of-pointer
.* pointer-to-member-of-object
->* pointer-to-member-of-pointer
```

### ternaty conditional aka Elvis operator

The _ternary conditional operator_ `x ? y : z` is a lump of **syntactic sugar**.

### comma

It allows several expressions separated by commas to be evaluated within a larger expression.

```cpp
int confusing(int &x) {
    return x = 9, x++, x / 2;
}

int main() {
    int x{};
    auto y = confusing(x);
    // after invoking confusing, x equals 10 and y equals 5
}
```







a











