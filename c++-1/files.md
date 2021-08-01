---
description: processing files
---

# files

## Summary

* `<fstream>` contains file stream processing types
* `<cstdlib>` exit function prototype

## File

* `ofstream` opens the file

### file open modes

* underhood it is bits flag so U can chain them together
* mega dziwna konstrukcja if która dopuszcza overload na condition ???

```text
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

