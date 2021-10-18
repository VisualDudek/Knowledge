# Easy

## String, not C-String

* `string::npos` czyli nie `-1` 
* there is a better way: regex

```cpp
//replace all spaces with period
size_t pos = s.find(" ");

while (pos != string::npos) {
    s.replace(pos, 1, ".");
    pos = s.find(" ", pos + 1);
}
```

* using `stoi()` to convert string to int

```cpp
string s("100hello");
size_t index;

int convertInt{stoi(s, &index, 2)}
// we will convert from base 2
// fisrt char not converted -> index will be stored at &index address
```

## String stream

```cpp
const string inputString{"name test 123 4.7 A"};
istringstream input{inputString};

input >> s1 >> s2 >> i >> d >> c;
```

## Files

### create file

* ciekawy while z "chain" stream cin
  * need to press EOF to exit cin (Ctrl+D)

```cpp
if (ofstream output{"clients.txt", ios::out}; output) {
    cout << "Enter the account, name, and balance. \n"
        << "Enter end-of-file to end input. \n";
        
    int account;
    string name;
    double balance;
    
    // read account, name na balance from cin, then place in file
    while (cin >> acount >> name >> balance) {
        output << fmt::format("{} {} {}\n", account, name, balance);
        cout << "? ";
    }
}
else {
    cerr << "File could not be opened\n";
    exit(EXIT_FAILURE);
}
```

### reading CSV file

* using _rapidcsv_ lib

```cpp
#include <algorithm>
#include <cmath>
#include <iostream>
#include <numeric>
#include <ranges>  // C++20 only in GNU 10
#include <string>
#include <vector>
#include "fmt/format.h" // In C++20 this will be #include <format>
#include "rapidcsv.h"
using namespace std;

int main() {
    // load Titanic dataset; treat missing age values as NaN
    rapidcsv::Document titanic{"titanic.csv",
        rapidcsv::LabelParams{}, rapidcsv::SeparatorParams{},
        rapidcsv::ConverterParams{true}};
        
```

## RegEx

```cpp
#include <iostream>
#include <regex>
using namespace std;

int main() {
    regex r1("marcin");
    cout << regex_match("marcin", r1) << endl;
    cout << regex_match("abc", r1) << endl;

    // fully match five digits
    regex r2(R"(\d{5})");

    // match a word that starts with a capital letter
    regex r3("[A-Z][a-z]*");

    // match any character that's not a lowercase letter
    regex r4("[^a-z]");

    // match metacharacters as literals in a custom charcter class
    regex r5("[*+$]");

    // matching a capital letter followed by at least one lowercase letter
    regex r6("[A-Z][a-z]+");

    // matching zero or one occurence of a subexpression
    regex r7("labell?ed");

    // matching n (3) or more occurrences of a subexpression
    regex r8(R"(\d{3,})");

    // matching n to m inclusive (3-6), occurrences of a subexpresion
    regex r9(R"(\d{3,6})");

}
```























\---END---
