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
// Just before function returns, these vars are deallocated.
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

{% hint style="danger" %}
`const char* const name;` co to jest ???
{% endhint %}

## Exceptions

* `throw` and `catch` keywords.
* `#include <stdexcept>`
* `std::runtime_error` constructor accepts a null-terminated `const char*` describing the nature of the error condition.
* use `try-catch` blocks to establish exception handlers for a block of code.
* can chaining together `catch` stmt's.
* can rethrow exception using `throw` inside `catch`
* `noexcept` mark any func that cannot possibly throw an exception -&gt; enable some code optimizations
* exception can affect object lifetime

```cpp
// throws an exception whenever you invoke the forget method with arg == 0xFACE
struct Foo {
    void forget(int x) {
        if (x == 0xFACE) {
            throw std::runtime_error{ "make an exception." };
        }
        print("Forgot 0x%x\n", x);
    }
};      
// try-catch
int main () {
    Foo foo;
    try {
        foo.forget(0xFACE);
    } catch (const std::runtime_error& e) {
        printf("exception caught with message: %s\n", e.what());
    }
}
```

{% hint style="danger" %}
`std:runtime_error& e` co tutaj robi &? czy to jest referencja?
{% endhint %}

{% hint style="info" %}
The rules for exception handling are based on class inheritance. When an exception is thrown, a `catch` block hankles the exception if the theown exception's type matchs the `catch` handler's exception type or if the thrown exception's type _inherits_ from the `catch` handler's exception type
{% endhint %}

{% hint style="info" %}
The runtime seeks the closest exception handler to a thrown exception. If there is a matching exception handler in the current stack frame. it will handle the exception. If no matching handler is found, the runtime will unwind the call stack until it finds a suitable handler. Any objects whose lifetimes end **are destroyed** in the usual way.
{% endhint %}

{% hint style="info" %}
As a general rule, treat destructors as if they were `noexcept` . Do not throw exception inside destrctor.
{% endhint %}

## SimpleString Class Example

* `size_t` is unsigned and cannot be negative, so you do not need to check for this bogus condition.
* new func `strncpy`
* co to jest _class invariant_ ???

> What is assumed to **be true for** a class is called "_class invariant_" or simply "invariant" It is the job of a constructor to establish the invariant for its class \(**so that the member functions can rely on it**\) and for the member functions to make sure that the invatiant holds when they exit.

```text
// Invariant Example: it is not c++ lang
context LargeCompany
inv: numberOfEmployees > 50
```

{% hint style="info" %}
initializacja var members w kostruktorze, dziwna składnia po operatorze colon \(`:`\)

`Constructor( args ) : init member1 using arg1, init member2 using arg2 {`
{% endhint %}

```cpp
struct SimpleString {
    SimpleString(size_t max_size) // constructor
        : max_size{ max_size },   // saves length into the max_size memeber
          length{} {              // init length to zero
        if (max_size == 0) {
          throw std::runtime_error{"Max size must be at least 1."};
          // ^--- exception in action
        }
        buffer = new char[max_size];
        buffer[0] = 0;
    }
    
    ~SimpleString() {           // deconstructor
      delete[] buffer;
    }
    
    void print(const char* tag) const {  // it is const bc doesn't need to mod
                                         //the state odf a SimpleString
      printf("%s: %s" tag, buffer);
    }
    
    bool append_line(const char* x) {
      const auto x_len = strlen(x);
      if (x_len + length + 2 > max_size) return false; // 2 bc of null byte and 
                                                       // newline character
      std::strncpy(buffer + length, x, max_size - length);
      length += x_len;
      buffer[length++] = '\n';
      buffer[length] = 0;
      return true;
    }
--snip--
private:
  size_t max_size;
  char* buffer;
  size_t length; // odpowiada idx null-byte
};
```

{% hint style="warning" %}
ciekawe czy jest róźnica pomiędzy `delete[] buffer;` a `delete buffer;` bez brackets
{% endhint %}

> This pattern is called _resource acquisiion is initialization_ \(RAII\) or _constructor acuires, deconstructor releases_ \(CADRe\).

### Composing a SimpleString

