# String

## Summary

* conversion from numeric values to string
* string manipulation: find, replace, insert
* use fmt/format lib or C+20 format header
* compared lex \(underlying ascii code value\)

## String, not C-String

* member function `assign` , overloaded with bunch of different versions, check cppreference
  * under cppreference check that each overload has number on right and explenation to that number below.
  * same with `append`
    * can append substring
  * and `compare` 
    * can compare two substring
* are mutable, e.g. `s.at(0) = 'x';`
* `substr` retrive substring
* `swap` self explantory 
* string characteristics
  * `s.capacity()` before realocating memory
    * can `s.reserve()` to avoid constatnt realocating memory due to huge impact
  * `s.max_size()`
  * `s.size()`
  * `s.empty()`

### find substring, chars

* `s.find()` vs `s.rfind()` second search from right to left
* `s.find_first_of("abc")`vs `s.find_last_of()` notice it will look for char 'a', 'b' or 'c' 
* `s.find_first_not_of("abc")` 
* return `-1` if not found

### replace

* `s.erase(62);` remove from index 63 throuh end of s
* `s.replace(pos, 1, ".")`
* `s.replace(pos, 2, "xxxxx;;yyy", 5, 2)` replace with two semicolons bc start at 5 idx of second strind and two chars
  * notice: that will overwtrite characters in s
  * `replace` has 13 variety of overloads

### insert

* `s.insert(10, "middle ")`
* `s.insert(3, "xx", 0, string::npos)`insert "xx" at location 3 in s
  * insert from second string "xx" from 0 to end
  * insert substring

## Numeric Conversions

* from numeric values to string use `to_string` function
* for conversion form string to numeric there is one func for each type
  * for `string s("100hello")` u can `int convInt{stoi(s)}`
    * `stoi()` took inplicite two additional args: `stoi(s, nullptr, 10)`
    * \(1\) pointer to a `size_t` var - stores index of first char not converted, default is nullptr
    * \(2\) an `int` from 2 to 32 representing base, default is base 10

