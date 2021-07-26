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

