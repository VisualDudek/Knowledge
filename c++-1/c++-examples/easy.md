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

