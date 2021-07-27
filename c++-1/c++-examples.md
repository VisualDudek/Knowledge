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

* when use `int` and when reference to int `int&` wewnątrz "vector scope"

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

#### Operator Overloading and Multidimensional vector

* 
```cpp
// based on hackerrank: Operator Overloading
struct Matrix {
    vector<vector<int>> a;
    
    Matrix operator+(Matrix& other) {
        Matrix res;
        size_t n{a.size()};
        size_t m{a[0].size()};
        //cout <<n<<" "<<m<< endl;
        
        for(int i=0; i<n; i++) {
            vector<int> b;
            for(int j=0; j<m; j++) {
                int res;
                res = a[i][j] + other.a[i][j];
                b.push_back(res);
            }
            res.a.push_back(b);
        }
        return res;
    }
};

// better in syntax but solution is wrong bc. mutate this
class Matrix {
public:
vector<vector<int> > a;

Matrix & operator + (const Matrix &y) {

    for (int m=0; m<y.a.size(); ++m) {
        for (int n=0; n<y.a[0].size(); ++n) {
            this->a[m][n] = this->a[m][n] + y.a[m][n];
        }
    }

    return *this;
};
```

### span C++20

* from `<span>` header
* pass by value BC they are just a pointer
* can pass built-in array and compiler will implicity create span obj.
  * compiler can also create implicity span obj from std::array and vector
* can work with span obj.
* can create subviews
* can use `[ ]` on span obj. but not all compilers do not make boud check!

```cpp
void displayArray(const int items[], size_t size) {//items[] will decay into pointer
    for(size_t i{0}; i < size; ++i) {
        cout << items[i] << " ";
    }
}

void displaySpan(span<const int> items) {
    for (const auto& item : items) {
        cout << item << " ";
    }
}

void times2(span<int> items) {
    for (int& item : items) {
        item *= 2;
    }
}

int arr[5]{1,2,3,4,5};
displayArray(arr, 5); // need to pass size of arr
dispalySpan(arr); // compiler will implicity creates span obj

span<int> mySpan{arr};
mySpan.front();
mySpan.back();
mySpan.first(3); // first three elements
mySpan.last(3);
mySpan.subspan(1, 3);

times2(mySpan.subspan(1, 3));
accumulate(begin(mySpan), end(mySpan), 0);
```

## Polymorphism

### virtual function

* zobacz jak wykorzystany jest w klasie HotelApartament construktor z base class
  * umozliwia to tzymanie private vars in base class
  * taki setter dla private vars.
* kluczowe w tym przykładzie jest przesłanianie metod
* zobacz z jakim prefixem oznaczone sa prywatne zmienne 

Root cause: Keyword _virtual_: It determines which method is used if the method is invoked by a reference or a pointer instead of by an object. If you **don’t use** the keyword virtual, the program chooses a method based on the **reference type or pointer type.** If you do **use** the keyword virtual, the program chooses a method based on the **type of object the reference or pointer refers to.**

```cpp
// based on hackerrank: Hotel Prices
class HotelRoom {
public:
    HotelRoom(int bedrooms, int bathrooms)
    : bedrooms_(bedrooms), bathrooms_(bathrooms) {}
    
    int get_price() {
        return 50*bedrooms_ + 100*bathrooms_;
    }
private:
    int bedrooms_;
    int  bathrooms_;
};

class HotelApartment : public HotelRoom {
public:
    HotelApartment(int bedrooms, int bathrooms)
    : HotelRoom(bedrooms, bathrooms) {}
    
    int get_price() {
        return HotelRoom::get_price() + 100;
    }
}

// MIND CHANGER
HotelApartment ha{1, 2};
ha.get_price(); // 250 call HotelApartment::get_price ponieważ ha jest derived 
    // from base class HotelRoom i przesłania metode get_price()
    // ivoked by object
    
// ALE dla rooms gdzie mozemy trzymać HotelRoom i klasy które dziedzicza po niej
//spraw wygląda juz troche inaczej
vector<HotelRoom*> rooms;
rooms.push_back(new HotelApartment(1,1));

for (auto room : rooms) {  // iteruje po pointerach bo: vector<HotelRoom*> rooms;
    room->get_price();  // invoke by pointer (or reference)
}
// teraz wykonywana jest metoda HotelRoom::get_price() 
//a nie HotelApartment::get_price()

// jeszcze inaczj ten sam przykład
HotelRoom* hrPtr;
hrPtr = &ha; // MIND CHANGER: do pointera typu base class moge przypisać
            // derived class
hrPtr->get_price(); // 150 a nie 250 poniewaz zostala wywolana 
                //metoda HotelRoom::get_price() a nie HotelApartment
// THAT IS THE RESON YOU NEED virtual int get_price() in base class
```

















---END--

