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

// define standard
g++ -std=c++20 file.cpp -o file
```

## Preprocessor

Preprocessor directives are lines included in the code preceded by a hash sign (#). These lines are directives for the preprocessor. The preprocessor examines the code before actual compilation of code begins and resolves all these directives before any code is actually generated by regular statements.

```cpp
#define INF 10000000
#define add(a, b) a + b  // without comma or semicolon

#if !defined FUNCTION || !defined INF
#error Missing preprocessor definitions
#endif
```

## if

* for one-liners do not need square brackets `{` but you need semicolon `;`
* can have init-stmt

```cpp
if (boolean-expression) {
    statement1;
    statement2;
    --snip--
}

if (boolean-expression) stmt-1;
else if (boolean-2) stmt-2;
else stmt-3;

// with initailizer
if (int value{9}; value == 7) {
--snip--
} else {
--snip--
}
```

## \[\[likely]] and \[\[unlikely]] C++20

* can apply to if, case ...

```cpp
if (condition) [[likely]] {
    --snip--
} else {
    --snip--
}
```

## Switch Statements

* init stmt from C++17

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

// C++17
switch (init-stmt; condition) {}
```

### \[\[fallthrough]]

```cpp
switch (grade / 10) {
    case 9: [[fallthrough]]; // without fallthrough compiler will warning
                            //need explicite stated that intention
    case 10:
        ++aCount;
        break;
    ...
```

## AND, OR

The logical operators AND `&&` and OR `||` are binary.

## Functions

* order in main program matters, if U want use function that is defined after `main()` block U have to declare it before `main()` block.
* ??? function prototype is the same as function signature?
  * return type is not part of function signature
* **argument coercion** - can call a function with an int arg, even though the function prototype spec duble parameter
* `inline` advice to the compiler, removes the overhead of the function call
  * only way to check the difference is to read asembly code 
* can have default values
* **signature **consists from name function and its parameters not return type
* _**name mangling**_ - compiler will combine name function with its parameters, each compiler has its own syntax
* C++20 functions marked `[[nodiscard]]` cause compilers to issue warnings if you ignore the return value
* _**hiher-order function**_ - is a func that does at least one of the following:
  * takes one or more functions as args
  * returns a function as its results
* **lambda** - (patrz rozdział FUNCTIONS)  

{% hint style="info" %}
sprawdzaj przykłady na wiki, np. bdb opis dla hihger-order function 
{% endhint %}

```cpp
// declaration function prototype
return_type function_name(par-type1 par_name1, par-type2 par_name2);

// definiton
return_type function_name(par-type1 par_name1, par-type2 par_name2) {
    --snip--
    return return_value;
}
```

```cpp
// inline functions
inline double cube(double side) {
    return side * side * side;
}
...
cout << cude(a); // compiler will put body of cube function into main code;
```

```cpp
// default values
//small flexibility e.g. if first arg has def value all ubsequent has to 
// defaults can be defined in prototype
int box(int l = 1, int w = 1, int h = 1);

--snip--

int box(int l, int w, int h) {}
```

```cpp
// check compiler name mangling syntax:
g++ -S helloworld.c // will produce helloworld.s assembler
```

```cpp
// [[nodiscard]] example
[[nodiscard]] int cube(int x) {
    return x * x * x;
}

int main() {
    cube(10); // generates a compiler warning
}
```

```cpp
// selection stmt with binding declaration
struct TextFile {
    bool success;
    const char* data;
    size_t n_bytes;
};

TextFile read_text_file(const char* path) {--snip--}

int main() {
    if(const auto [success, txt, len] = read_text_file; success) {
    // success == true
    } else {
    }
}
```

> Defining multiple functions with the same name but different parameters is called _function overloading_.

## printf

{% hint style="info" %}
another option to put smth on standard output is `cout` which is part of `iostream` library

C++20 text-format \<format> if compiler do not support use format.h lib
{% endhint %}

### format specifiers

`%x` hexadecimal and `%o` octal representation.

As a general rule, use `%g` to print floating-point types.

There is no format specifiers for `bool`, but you can use the `int` format spec. `%d` to yield a `1` for ture and a `0` for false.

### fmt lib

* do not return fmt-string as reference BC it is only temporary obj in memory and will we "clensed" before U will use it

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

### std

* `swap`
  * `void swap( T& a, T& b);`

### bignumber.h

### cmath

* all func are in `std` namespace,
* `pow(a, b)`

### cstdio

* printf

### cstdlib

* C at the bgn of `cstdlib` indicate that it comes from C lang.
* `abs()`
* `rand()`
  * by default it use the same seed every time
* `srand()` 

### cstring

* strlen
* strncpy

### format C++20

* {fmt} library provides a full implementation of the new text-formatting features
* Optional header-only configuration enabled with the FMT_HEADER_ONLY macro
* For anybody else who encounters this error: You need to insert #define FMT_HEADER_ONLY above #include "fmt/format.h" so your code will compile.

### iostream

* cout, cerr
* sticky stream manipulator
  * `cout << fixed << setprecision(2);`
* `cerr << "An exception occured"`

### iomanip

* `setw(int n);`
  * `cout << "Year" << setw(20) << "Amount on deposit" << endl;` 
  * not sticky
  * right align by default
  * `cout << setw(4) << left << year << endl;`
  * better use C++20 format
* `boolalpha`

### limits

* `std::numeric_limits<long long>::min();`

### numbers C++20

* namespce `std::numbers`

### numeric

* `accumulate`

### random

* `class uniform_int_distribution`

### std::string

* `getline()`
* `string.size()`

### sstream

* `stringstream`

### stdexcept

* `std::runtime_error`

## literals

A literal is a hardcoded value in a program:

`binary` uses the prefix `0b` 

`octal` -> `0`

`decimal` -> this is the default

`hexadecimal` uses the prefix `0x`

{% hint style="info" %}
Integer literal can contain any number of single quotes ' for readability. These are completely ignored by the compiler. For example, `1000000` and `1'000'000` are both integer equal to one million.
{% endhint %}

A _character literal_ is a single, constant character. Single quotation marks (' ') surround all characters. If the character is any type but `char`, you must also provide a prefix: `L` for `wchar_t`, `u` for `char16_t`, and `U` for `char32_t`. For example, 'J' declares a `char` literal and `L'J'` declares a `wchar_t`.

Unicode character literals: `\u` followed by a 4-digit Unicode code or the prefix `\U` followed by an 8-digit Unicode code.

```cpp
'\u0041' // A
'\U0001F37A' // beer mug character
```

## Escape Sequences

## Scope

* can access hidden global var by _unarry scope _resolution operator

```cpp
// using global scope

int x{1};

int main() {
    int x {5}; // local main scope
}

void foo() {
    cout << x << endl; // using global scope
}
```

```cpp
// unary scope resolution operator

const int number{7};

int main() {
    const double number{10.5};
    
    cout << "Global: " << ::number << endl;
}
```

{% hint style="danger" %}
todo static local
{% endhint %}

## Magic number

Unique values with unexplained meaning or multipe occurrences which could be relaced with names constans

```cpp
// example
std::array<int, 5> arr{};  // here 5 is magic number

// can be refactor
constexpr size_t arraySize{5};
std::array<int, arraySize> arr{};
```

## Types

System programmers sometimes work directly with _raw memory_, which is a collection of bits without a type. Employ the `std::byte` type, available in the `<cstddef>` header, it permits bitwise logical operations and little else.

`size_t` type, also available in the `<cstddef>` header, to encode size of objects. \[Format Specifiers] the usual format specifiers for a `size_t` are `%zu` for a decimal representation of `%zx` for a hexadecimal.

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

_C-Strings_ are contiguous blocks of characters. A _C-style string_ or _null-terminated string_ has a zero-byte appended to its end (a null) to indicate the ind of the string. Because array elements are contiguous, you can store strings in array of character types.

String literal, declare by enclosing text in quotation marks (" ").

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

* can overload increment operator on enum type

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

```cpp
// can define operator for enum class

enum class Color {red, blue , green };

Color& operator++(Color& t);
{
    switch (t) {
    case Color::red: return t=Color::blue;
    case Color::blue: return t=Color::green;
    case Color::green: return t=Color::red;
    }
}

Color next = Color::red;
++next; 
```

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

```cpp
Account account{}; // an Account obj
Account& ref{account}; // ref refers to an Account obj
Account* ptr{&account}; // ptr points to an Account obj
```

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
You want to guarantee that `year` is never less than 2019 under _any circumstatnces_. Such requirement is called a **class invariant**: a feature of a class that is always true (that is, it never varies). ---> U can use **constructor**.
{% endhint %}

### constructor(s)

* name matches the class's name
* can take any number of args < - - - what if you wnat to init `Clock` with a custom year?
* can have several constructors depending on type of constructor arg.
* constructor that can be invoked without an argument is called _default constructor_.
* many useful operations do not require direct access to the representation of class, so they can be defined separtely from the class definition.
* the `std::initializer_list` used to define the initializer-list constructor is a standard-library type known to the compiler: when we use `{ }`  -list, such as`{1,2,3,4}` the compiler will create an object of type `initializer_list` to give to the program.
* _**concrete types**_ are types where their representation is part of theri definition.
* _**abstract types**_ completely insulates a user from implementtaion details. SEE ## BELOW

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

```cpp
// allow init with { }
// below Vector is not the same Vector as this from <vector> header
Vector::Vector(sdt::initializer_list<double> lst) // init with a list
    :elem{new double[lst.size()]}, sz{static_cast<int>(lst.size())}
{
    copy(lst.begin(),lst.end(), elem); //copy from list into elem
}
```

### abstract class

* `virtual` means "may be redefined later in a class derived from this one"
* `=0` syntax says the function is pure virtual; that is, some class derived from it MUST define the function.
* Since we **don't know anything about the representation of an abstract type **(not even its size), we **must allocate objects on the free store and access them through references or pointers**.
* `override` keyword is optional but beeing explicit allows the compiler to catch mistakes
* ??? note that the member

```cpp
class Container {
public:
    virtual double& operator[](int) = 0; 
    virtual int size() const = 0;
    virtual ~Container() {}
};

Container c; // BANG! there can be no objects of an abstract class SEE 3th point
            //above
Container* p = new Vector_container(10) // OK

// see how use func useses the Container interface in complete ignoracce of 
// implementation details
void use(Container& c)
{
    const int sz = c.size();
    
    for (int i=0; i!=sz; ++i)
        cout << c[i] << endl;
}

// derived class from Container
class Vector_container : public Container { // Vector_container implements Container
public:
    Vector_container(int s) : v(x) {} // Vector of s elements
    ~Vector_container() {}
    
    double& operator[](int i) override { return v[i]; }
    int size() const override { return v.size(); }
private:
    Vector v;
};
```

### destructor

* `delete` cleans up object, `delete[]` deletes an array

```cpp
struct Earth {
    ~Earth() {
        // Earth's destructor
    }
    
    ~Earth() { delete[] elem; }
    
private:
    double* elem;  // elem points to an array
};
```

### Initialization

* the `std::initializer_list` used to define the initializer-list constructor is a standard-library type known to the compiler: when we use `{ } ` -list, such as `{1,2,3,4}` the compiler will create an object of type `initializer_list` to give to the program.

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
* **cannot use parentheses** to init PODs
  * with `()` it will be function declaration, not class initialization

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
* You can obtain the address of a variable by prepending the _address-of operator_ (`&`)
* Pointers encode object location
* indirection `*` pointer operator
* legacy code from C
  * built-in pointer-based arrays
  * pointer-based strings
* Null pointers before C++11: 0, NULL (this is preprocessor value of 0)
* name convention, add Ptr suffix
* "smart pointers" can auto manage memory and avoid of "memory leaks"

```cpp
int* my_ptr;    // declare a pointer using an asterisk (*)
printf("The value of my_ptr is %p.", my_ptr);  // format specifier
```

```cpp
int* countPtr;   // uninitialized "dangling pointer"
// C++11 nullptr
int* countPtr{nullptr};    // pointer to nothing
```

```cpp
void cubeByReferene(int* nPtr) {
    *nPtr = *nPtr * *nPtr * *nPtr;
}
// takich cyrków nie trzeb robić gdy używasz referencji &
```

{% hint style="info" %}
Address Space Layout Randomization
{% endhint %}

### using const with pointers

* four ways to pass a pointer to a function:
  * a nonconstst pointer to nonconstant data, highest access `int* ptr` 
  * a nonconstant pointer to constant data,
  * a constant pointer to nonconstant data,
  * a constant pinter to constant data
* constatn pointer oznacza tylko tyle ze nie mozna zmienić na xo wskazuje ale mozna (jeśli nonconstatnt data) zmienic dane na które wskazuje
* **mind changer**: `const int*` vs `int* const`  

```cpp
// mind changer
// constant pointer to nonconstant data a i tak nie moge modyfikować????
int y{0};
const int* yPtr{&y};  // pointer thinks that this location is constant
//^^^^^^^- const is on th left of int -> constant data of type int
*yPtr = 100;  // error: cannot modify a const obj.

//
int x, y;
int* const ptr{&x}; //ptr always points to the same location
//  ^^^^^^^^- constant pointer of non-constant data of type int
*ptr = 7; // OK
ptr = &y; // error

// can only read 
int x{5}, y;
const int* const ptr{&x};
*ptr = 7; // error: *ptr is const int
ptr = &y; // error: ptr is const ptr 
```

### Legacy Arrays

* C++20, if built-in arrays are required
  * (1) use `to_array` function to convert to `std::array`
  * (2) process as C++20 `span` 
* `[ ]` does not provide bounds checking
* array name `n` is equivalent to `&n[0]` 
* name of array decay to the pointer
* do not know their own size, U loose information about size of array
* cannot be compared using the relational and equality operators
  * `arr1 == arr2` will be always false
* cannot be assigned to one another
* when pass to func as arg wil decay into pointer
* `sort(begin(arr), end(arr));` can be applied to built-in arrays
  * works only in the scope that originally defines the array
  * out of scope compilator lose the knowledge of size of array
* C++20 `to_array()` 
* `sizeof()` use on array to get size of whole array and on array pointer to get size of single element
  * need only `( )` when dealing with type e.g. `sizeof(char) sizeof(int)` 
  * do not need wiht vars e.g. `sizeof arr` chyna jednak powoduje ze jest maloczytelne

```cpp
int sumElements(const int values[], size_t numberOfElements)
// compilator will dacay into pointer
int sumElements(const int* values, size_t numberOfElements)

// same thing again
void foo(int ptr[]);  //ptr will decay into int* ptr, need to use *ptr inside
//will decay to:
void foo(int* ptr);
```

### Legacy pointer-based string

* null character `\0` 
* `sizeof` for string literal is the length of the string, **including** null char
* string literals are immutable

```cpp
char color[]{"blue"};
const char* colorPtr{"blue"};
char color[]{'b', 'l', 'u', 'e', '\0'};
```

### Dereferencing Pointers

* dereference operatror (`*`) is a unary operator that accesses the object to which a pointer refers.

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

* or called _arrow operator_ (`->`)
* perform two simultaneous operations: (1) dereferences a pointer (2) accesses a member of the pointed-to object.

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
* can use `++` and `--` 
* C++ Core Guidelines - A pointer should refer only to a single object (not an array)
* subtracting one pointer from another of the same type wil give a distance
* pointer can be assigned to another pointer if both pointers are of the same type
  * exception: pointer to void `void*` 
  * viod pointer need to be manually cast to the proper ponter type

```cpp
College* second_college_ptr = &work[1];
// or using pointer arithmetic
College* second_college_ptr = work + 2; // remember that work is an array
```

```cpp
int v[5];
int* vPtr{v};

/*
location:
3000    3004    3008    3012    3016
  | v[0]  | v[1]  | v[2]  | v[3] | v[4]
  |           
vPtr
*/
vPtr += 2;
/*
3000    3004    3008    3012    3016
  | v[0]  | v[1]  | v[2]  | v[3] | v[4]
                  |       
                 vPtr
*/
```

{% hint style="info" %}
`&work[1]` this seem tricky, but it seems that order of "decode" this is first form right than from left: (1) obtain object 2-th at array work (2) get address of this object
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

* cannot be reseated or reassigend, **references are always const**
* cannot be assigned to null (easily)
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

## std::array

* defined in header `<array>`
* support iterator
* `array.at(idx)` vs `array[idx]` the first one make sure U have provided valid index, the later does not
* support `size()`
* multidimensional arrays

```cpp
template <class T, std::size_t N>
struct array;

std::array<int, 3> a = {1, 2, 3};
// or
std::array<int, 3> a{32, 27, 64};

const auto array1 = array{"abc"}; // creates a one-element array<const char*>
        //not char[4] 
array1.size(); // 1

const auto array2 = to_array("abc"}; // will create const char[4]
array2.size(); // 4

// can be init at for-range-based loop
for (auto* i : array <int, 6> {1, 2, 3, 4, 5, 6}) {}
```

```cpp
// multidimensional array [C++20: Multidimensional array]
size_t rows{2|;
size_t columns{3};
array<array<int, colums>, rows> array1{1, 2, 3, 4, 5, 6};
                                    // ^^^^^^^-- first row
// array1 is array of where each row is composed from array<int, columns>

// access:
array1[0][3]  // 0 row; 3 column idx

array1.at(0).(3) 
```

## Vector

* dynamic size
* iterators and reverse_iterator
* C++ `erase` method
* co to jest `vector<int>& items` co tu robi `&` ???SOLUTION reference  dotyczy całego vektora a nie elementów w vektorze.
* copy constructor
  * przekazanie do konstruktora obiektu o tym samym typie
* can be accessed by `[ ]` or `at( )` the later do bound-check
* `v.front()` and `v.back()`

```cpp
#include <vector>

vector <int> v;
vector <int> vec {1, 2, 3, 4, 5};
vector <int> v(7); // 7 elements filled with zeros

vec.size();
vec.capacity(); // returns the number of elements that 
//can be held in currently allocated storage

// copy constructor
vector<int> v{vec}; 

// use overloaded assignment (=) operator
vec = v; // assign integers from v to vec
```

## range \<ranges> C++20

* support_ lazy evaluation_, eval only when needed via `views` namespace
* SEE: C++20 video L06 Intro to Functional-Style Programming

### views namespace

* `views::iota(1, 11);` only declare not allocate, it is lazy eval
* can be chained by pipe operator `|` 
* najciekawsze jest to że na przykładach z {C+20 video} iterator sie nie wyczeruje tzn. podeklaracji i wykorzystaniu jest wykorzystywany jeszcze kilka razy bez ponownej deklaracji

## For loops

* init-stmt vars are limited to for-scope
* if U want to use counter outside loop, declare it outside loop-scope
* nie wiedziałem że w `iteration-stmt` mozna kilka stmt po przecinku

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

```cpp
int counter;
for (counter = 0; ... ; ...) {} // notice that inside for-loop there
                                //is no declaration or initialization
                                //only assignment

// you can use counter outside for-loop
```

```cpp
// ciekawostka
for(int i{0}; i < n; i++, x++) {} // dwa stmt w ostatnim args po przecinku
```

{% hint style="info" %}
`size_t` is guaranteed to be able to index any value within it, `int` is not.
{% endhint %}

### Range-Based for Loop

* C++20 added init-stmt 

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

```cpp
// init iterowanego obiektu w stmt for-loop
for (auto* i : array <int, 6> {1, 2, 3, 4, 5, 6}) {}
// ciekawe że to działa bo przy legacy array nie działa
```

```cpp
// multiply the elements of items by 2
for (int& itemRef : items) {
    itemRef *= 2;
}
```

```cpp
// C++20, init-stmt runningTotal example
for (int runningTotal{0}; const int item : items) {
    runningTotal += item;
}
```

## while

* only thing with `do...while` loop is that it will be executed at least once -> condition is tested after first do block
* `continue` example

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

```cpp
// do ... while
int counter{1};
do {
    cout << counter << " ";
    ++counter;
} while (counter <= 10); //end do...while
```

```cpp
// loop until user enters the end-of-file indicator -> Ctrl-d 
while (cin >> grade) {
    --snip--
```

```cpp
// continue keyword example
//how to skip 5 in interation
for (int i{1}; i <=10; ++i) {
    if (i == 5) {
        continue;
    }
}
```

## debugging, gdb

? need to compile with debug support, using `-g` flag.

`$ g++ main.cpp -g`

## other

### declaration vs initialization vs definition

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

### comma operator

* comma operator guarantees left-to-right evaluation `a, b, c`
* NOTE: in func evaluation order is not specified
