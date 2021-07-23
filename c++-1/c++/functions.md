# FUNCTIONS

## Summary

* modifiers

## Declarations

* `prefix-mod return-type func-name(args) suffix-mod;`

### prefix mod

* `static`
* `virtual`
* `override` 
* `constexpr` 
* `[[noreturn]]` 
* `inline` Inlinig a function means copying and pasting the content of the function directly into the execution path, elimitating the need for the five steps below.

on most platforms, a function call compiles into a series of instructions, such as the following:

1. Place args into registers and on the call stack
2. Push a return address onto the call stack
3. Jump to the called function
4. After the function completes, jump to the return address
5. Clean up the call stack

### suffix mod

* `noexcept` 
* `const` 
* `final` indicates that a method cannot be overridden by a child class
  * can apply to an entire class, disallowing the class from becoming a parent entirely
* `override` 
* `volatile` indicates that a method can be invoked on volatile obj

{% hint style="warning" %}
??? U cannot invoke non-volatile method on volatile obj. 
{% endhint %}

## Auto return types

* `auto` 
* \(very pedantic\) it is possible to extend the auto return type deduction syntax to provide the return type as a suffix with the arrow operator `->` 
* `decltype ( expression )` 

```cpp
auto my-func(args) -> type-expression {
    // return an obj with type matching
    //the type-expression above
}

// example
template <typename X, typename Y>
auto add(X x, Y y) -> decltype(x + y) {
    return x + y;
}
```

```cpp
// decltype example
struct A { double x; };
const A* a;

decltype(a->x) y;     // type of y is double 
```

## Overload Resolution

## Variadic Function











---END---

