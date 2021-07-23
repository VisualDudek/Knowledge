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
* `final` 
* `override` 
* `volatile` 















---END---

