---
description: processing files
---

# files

## Summary

* `<fstream>` contains file stream processing types
* `<cstdlib>` exit function prototype

## File

* `ofstream` opens the file for writing data
* `ifstream` opens for reading

### file open modes

* underhood it is bits flag so U can chain them together
* mega dziwna konstrukcja if która dopuszcza overload na condition ???

```
ios::app
ios::ate
ios::in
ios::out
ios::trunc
ios::binary
```

```cpp
if (ofstream output{"foo.txt", ios::out}; output) {}
//  ^^^^^^^^^-success or not will be store as bool into output var
```

### posiotion pointers

* `istream seekg` , `ostream seekp` those are seek get and seek put
* `input.seekg(0)` move pointer to the beginning of file
* `input.clear()` clrear end-of-file indicatior so you can re-read from `input` stream
  * optional second arg indicates the seek direction
* `tellg()` and `tellp()`
  * return current `get` and `put` pointer locations

```cpp
fileObj.seekg(n);  // nth byte form ios::beg
fileObj.seekg(n, ios::cur);  // n bytes forward
fileObj.seekg(n, ios::end);  // n byte from end
fileObj.seekg(0, ios::end);  // move to end
// also used with ostream member function seekp
```

### reading and writing quoted text

* problem with reading quoted text
* fix with `<iomanip>` header

```cpp
100 "Janie Jones" 24.98
intput >> account >> name >> balance // only "Janie is read into string var name

// SOLUTION
// header <iomanip>
input >> account >> quoted(name) >> balance
```

### CSV Comma-Separated Values



## Libs

### rapidcsv

* by d99kris

```cpp
#include "rapidcsv.h"
```



















\---END---
