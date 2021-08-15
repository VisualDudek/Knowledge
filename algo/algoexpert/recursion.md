# Recursion

## Permutations

* `res` is only reference and way to access store obj. from deep recursion call-stack

```text
# pseudo-code
call(array, perm, res)

if array is empty:
    add perm to res
else:
    for item in array:
        deepcopy array
        deepcopy perm
        remove item from array
        add item to perm
        call(array, perm, res)
```