```cpp
struct SimpleStringOwner {
    SimpleStringOwner(const char* x)
        : string{ 10 } {
        if (!string.append_line(x)) {
            throw std::runtime_error{ "Not enoug memory!" };
        }
        string.print("Constructed");
    }
    ~SimpleStringOwner() {
        string.print("About to destroy");
    }
        
private:
 SimpleString string;
};

int main() {
    SimpleStringOwner x{ "x" };
    printf("x is alive\n");
}
```

{% hint style="danger" %}
Gdzie jest problem w SimpleString i jak go rozwiązuje SimpleStringOwner ???
{% endhint %}

### Call Stack Unwinding

{% hint style="warning" %}
TODO: example from boook
{% endhint %}

### Alternatives to Exceptions

1. manually enforce class invariants by exposing some method that communicates whether the class invariants could be established
2. return multiple values using _structured binding declaration._ **Factory methods,** their purpose is to initialize objects.

```cpp
// factory method + return multiple values
struct Result {
    HumptyDumpty hd;
    bool success;
};

Reslut make_humpty() {
    HumptyDumpty hd{};    // init all to zero
    bool is_valid;
    // Check that hd is valid and set is_valid appropriately
    return { hd, is_valid };
}

bool send_kings_horses_and_men() {
    auto [hd, success] = make_humpty();  // zwróć uwagę na auto deklaracje typów
    if (!success) return false;
    // Class invariant established
    --snip--
    return true;
}
```

{% hint style="info" %}
deklaracja typów w przypadku gdy funkcja zwraca więcej niż jeden arg, patrz powyżej na --15--
{% endhint %}

## Copy Semantics

_Copy sematics_ is "the meaning of copy". In practice, programmers use the term to mean the rules of makeing copies of objects: after `x` is _copied into_ `y`, they'r _equvalent_ and _independent_. That is, `x == y` is ture after a copy \(equivalence\), and a modification to `x` doesn't cause a modification of `y` \(independence\).

```cpp
// example pass by value
int add_one(int x) {    // pass by value
    x++;
    return x;
}

int main() {
    auto original = 1;
    auto reult = add_one(original);  // pass by value
    printf("Original: %d; Result: %d", original, result);
}
```

```cpp
// same wiht POD types, original is unaffected
struct Point {
    int x, y;
};

Point make_transpose(Point p) {  // pass by value
    int tmp = p.x;
    p.x = p.y;
    p.y = tmp;
    return p; // return new Point
}
```

{% hint style="warning" %}
co to jest **memberwise copy** ?
{% endhint %}

{% hint style="danger" %}
Why it is not so easy with _Fully featured classes_ ?
{% endhint %}

### Copy Constructor

* we want _deep copy_
* copy constructror is invoked when passing `SimpleString` into a function by value

```cpp
struct SimpleString {
    --snip--
    SimpleString(const SimpleString& other); // no reson to modifiy src
};

int main() {
    SimpleString a;
    SimpleString a_copy{ a };
}
```

```cpp
// copy constructor implementation
SimpleString(const SimpleString& other)
    : max_size( other.max_size },
      buffer{ new char[other.max_size] },
      length { other.length } {
    std::strncpy(buffer, other.buffer, max_size);
}
```

```cpp
// if class have copy constructor, it will be invoked while passing into 
//function by value
--snip--
void foo(SimpleString x) {
    x.append_line("This change is lost.");
}

int main() {
    SimpleString a { 20 };
    foo(a); // Invoke copy constructor
    a.print("Still empty");
}
```

{% hint style="danger" %}
You shouldn't pass by value to avoid modification. Use a `const` reference.

e.g. class that manage GB data and each time U copy the object you'll need to allocate and copy a gigabyte of data.
{% endhint %}

### Copy Assignment

* major differenve between copy assignment and copy constructor is the in copy assignment `dst` might already have a value. You must clean up `dst` resources before copy `src` e.g. `dst = src`

> copy assignment operator usrs the `operator=` syntax

```cpp
struct SimpleString {
    --snip--
    SimpleString& operator=(const SimpleString& other) {
        if (this == &other) return *this; // no need of slef-copy
        const auto new_buffer = new char[other.max_size];
        delete[] buffer;
        buffer = new_buffer;
        length = other.length;
        max_size = other.max_size;
        std::strncpy(buffer, other.buffer, max_size);
        return *this;
    }
}
```



