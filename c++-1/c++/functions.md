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

takes a variable number of args, e.g. `printf` is a variadic function, it accepts any number of parameters.

* declare variadic by placing `...` as the final parameter in the func arg list.
* need to be accessed by utility functions int the `<cstdarg>` header.
  * func: `va_list` `va_start` `va_end` `va_arg` `va_copy` 
* not type-safe \(notice that the second arg of `va_arg` is a type\)
* the number of elements in the variadic args must be tracked separately

```cpp
int sum(size_t n, ...) {
    va_list args;
    va_start(args, n);
    int result{};
    while (n--) {
        auto next_element = va_arg(args, int);
        result += next_element;
    }
    va_end(Args);
    return rsult;
}

int main() {
    cout << sum(6, 2, 4, 6, 8, 10, 12);
```

## Variadic Templates

* enable: create func templates that accept variadic, same-type args.
* _template parameter pack_
  * the recursion needs a stopping criteria, so you add a func template specialization without the parameter

```cpp
// example
template <typename T, typename... Args>
void my_func(T x, Args...args) {
    // use x, then recurse:
    my_func(args...);
}

// func template specialization
template <typename T>
void my_func(T x) {
    // Use x, but DON't recurse
}

// sum example
template <typename T>
constexpr T sum(T x) {
    return x;
}

template <typename T, typename.. Args>
constexpr T sum(T x, Args... args) {
    return x + sum(args...);
}

int main() {
    cout << sum(2, 4, 6, 8, 10, 12); // now you do not need size of input
}
```

{% hint style="danger" %}
BC all of this generic programming can be computed at compile time, you mark these func `constexpr` this compile-time computatin is a _**major**_ advantage over variadic function above.
{% endhint %}

### fold expressions

* computes the result of using a binary operator over all the arguments of a parameter pack.
*  `(... binary-operator parameter-pack)` 

```cpp
// example using fold expression indtead of recursion
template <typename... T>
constexpr auto sum(T... args) {
    return (... + args);
}

int main() {
    cout << sum(2, 4, 6, 8, 10, 12);
}
```

## Function Pointers

_Functional programming_ is a programming paradigm that emphasizes function evaluation and immutable data.

* One way you can achive this is to pass a function pointer. Functions occupy memory, just like objects. You can refer to this memory address via usual pointer mechanism.
* cannot modity the pointed-to function \(conceptually similar to const obj.\)
* `return-type (*pointer-name)(args);` 
* use address-operator `&` to take the address of a function
* **mind changer**: just init func pointer that accept types U want
* alias to function pointers:  `using alias-name = return-type(*)(args)`

{% hint style="warning" %}
Gdzie jest edge ???
{% endhint %}

```cpp
float add(float a, int b) {
    return a + b;
}

float subtract(float a, int b) {
    return a - b;
}

int main() {
    const float first{ 100 };
    const int second{ 20 };
    
    float(*operation)(float, int) {}; // init function pointer
                                    //accepting a float and int a
                                    
    operation = &add; // assign address of add func to operation pointer
    cout << operation(first, second);
}
```

```cpp
// alias to function pointer
using operation_func = float(*)(float, int);
```

## Function-Call Operator

* You can make user-define types callable or invocable by overloading the function-call operator `operator()()` 
* mind change: umozliwia call bezpośrednio na ~~clasie~~ instatncji classy, patrz przykład

```cpp
// definition
struct type-name {
    return type operator()(args) {
        // body of function-call operator
    }
}

// count if example

struct CountIf {
    CountIf(char x) : x{ x } {} // zwykły konstruktor
    size_t operator()(const char* str) const { 
                            // bc calling the function doesn't modify the state
                            //of CountIf object, we can mark it const
        size_t index{}, result{};
        while (str[index]) {
            if (str[index] == x) result++;
            index++;
        }
        return result;
    }
private:
    const char x;
};

int main() {
    CountIf s_sounter{ 's' };
    auto sally = s_counter("Sally sells seashells by the seashore.");
    cout << sally << endl;
    auto buffalo = CountIf{ 'f' }("Buffalo buffalo Buffalo");
    cout << buffalo << endl;
}
```

## Lambda Expressions

* `[captures] (parameters) modifiers -> return-type { body }`
* only captures and body are req
* can take default args
  * can override def value
* _generic lambda_, TODO

```cpp
auto square = [](int x) {return x*x;};
square(3);

// example

// template który będzie przyjmował na pierwszym arg funkcje lambda
template (typename Fn>
void transform(Fn fn, const int in, int* out, size_t length) {
    for(size_t i{}; i < length; i++) {
        out[i] = fn(int[i]);
    }
}

int main() {
    const size_t len{ 3 };
    int base[]{ 1, 2, 3}, a[len], b[len], c[len];
    transform([](int x) { return 1; }, base, a, len);
    transform([](int x) { return x; }, base, b, len);
    transform([](int x) { return 10*x+5 }, base, c, len);
}
```

```cpp
// lambda with default arg
auto increment = [](auto x, int y = 1) {return x + y }
increment(10);
increment(10, 5);
```





















---END---

