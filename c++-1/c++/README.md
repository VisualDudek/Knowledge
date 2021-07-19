# C++

## hello, world

```cpp
#include <cstdio>

int main() {
    printf("Hello, world!");
    return 0;
}
```

* _.cpp_ extension
* single entry point called the `main` function, can not be _void_
* exit code `return 0;`
* `printf` is defined in the `cstdio` library

## Compiler

1. preprocessor
2. compiler
3. linker

```bash
g++ -std=c++2a -I fmt fig03_06.cpp format.cc -o fig03_06
```

## if

* for one-liners do not need square brackets `{` but you need semicolon `;`

```cpp
if (boolean-expression) {
    statement1;
    statement2;
    --snip--
}

if (boolean-expression) stmt-1;
else if (boolean-2) stmt-2;
else stmt-3;
```

## Switch Statements

```cpp
switch (condition) {
    case (case_a): {
        // Handle case a here
        --snip--
    } break;
    case (case_b): {
        // Handle case b here
        --snip--
    } break;
    default: {
        // Handle the default case here
        --snip--
    }
}
```

## AND, OR

The logical operators AND `&&` and OR `||` are binary.

## Functions

```cpp
return_type function_name(par-type1 par_name1, par-type2 par_name2) {
    --snip--
    return return_value;
}
```

> Defining multiple functions with the same name but different parameters is called _function overloading_.

## printf

{% hint style="info" %}
another option to put smth on standard output is `cout` which is part of `iostream` library

C++20 text-format &lt;format&gt; if compiler do not support use format.h lib
{% endhint %}

### format specifiers

`%x` hexadecimal and `%o` octal representation.

As a general rule, use `%g` to print floating-point types.

There is no format specifiers for `bool`, but you can use the `int` format spec. `%d` to yield a `1` for ture and a `0` for false.

### format lib

```cpp
#include "fmt/format.h"
using namespace fmt;
...
    cout << format("{}'s garade id {}", sudent, grade) << endl;
```

## Comments

```cpp
// This comment is on its own line

/*
 * This is a commment
 * with multiple
 * lines, Don't forget to close
 */
```

## ternary operator :?

* very good usecase for return stmt

`variable = (condition) ? expressionTrue : expressionFalse;`

```cpp
// ternary operator :? example
int foo = 5;
string result = (foo < 10) ? "lower" : "upper";
```

## libraries

### bignumber.h

### cstdio

* printf

### cstdlib

* `abs()`

### cstring

* strlen
* strncpy

### format C++20

* {fmt} library provides a full implementation of the new text-formatting features

### iostream

* cout

### limits

* `std::numeric_limits<long long>::min();`

### string

### stdexcept

* `std::runtime_error`

## literals

A literal is a hardcoded value in a program:

`binary` uses the prefix `0b` 

`octal` -&gt; `0`

`decimal` -&gt; this is the default

`hexadecimal` uses the prefix `0x`

{% hint style="info" %}
Integer literal can contain any number of single quotes ' for readability. These are completely ignored by the compiler. For example, `1000000` and `1'000'000` are both integer equal to one million.
{% endhint %}

A _character literal_ is a single, constant character. Single quotation marks \(' '\) surround all characters. If the character is any type but `char`, you must also provide a prefix: `L` for `wchar_t`, `u` for `char16_t`, and `U` for `char32_t`. For example, 'J' declares a `char` literal and `L'J'` declares a `wchar_t`.

Unicode character literals: `\u` followed by a 4-digit Unicode code or the prefix `\U` followed by an 8-digit Unicode code.

```cpp
'\u0041' // A
'\U0001F37A' // beer mug character
```

## Escape Sequences

## Types

System programmers sometimes work directly with _raw memory_, which is a collection of bits without a type. Employ the `std::byte` type, available in the `<cstddef>` header, it permits bitwise logical operations and little else.

`size_t` type, also available in the `<cstddef>` header, to encode size of objects. \[Format Specifiers\] the usual format specifiers for a `size_t` are `%zu` for a decimal representation of `%zx` for a hexadecimal.

```cpp
#include <cstddef>
#include <cstdio>

int main() {
    size_t size_c = sizeof(char);
    printf("char: %zu\n", size_c);
    size_t size_i = sizeof(int);
    printf("int: %zu\n", size_i);
}
```

**void** The `void` type has an empty set of values.

### Strings

_Strings_ are contiguous blocks of characters. A _C-style string_ or _null-terminated string_ has a zero-byte appended to its end \(a null\) to indicate the ind of the string. Because array elements are contiguous, you can store strings in array of character types.

String literal, declare by enclosing text in quotation marks \(" "\).

```cpp
char english[] = "A book holds a house of gold.";
```

```cpp
int main() {
    char alphabet[27];
    for (int i = 0; i <26; i++) {
        albhabet[i] = i + 97;
    }
    alphabet[26] = 0; // make null-terminated string
    printf("%s", alphabet);
}
```

