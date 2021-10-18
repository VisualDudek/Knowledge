# Classes

## const member function

* calling non-const member function on const obj. will create compilation error
  * cannot convert this pointer from const Time to Time&
* on const obj U need to call explicit const member function

```cpp
...
Time wakeUp{6, 45, 0};        // non-constant object
const Time noon{12, 0, 0};    // constant object

                              // OBJECT          MEMBER FUNCTION     COMPILATION
wakeUp.setHour(18);           // non-const       non-const           OK
noon.setHour(12);             // const           non-const           BANG 
wakeUp.getHour();             // non-const       const               OK
noon.getMinute();             // const           const               OK
noon.to24HourString();        // const           const               OK
noon.to12HourString();        // const           non-const           BANG
```
