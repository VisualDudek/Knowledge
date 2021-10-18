# COMPILE-TIME POLYMORPHISM

## summary

* **templates** - just place holders for types

### Q

* what are _void pointers_?
* ^--- type of pointer that can be pointed at objects of any data type! BUT bc the void pointer does not know what type of object it is pointing to, direct indirection through it is not possible. Rather, the void pointer must **first be explicitly cast to another pointer type before** indericting through the new pointer.
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
tc_name<t_param1, t_param2, ...> my_concrete_class ( ... );
```

```cpp
template <typename T>
// same as
template <class T>
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

### specialization templates

* best example from hackerrank 

## Named Conversion Functions

> `named-conversion<desired-type>( object-to-cast )`

### const_cast

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

### static_cast

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

### reinterpret_cast

### narrow_cast

## Example: mean, A Template Function

```cpp
// consider the func that computes the mean of a double array
double mean(const double* values, size_t length) {
    double result{};
    for(size_t i{}; i<length; i++) {
        result += values[i];
    }
    return result / length;
}
```

**Plot twist**: suppose you want to support `mean` calculations for other numeric types, such as `float` or `long`. First thought "That's what function overloads are for!"

So it is simple copy pase with different types BUT this approach doesn't scale as you add more types.

> What you need to solve this copy-and-paste problem is _generic programming_, a prgramming style where you program with yet-to-be-specified types. Tou achive gereric programming using the support C++ has for templates. Templates allow the compiler to instantiate a custom class or function based on the types in use.

```cpp
// template in practice
template<typename T>
T mean(const T* values, size_t length) {
    T result{};
    for(size_t i{}; i<length; i++) {
        result += values[i];
    }
    return result / length;
}

int main() {
    const double nums_d[] { 1.0, 2.0, 3.0 };
    const auto result1 = mean<double>(nums_d, 3);
    // can drop explicit template parameter
    const auto result1 = mean(nums_d, 3);
    
    const float nums_f[] { 1.0f, 2.0f, 3.0f };
    const auto result2 = mean<float>(nums_f, 3);
}
```

{% hint style="danger" %}
dlaczego jest potrzebne castowanie ???

no i się okazało że mozna opuścić

BUT sometimes, templare arguments cannot be deduced. For example, if a templare function's return type is a templare argument, you must specify template arguments explicitly.
{% endhint %}

## Example: SimpleUniquePointer, A Templare Class

A _unique pointer_ is an RAII wrapper around a free-store-allocated object. As its name suggests, the unique pointer has a single owner at a time, so when a unique pointer's lifetime ends, the pionted-to object gets destructed.

```cpp
template <typename T>
struct SimpleUniquePointer {
    SimpleUniquePointer() = default;
    SimpleUniquePointer(T* pointer)
        : pointer{ pointer } {
    }
    ~SimpleUniquePointer() {
        if(pointer) delete pointer;
    }
    // bc we want to allow a single owner of the pointed-to object
    //we delete the copy and copy-assignment operators
    SumpleUniquePointer(const SimpleUniquePointer&) = delete;
    SimpleUniquePointer& opertor=(const SimpleUniquePointer&) = delete;
    // but we anable move operator
    SimpleUniquePointer(SimpleUniquePointer&& other) noexcept
        : pointer{ other.pointer } {
        other.pointer = nullptr;
    }
    // ???
    SimpleUniquePointer& operator=(SimpleUniquePointer&& other) noexcept {
        if(pointer) delete pointer;
        pointer = other.pointer;
        other.pointer = nullptr;
        return *this;
    }
    // implement get method
    T* get() {
        return pointer;
    }
private:
    T* pointer;
};

// uasge example

struct Tracer {
    Tracer(const char* name) : name{ name } {
        printf("%s constructed.\n", name);
    }
    ~Tracer() {
        printf("%s destructed.\n", name);
    }
private:
    const char* const name;
};

void consumer(SimpleUniquePointer<Tracer> consumer_ptr) {
    printf("(cons) consumer_ptr: 0x%p\n", consumer_ptr.get());
}

int main() {
    auto ptr_a = SimpleUniquePointer(new Tracer{ "ptr_a" });
    printf("(main) ptr_a: 0x%p\n", ptr_a.get());
    consumer(std::move(ptr_a));
    printf("(main) ptr_a: 0x%p\n", ptr_a.get());
}
```