### Enumerations

```cpp
enum class Race {
    Dinan,
    Teklan,
    Ivyn,
    Aidan
}

// Initialize
Race langbard_race = Race::Aidan;
```

{% hint style="info" %}
`enum class` is one of two kinds of enumerations: it's called a scoped enum. For compatibility with C, C++ also aupports an unscoped enum, which is declared with `enum` rather than `enum class`. 
{% endhint %}

### Plain-Old-Data Classes POD

* keyword `struct`
* semicolon and the end of last square bracket
* access members using the dot operator

```cpp
struct Book {
    char name[256];
    int year;
    int pages;
    bool hardcover;
}; // semicolon and end

int main() {
    Book book;
    book.pages = 271;
}
```

{% hint style="warning" %}
As a general rule, you should order members from largest to smallest within POD definitions.
{% endhint %}

POD classes contina inly data maembers, and sometimes that's all you want from a class. However, designing a program using only PODs can create a lot of complexity. You can fight such complexity with **encapsulation**, a design pattern that binds data with the functions that manipulate it. Placing related functions and data together helps to simplity code in at least two ways. First, you can put related code in one place, which helps you to reason about your program. Second, you can hide some of a class's code and data from the rest of your program using a practice called _information hiding_.

> **encapsulation** a design pattern that binds data with the functions that manipulate it. In C++, you achive encapsulation by adding methods and access controls to class definitions.

### Classes

`struct` vs `class` keyword, aside from default access control, classes declared with the `struct` and `class` are the same.

* `class` declares members `private` by default

### methods

```cpp
struct ClockOfTheLongNow {
    void add_year() {
        year++;
    }
    int year;
};
```

### access controls

```cpp
struct ClockOfTheLongNow {
    void add_year() {
        year++;
    }
    bool set_year(int new_year) {    // setter
        if (new_year < 2019) return false;
        year = new_year;
        return true;
    }
    int get_yer() {    // getter
        return year;
    }
private:
    int year;
};
// such way U cannot modify `yaer` directly
```

{% hint style="info" %}
Having encapsulated `year` you must now use methods to interact with `ClockOfTheLongNow`
{% endhint %}

```cpp
// will show U the problem when clock is declared and year is uninitailized
int main() {
    ClockOfTheLongNow clock;
    --snip--
}
```

{% hint style="info" %}
You want to guarantee that `year` is never less than 2019 under _any circumstatnces_. Such requirement is called a **class invariant**: a feature of a class that is always true \(that is, it never varies\). ---&gt; U can use **constructor**.
{% endhint %}

### constructor\(s\)

* name matches the class's name
* can take any number of args &lt; - - - what if you wnat to init `Clock` with a custom year?
* can have several constructors depending on type of constructor arg.

```cpp
struct Clock {
    Clock() {
        year = 2019;
    }
private:
    int year;
};

// constructor with arg
    ...
    Clock(int year_in) {
        --snip--
};

int main() {
    Clock clock{ 2020 }; // Class initialization. constructor with arg
```

```cpp
struct T {
    T() {
        // no arg
    }
    T(char arg) {
        printf("char: %c\n", arg);
    }
    T(int arg) {
        printf("int: %d\n", arg);
    }
};

// Init
int main() {
    T t1; // no arg
    T t2{ 'c' };         // char: c
    T t3{ 65537 };       // int: 65537
    T t4{ 6.02e23f };    // float: 60200017271895229464576.000000
    T t5('g');           // char: g
    T t6 = { '1' };      // char: 1
    T t7{};              // no arg
    T t8();              // suprise 
}
```

{% hint style="danger" %}
`T t8();` This is function declaration. It is function `t8` that a yet-to-be-defined function takes no args and returns an object of type `T`.
{% endhint %}

```cpp
struct Tracer {
    Tracer(const char* name) : name{ name } { // take arg and saves it into
    --snip--                                  // member name.
    }
private:
    const char* const name;
};
```

### destructor

```cpp
struct Earth {
    ~Earth() {
        // Earth's destructor
    }
};
```

### Initialization

```cpp
// Initialized to 0
int a = 0;
int b{};
int c = {};
int d; // maybe
```

```cpp
// Initialized to 42
int e = 42;
int f{ 42 };
int g = { 42 };
int h(42);
```

* only form left to righ, cannot skip args
* cannot use parentheses to init PODs

```cpp
// POD initialization

struct Foo {
    int a;
    char b[256];
    bool c;
};

int main() {
    Foo foo{};    // All fields zeroed
    Foo foo = {}; // All fields zeroed
    
    Foo foo{ 42, "Hello" };        // Fields a & b set; c = 0
    Foo foo{ 42, "Hello", true };  // All fields set
}
```

