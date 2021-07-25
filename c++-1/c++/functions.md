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
* _generic lambda_, are lambda expression templates
* lambda explicit return type, use `->` syntax
  * `[](int x, double y) -> { return x + y; }`
  * can use `decltype`
  * `[](auto x, double y) -> decltype(x+y) { reutrn x + y; }`
* _lambda captures_, porównaj z Countif \(powyżej\) it is analogous to function type constructor
  * by default, lambda capture by value
  * capture is a list with comma
  * jeśli chcesz przechowywać wartości pomiędzy kolejnymi call lambda use reference pass
* _lambda default capture_, reference `[&]` and by value `[=]` will auto-match variables
  * you're not allowed to midify variables captures by value unless you add the `mutable` keyword
* _initializer expressions in capture list_,  
  * przydatne jelsi chcesz przeniesc obiekt \(move\)
* can capture `this`
* all labda expressions are `constexpr` as long as the labmda can be invoked at compile time.

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
increment(10, 5);  // can override default args
```

```cpp
// jak uniknąć redundancji w dwóch linijkach poniżej? gdzie różnica jest tylko
//w typie
transform([](int x){reutrn 10 * x + 5}, base_int, a, 1);
transform([](double x){return 10 * x + 5}, base_float, b, 1);
// use generic lambda ( template )

template <typename Fn, typename T>
void transform(Fn fn, const T* in, T* out, size_t len) {
    for(size_t i{}; i<len; i++) {
        out[i] = fn(in[i]);
    }
}
```

```cpp
// lambda capture example
// ciekawy przykład składni kodu, zobacz na ile linijek zociaga sie 
//definicja lambdy
int main() {
    char to_count{ 's' };
    auto s_counter = [to_count](const char* str) {
        size_t index{}, result{};
        while (str[index]) {
            if (str[index] == to_count) result++;
            index++;
        }
        return result;
    };

auto sally = s_counter("Sally sells seashells by the seashore");
```

```cpp
// lambda capture with reference
int main() {
    char to_count{ 's' };
    size_t tally{};
    auto s_counter = [to_count, &tally](const char* str) {
        size_t index{}, result{};
        while (str[index]) {
            if (str[index] == to_count) result++;
            index++;
        }
        tally += result;
        return result;
    };
    
    auto sally = s_counter("Sally sells seashells by the seashore.");
    // sally = 7; tally = 7
    auto sailor = s_counter("Sailor went to sea to see what he could see.");
    // sailor = 3; tally = 10
```

## std::function c++11

* `std::funtion<return-type(args)>`

{% hint style="danger" %}
nie do końca rozumiem

SOLUTION: ułatwienie żeby nie męczyć sie w coś takiego:  `x(void(*foo)())` 
{% endhint %}

## The main function

* can access command line parameters within `main` by adding arguments to your `main` declarateion.
* `char* argv[]` array of pointers

```cpp
// three main overloads
int main();
int main(int argc, char* argv[]);
int main(int argc, char* argv[], impl-parametes);
```

```cpp
// exploring program parameters
int main(int argc, char** argv) {
    cout << "Arguments: " << argc << endl;
    for (size_t i{}; i < argc, i++) {
    cout << i << "; " << argv[i] << endl;
    }
}
```

### example

* zwróć uwage na sposób wyliczania indeksu do incrementacji \(row:18\)

```cpp
// start with helper functions
char pos_A{ 65 }, pos_Z{ 90 }, pos_a{97}, pos_z{ 122 };
bool within_AZ(char x) {return pos_A <= x && pos_Z >= x; }
bool within_az(char x) {return pos_a <= x && pos_z >= x; }

// build the simple structure
struct AlphaHistogram {
    void ingest(const char* x);
    void print() const;
private:
    size_t counts[26]{};
);

// implementation of ingest
void AlphaHistogram::ingest(const char* x) {
    size_t index{};
    while(const auto c = x[index]) {
        if (within_AZ(c)) counts[c - pos_A]++;
        else if (within_az(c) counts[c - pos_a]++;
        index++;
    }
}

//implement print()
void AlphaHistogram::print() const {
    for(auto index{ pos_A }; index <= pos_Z; index++) {
        printf("%c: ", index);
        auto n_asterisks = counts[index - pos_A];
        while (n_asterisks--) printf("*");
        printf("\n");
    }
}

int main(int argc, char** argv) {
    AlphaHistogram hist;
    for(size_t i{ 1 }; i<argc; i++) {
        hist.ingest(argv[i]);
    }
    hist.print();
}
```













---END---

