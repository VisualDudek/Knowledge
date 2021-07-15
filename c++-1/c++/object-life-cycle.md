# OBJECT LIFE CYCLE

## An Object's Storage Duration

_automatic memory manager_ or a _garbage collector_. At runtime, programs create objects. Periodically, the garbage collector deterines which objects are no longer required by the program and safely deallocates them. 

> _automatic object_ is allocated at the beginning of an enclosing code block. The enclosing block is the automatic object's _scope_.

```cpp
void power_up(int nuclear_isotopes) {
    int waste_heat = 0;
    --snip--
}
// each time function power_up is invoked both nuclear_isotopes and waste_heat 
// are allocated each time.
// Just before junction returns, these vars are deallocated.
```

### static

* declare static var at global scope, `static int foo = 200;` 
* can use `static` or `extern` keyword
* `extern` _External linkage_

{% hint style="warning" %}
With `exter` rather than `static` you can access declared var from other trnaslation units.

Co to są **translation units**?
{% endhint %}

* **local static var** are declared at function scope
* to co może dziwić to jednorazowe initializacja np. `static int foo{20};`
* ^--- czytane pochopnie może błednie wskazywać na reinitializacje z wartością 20, co nie jest prawdą.

{% hint style="info" %}
_local static var_ begin upon the first invocation of the enclosing function and end when the program exits.
{% endhint %}

* **static members**, are members of a class that aren't associated with a particular instance of the class.
* need to be initialize at global scope
* must refer ot them by the contating class's name using _scope resolution operator_
* have only a single instance
* can be vars and also methods!

> _**scope resolution operator**_ ::

{% hint style="danger" %}
Exception: to the static member initialization rule: you can declare and define integral types witih aclass definition as long as they're also `const`
{% endhint %}

* `thread-local`

### dynamic

* `new` expression, return a pointer to the newly minted object.
* `int* myint_ptr = new int;`
* ^--- declare or initialize: `int* myint_ptr = new int{ 42 };`
* need to delete dynamic object using keyword `delete`

```cpp
// dynamic arrays
int* my_int_array_ptr = new int[100];
delete[] my_int_array_ptr;
```

## Tracing the Object Life Cycle