{% hint style="danger" %}
`Foo foo = 0;  // this will fial`
{% endhint %}

```cpp
int array[]{ 1, 2, 3 };
int array[5]{};             // 0, 0, 0, 0, 0   
int array[5]{ 1, 2, 3 };    // 1, 2, 3, 0, 0
int array[5];               // uninitialized values
```

#### Narrowing Conversions

```cpp
float a{ 1 };
float b{ 2 };
int narrowed_result(a/b);    // Potentially ansty narrowing conversion
int result{ a/b };           // Compiler generates warning
```

{% hint style="info" %}
_use braced initializers everywhere_
{% endhint %}

### inheritance

```cpp
struct Superclass {
    int x;
};

struct Subclass : Superclass {    // inheritance syntax
    int y;
    int foo() {
        return x + y;    // subclass inherits members from Superclass
    }
};
```

### Unions

The union is a cousin of the POD that puts all of its members in the same place.

* keyword `union`
* store only one value 

{% hint style="info" %}
You can think of unions as different views or interpretations of a block of memory.
{% endhint %}

```cpp
union Variant {
    char string[10];
    int integer;
    double floating_point;
};

int main() {
    Variant v;
    v.integer = 42;
    printf("The ultimate answer: %d\n", v.integer);
    v.floating_point = 2.7182818284;
    printf("Euler's number e: %f\n", v.floating_point);
    // Disaster strikes:
    printf("A dumpser fire: %d\n", v.integer);
}
```

## Reference Types

* keywords `this` , `const` and `auto`
* _**dereferencing**_: given a pointer, you can obtain the object residing at the corresponding address
* `void` pointer
* `std::byte` pointer
* encode empty pointers with `nullptr`
* You can obtain the address of a variable by prepending the _address-of operator_ \(`&`\)
* Pointers encode object location.

```cpp
int* my_ptr;    // declare a pointer using an asterisk (*)
printf("The value of my_ptr is %p.", my_ptr);  // format specifier
```

{% hint style="info" %}
Address Space Layout Randomization
{% endhint %}

### Dereferencing Pointers

* dereference operatror \(`*`\) is a unary operator that accesses the object to which a pointer refers.

```cpp
int main() {
    int foo{};
    int* foo_address = &foo;
    printf("Value at foo_address: %d\n", *foo_address);
    printf("foo Address: %p\n", foo_address);
    *foo_address = 1234;    // same as foo = 1234
    printf("Value at foo_address: %d\n", *foo_address);
    printf("foo Address: %p\n", foo_address);
}
```

### The Member-of-Pointer Operator

* or called _arrow operator_ \(`->`\)
* perform two simultaneous operations: \(1\) dereferences a pointer \(2\) accesses a member of the pointed-to object.

```cpp
struct Clock {
    --snip--
};

int main() {
    Clock clock;
    Clock* clock_ptr = &clock;
    clock_ptr->set_year(2020);
    // same as (*clock_ptr).set_year(2020)
    
    printf("Address of clock: %p\n", clock_ptr);
    printf("Value of clock's year: %d", clock_ptr->get_year());
}
```

```cpp
struct Bar {
    char name[265];
};

void print_name(Bar* bar_ptr) {
    printf("Bar name: %s\n", bar_ptr->name);
}

int main() {
    Bar foo[] = { "Mag", "Nul", "Kelly" };
    print_name(foo); // HINT: remember that array is already address!
}
```

### Pointer Arithmetic

* can access array elements using pointer with the bracket operator `[ ]`

```cpp
College* second_college_ptr = &work[1];
// or using pointer arithmetic
College* second_college_ptr = work + 2; // remember that work is an array
```

{% hint style="info" %}
`&work[1]` this seem tricky, but it seems that order of "decode" this is first form right than from left: \(1\) obtain object 2-th at array work \(2\) get address of this object
{% endhint %}

```cpp
int main() {
    char lower[] = "abc?e";
    char upper[] = "ABC?E";
    char* upper_ptr = upper;
    
    lower[3] = 'd';
    upper_ptr[3] = 'D';
```

```cpp
// array pointer arithmetics, equivalent to above Listing
int main() {
    char lower[] = "abc?e";
    char upper[] = "ABC?E";
    char* upper_ptr = &upper[0];
    
    // next magic happens
    *(lower + 3) = 'd';    
    *(upper_ptr + 3) = 'D'; // same as upper_ptr[3] but no dereference needed
}
```

### void Pointers and std::byte Pointers

### nullptr and Boolean Expressions

Pointers have an implicity conversion to `bool`

### References

* cannot be reseated or reassigend
* cannot be assigned to null \(easily\)
* no deref operator needed

```cpp
struct Clock {
    --snip--
};

void add_year(Clock& clock) {
    clock.set_year(clock.get_year() + 1); // No deref operator needed
}

int main() {
    Clock clock;
    printf("The year is %d.\n", clock.get_year()); // directly object pass
    add_year(clock); // Clock is implicity passed by reference!
    printf("The year is %d.\n", clock.get_year());
}
```

