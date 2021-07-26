# C++ examples

## Functional-Style Programming

### lambda, accumulate

* `accumulate` przyjmuje dwa arg
* `array.begin()` to samo `begin(array)`
* ciekawa referencja w lambda `const auto& x` 

```cpp
#include <array>
#include <iostream>
#include <numeric>
using namespace std;

int multi(int x, int y) {
    return x * y;
}

int main() {
    constexpr size_t arraySize{5};
    array<int, arraySize> integers{1, 2, 3, 4, 5};
    
    cout << "Product of integers: "
        << accumulate(begin(integers), end(integers), 1, multi) << endl;
        
    cout << "Product of integers with a labda: "
        << accumulate(begin(integers), end(integers), 1,
            [](const auto& x, const auto& y){reutrn x * y;}) << endl;
}
```

### lambda, generic

* assign labda to local var

```cpp
//lambda to dispaly results of range operations
//can pass any type that support iterator
auto showValues = [](auto& values) {
    for(auto value : values) {
        cout << value << " ";
    }
}

//use case
showValues(v); // v is Vector
```

## Containers

### vector

* when use `int` and when reference to int `int&` 

```cpp
void outputVector(const vector<int>& items) { //pass-by reference bo po co
                        //marnować przestrzeń na dublowanie elemenu
                        //const bo ta funkcja nie powinna absolutnie nic
                        //zmieniać w przekazanym vektorze
    for (const int item : items) {
        cout << item << " ";
    }
}

void inputVector(vector<int>& items) { //pass-by reference
    for (int& items : items) { //potrzebuje referencji do kazdego obiektu
        cin >> item;
    }
}
```

