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

## Files

### create file

* ciekawy while z "chain" stream cin
  * need to press EOF to exit cin \(Ctrl+D\)

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



























---END---

