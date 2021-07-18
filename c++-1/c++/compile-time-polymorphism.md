# COMPILE-TIME POLYMORPHISM

## summary

### Q

* what are _void pointers_?
* ^--- type of pointer that can be pointed at objects of any data type! BUT bc the void pointer does not know what type of object it is pointing to, direct indirection through it is not possible. Rather, the void pointer must first be explicitly cast to another pointer type before indericting through the new pointer.
* ^--- ciekawe gdzie jest edge dla void pointers?

## Templates

> The big idea is that, rather than copying and pasting common code all over ther place, you write a single template; the compiler generates new template instatnces when it encounters a new combination of types in the tmeplate parameters.

## Declaring Templates

{% hint style="info" %}
The coexistence of the `typename` and `class` keyword is unfortunate and confusing. They mean the same thing.
{% endhint %}

```cpp
// template class definition
template <typename X, typename Y, typename Z>
struct MyTemplateClass {
    X foo(Y&);
private:
    Z* member;
};

// instanting templates
tc_name<t_param1, t_param2, ...> my_concrete_class { ... };
```

```cpp
// template function definition
template<typename X, typename Y, typename Z>
X my_template_function(Y& arg1m const Z* arg2) {
    --snip--
}

// instanting
auto result = tf_name<t_param1, t_param2, ...>(f_param1, f_param2, ...);
```

## Named Conversion Functions

> `named-conversion<desired-type>( object-to-cast )`

### const\_cast

The `const_cast` function dropts away the `const` modifier, allowing the modification of `const` values.

```cpp
// how to drop const modifier
void carbon_thaw(const int& encased_solo) {
    //encased_solo++;     // compiler error due to modifying const
    auto& hibernation_sick_solo = const_cast<int&>(encased_solo);
    hibernation_sick_solo++;
}
```

{% hint style="info" %}
teraz doszło do mnie że const jest jedynie ograniczeniem compilatora ponieważ dalej to jest ten sam adres w pamięci, nic w warstwie kodu się nie zmienia
{% endhint %}

{% hint style="info" %}
ciekawostka: you can use `const_cast` to add `const` to an object's type.
{% endhint %}

### static\_cast

```cpp
short increment_as_short(void* target) {
    auto as_short = static_cast<short*>(target);
    *as_short = *as_short + 1;
    return *as_short;
}

int main() {
    short beast {665};
    auto mark_of_the_beast = increment_as_short(&beast);
}

// chyba znalazlem lepszy pszykład:
int value{ 5 };
void* voidPtr{ &value };

cout << *voidPtr << '\n'; // illegal: Indirection through a void pointer

int* intPtr{ static_cast<int*>(voidPtr) };

cout << *intPtr << '\n'; // OK
```

> end
>
> end
>
> e
>
> e
>
> e
>
> e
>
> e
>
> e
>
> e
>
> e
>
> e
>
> e
>
> ee
>
> e