```cpp
// references cannot be reseated
int main() {
    char original{ 'A' };
    char& original_ref = original;
    printf("Original: %c\n", original);
    printf("Reference: %c\n", original_ref);
    
    char new_char{ 'B' };
    original_ref = new_char;
    printf("Original: %c\n", original);        // B
    printf("New_value: %c\n", new_value);      // B
    printf("Reference: %c\n", original_ref);   // B 
```

### Forward-Linked Lists

Each element holds a pointer to the next element. The last element in the linked list holds a `nullptr`

```cpp
// singly linked list element
struct Element {
    Element* next{};
    
    void insert_after(Element* new_element) {
        new_element->next = next;
        next = new_element;
    }
    
    char data[2];
    int number;
};

int main() {
    Element a, b, c;
    a.number = 1;
    b.number = 2;
    c.number = 3;
    
    a.insert_after(&b);
    b.insert_after(&c);
    
    for (Element* cursor = &a; cursor; cursor = cursor->next) {
        printf("element %c%c-%d\n",
            cursor->data[0],
            cursor->data[1],
            cursor->number);
    }
}
```

{% hint style="info" %}
for loop check if pointer `cursor` is not `nullptr`
{% endhint %}

### this Pointer

* use when a method parameter name collides with a member variable

```cpp
struct Element {
    Element* next{};
    void insert_after(Element* new_element) {
        new_element->next = this->next;
        this->next = new_element;
    }
    
    char data[2];
    int number;
};
```

```cpp
// resolve ambiguity between members and arguments
struct Clock {
    bool set_year(int year) {
     if (year < 2019) return false;
     this->year = year;
     return true;
    }
--snip---
private:
 int year;
};
```

### const

* "I promise not to modify"
* read-only

### auto

* can use `auto*` and `auto&`

```cpp
int a = 42;

auto the_a { 42 };    // int
auto foot { 12L };    // long
auto rootbeer { 5.0F };    // float
auto bar { "string" };     // char[7]
```

## Arrays

* indexing is zero based
* array is just memory addres so U do not need reference if you want pointer to array
* passing array in to function arg in fact you are passing pointer

```cpp
int my_array[100];
int array[] = { 1, 2, 3, 4 };

// Accessing Array Elements
arr[2] = 100;

// number of elements in an Array
size_t n = sizeof(array)/sizeof(int)
```

{% hint style="info" %}
you can safely obtain the number of elements using the `std::size` function available in the `<iterator>` header.
{% endhint %}

```cpp
int arr[]{3, 6, 9};
int* arr_ptr = arr;     // Points to 3
// do not need reference operator bc array is just address
```

```cpp
// pass array as two args

struct College {
    char name[256];
};

void print_names(College* colleges, size_t n_colleges) {
    for (size_t i = 0; i < n_colleges; i++) {
        --snip--
    }
}

int main() {
    College work[] = { "Andy", "Fox", "Napoleon" };
    print_names( work, sizeof(work) / sizeof(College) );
}
```

## Vector

* iterators and reverse\_iterator

```cpp
#include <vector>

vector <int> v;
vector <int> vec {1, 2, 3, 4, 5};
```

## For loops

* init-stmt vars are limited to for-scope

```cpp
for( init-stmt; conditional; iteration-stmt) {
    --snip--
}
```

```cpp
int main() {
    int max = 0;
    int array[] = { 1, 5, 2, 4, 0};
    for(size_t i=0; i < 5; i++) {
        if (array[i] > max) maximum = array[i];
    }
}
```

{% hint style="info" %}
`size_t` is guaranteed to be able to index any value within it, `int` is not.
{% endhint %}

### Range-Based for Loop

```cpp
for(element-type element-name : array-name) {
    --snip--
}
// element-type must match the types within the array you're iteraing over
```

```cpp
int main() {
    int max = 0;
    int array[] = { 1, 5, 2, 4, 0};
    for(int value : array) {
        if (value > max) maximum = value;
    }
}
```

## while

```cpp
int main() {
    int counter{1};
    
    while (counter <= 10) {
        cout << counter << " ";
        ++counter;
    }
    count << endl;
}
```

## debugging, gdb

? need to compile with debug support, using `-g` flag.

`$ g++ main.cpp -g`

## other

### declaration vs initialization

Declaration, generally, refers to the introduction of a new name in the program. For example, you can _declare_ a new function by describing it's "signature":

```cpp
void foo();
class klass;
struct ztruct;
int x;
```

Initialization refers to the "assignment" of a value, at construction time. 

### unary, binary and ternarry operator

A _unary_ operator takes a single operand: the unary negation operator `!` takes a single operand and returns its opposite.

