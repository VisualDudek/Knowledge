# Untitled

## Calculate first k digits

trick is to calculate iteger part of N divated by 10 to the number of digits of N minus k

* remember that: `log(a**b)` is equal `b * log(a)`
* useful when need to calculate first digits of `a**b` when a and b are realy big

```text
N = 1234567    // N has 7 digits
k = 3        // we want to calculate first three digits
1234567 / 10**(7-3) = 123.4567 // integer part is what we are looking for

// how to calculate number of digits of given number?
int(log N)+1    // log base = 10
// explanation
100       = 10**2
1_000     = 10**3
10_000    = 10**4
100_000   = 10**5

// we have:
N / 10**(int(log N)+1-k) and take int of result

// we can ease calc by first taking log base of fraction
log N - int(log N) - 1 + k
// calculate it and raise 10 to the result
```



