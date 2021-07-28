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