{% hint style="danger" %}
co to za konstrukcja? z podwójnym const

`const char* const name;`
{% endhint %}

{% hint style="danger" %}
aghghghghgh nie rozumiem
{% endhint %}

{% hint style="info" %}
The SimpleUniquePointer is a pedagogical implementation of the stdlib `std::unique_ptr` which is a member of the family of RAII templates called smart pointers.
{% endhint %}

## Type Checking in Templates

```cpp
// this template has a silent requiremnt: it must support multiplication
template<typename T>
T square(T value) {
    return value * value;
}

// how can it goes wrong?
int main() {
    char c{ 'a' };
    auto result = square(&c); // Bang!
}
// solution -> concepts 
```

> C++ templare programming shares similarities with _duck-typed languages_. Duck-typed languages (like Python) defer type checking until runtime.

## Concepts C++20

* `concept`

{% hint style="danger" %}
skip
{% endhint %}

## static_assert (the preconcepts stopgap) C++17

* alternative to concepts

```cpp
static_assert(boolean-expression, optional-message);

//alternative to concepts
template <typename T>
T mean(T* values, size_t length) {
    static_assert(std::is_default_constructible<T>(),
        "Type must be default constructible.");
    static_assert(std::is_copy_constructible<T>(),
        "Type must be copy constructible.");
    static_assert(std::is_arithmetic<T>(),
        "Type must support addition and division.");
    static_assert(std::is_constructible<T, size_t>(),
        "Type must be constructible from size_t.");
    --snip--
}
```

## Non-Type Template Parameters

* function `get` can be improved by enable by non-type template parameters genericizing the values out of `get`
* _first_: relax the requ. that `arr` refer to an `int` array by making `get` template function
* _second_: can relax the req that `arr` refer to an array of length `10` by introducing a non-type template parameter `Length` **into template, not func. parameter**. **U are injecting a value into the generic code at complie time.**
* third**: **U can perform compile time bounds checking by taking `size_t index` as another non-type templare parameter. This allow you to replace the `std::out_of_range` with a `static_assert`.

{% hint style="info" %}
when U declare int a\[] = {1, 2, 3} it is the same as (or will be rewritten as) int\[3] = {1, 2, 3} since the templated function is receiving argument in form of T a\[N], then N will have value of 3.
{% endhint %}

```cpp
// base scenario
#include <stdexcept>

int& get(int (&arr)[10], size_t index) {
    if (index >= 10) throw std::out_of_range{ "Out of bounds" };
    return arr[index];
}

// first improvment: accept an array of a generic type
template <typename T>
T& get(T (&arr)[10], size_t index) {
    if (index >= 10) throw std::out_of_range{ "Out of bounds" };
    return arr[index];
}

// second: tytaj wazne bedzie jak tego uzywać
template <typename T, size_t Length>
T& get (T (&arr)[Length], size_t index) {
    if (index >= Length) throw std::out_of_range{ "Out of bounds" };
    return arr[index];
}

// third:
template <size_t Index, typename T, size_t Length>
T& get(T (&arr)[Length]) {
    static_assert(Index < Length, "Out-of-bound access");
    return arr[Index];
}

// usage
int main() {
    int fib[] {1, 1, 2, 3};
    cout << get<0>(fib) << get<1>(fib);
}
```

## Variadic Templates

> _Variadic templates_ take a variable number of arguments.

{% hint style="danger" %}
skip
{% endhint %}

## Advanced Template Topics

{% hint style="danger" %}
skip
{% endhint %}

## Polymorphism at Runtime vs. Compile Time





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
>
>
