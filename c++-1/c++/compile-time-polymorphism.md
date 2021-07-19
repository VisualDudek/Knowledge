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

### reinterpret\_cast

### narrow\_cast

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

