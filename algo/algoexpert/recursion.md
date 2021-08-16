# Recursion

## Permutations

* algo with mutating array in place is much harder to grasp
* `res` is only reference and way to access store obj. from deep recursion call-stack
  * trick to pass reference from outer-scope as arg into recursion-function as a storage

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

## N-th Fibo

* storing dict as function arg is absolutly genius

```python
# O(n) time | O(n) space
def getNthFib(n, memo={1:0, 2:1}):
	if n in memo:
		return memo[n]
	else:
		memo[n] = getNthFib(n-1, memo) + getNthFib(n-2,memo)
		return memo[n]
```

