# EXPRESSIONS

## Summary

* built-in operators
* prefix and postfix operator the return of expresion is related to pre or post
* `typeid` form `<typeinfo>`
* overloading operator `new`
  * overloading `malloc` ???
* user-defined literals
* type conversion of own user-defined types
* `constexpr`
* `voiatile`

## Operators

### Logical Operators

```cpp
// bitwise operators

// AND 
x & y // 0b1100 & 0b1010 = 0b1000
// OR
x | y // 0b1100 | 0b1010 = 0b1110
// XOR
x ^ y // 0b1100 ^ 0b1010 = 0b0110
// bitwise complement
~x // ~0b1010 = 0b0101
// bitwise left shift
x << y
// bitwise right shift
x >> y
```

### operator promotion

e.g. for unary arithmetic operators if the operand is of type `bool, char or short int` the result of the expression is an `int` 

### member access operators

```cpp
// member access operators
[ ] subscript
* indirection
& address-of
. member-of-object
-> member-of-pointer
.* pointer-to-member-of-object
->* pointer-to-member-of-pointer
```

### ternaty conditional aka Elvis operator

The _ternary conditional operator_ `x ? y : z` is a lump of **syntactic sugar**.

### comma

It allows several expressions separated by commas to be evaluated within a larger expression.

```cpp
int confusing(int &x) {
    return x = 9, x++, x / 2;
}

int main() {
    int x{};
    auto y = confusing(x);
    // after invoking confusing, x equals 10 and y equals 5
}
```

### operator overloading

* keyword `operator`
* "design pattern" for immutable

{% hint style="info" %}
`type_traits`which allow you to determine features of your types at compile time. A related family of type support is available in the `<limits>` header, which allows you to query various properties of arithmetic types.
{% endhint %}

```cpp
struct CheckedInteger {
    CheckedInteger(unsigned int value) : value{ value } {}
    
    CheckedInteger operator+(insigned int other) const {
        CheckedInteger result{ value + other };
        if (result.value < value) throw std::runtime_error{ "Overflow!" };
        return result;
    }
    
    const unsigned int value; // due to const CheckedInteger is immutable
                            //after construction
};

int main() {
    CheckedInteger a { 100 };
    auto b = a + 200;
    cout << b; // a + 200 = 300
    
    try {
        // a + max does not fit inside an unsigned int
        auto c = a + std::numeric_limits<unsigned int>::max();
    } catch(const std::overflow_error& e) {
        cout << e.what();    // Exception: Overflow!
    }
}
```

### overloading operator new

* _free store_, also known as the _heap_, is an implementation-defined storage location. **Kernel usually manages the free store malloc on Linux**.

{% hint style="info" %}
In game dev or high-frequency trading, free store allocation simply involve too much latency, because you've delegated its management to the operating system.

You could try to avoid using the free store entirely, but this is severely limiting. One major limitation this would introduce is to preclude the use of stdlib containers, which after reading Part II you'll agree is a major loss. Rather than settling for these severe restrictions, you can overload the free store operations and take control over allocations.

**You do this by overloading opertor `new`.**
{% endhint %}

### the &lt;new&gt; Header

* Notice that the return type of operator `new` is `viod*` 
* if ther is plenty of free memory, but it's scattered across allocated memory large requests for memory can fail
* &lt;new&gt; header contains the following four operators

```cpp
void* operator new(size_t);
void operator delete(void*);
void* operator new[](size_t);
void operator delete[](void*);
```

### Buckets

### Control of the Free Store

```cpp
// implementing very simple Bucket and Heap classes that will take control
//over free store allocation
#include <cstddef>
#include <new>

struct Bucket {
    const static size_t data_size{ 4096 };
    std::byte data[data_size];
};

struct Heap {
    void* allocate(size_t bytes) {
        if (bytes > Bucket::data_size) throw std::badalloc{};
        for (size_t i{}; i < n_heap_buckets; i++) {
            if (!bucket_used[i]) {
                bucket_used[i] = true;
                return buckets[i].data;
            }
        }
        throw std::bad_alloc{};
    }
    
    void free(void* p) {
        for (size_t i{}; i < n_heap_buckets; i++) {
            if (buckets[i].data == p) {
                bucket_used[i] = false;
                return;
            }
        }
    }
    
static const size_t n_heap_buckets{ 10 };
Bucket buckets[n_heap_buckets]{};
bool bucket_used[n_heap_buckets]{};
};               
```

### Using Our Heap

```cpp
Heap heap;

void* operator new(size_t n_bytes) {  // new operator overload
    return heap.allocate(n_bytes);
}
void operator delete(void* p) {
    return heap.free(p);
}

int main() {
    cout << "Buckets: " << heap.buckets << endl;
    
    auto breakfast = new unsigned int { 0xCOFFEE };
    auto dinner = new unsigned int { 0xDEADBEEF };
    
    cout << "pointer of breakfast: " << breakfast << endl;
    cout << "dereference pointer of breakfast: " << * breakfast << endl;
    delete breakfast;
    delete dinner;
}
```

### Operator precedence and associativity

```cpp
*a++ + b * c // postfixx addition has higher precedence than the dereference *
```

### Evaluation order









