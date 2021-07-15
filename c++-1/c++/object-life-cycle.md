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

{% hint style="info" %}
_local static var_ begin upon the firdt invocation of the enclosing function and end ehen the program exits.
{% endhint %}

