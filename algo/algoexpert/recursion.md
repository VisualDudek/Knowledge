# Recursion

## Permutations

* `res` is only reference and way to access store obj. from deep recursion call-stack
* algo with mutating array in place is much harder to grasp

```text
# algo
[1,2,3,4] 
[1] and perm [2,3,4]
[2] ...
[3] ...
[4] ...

# [2,3,4] will break into each elemnt plus perm of other elements
```

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

