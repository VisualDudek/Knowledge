# Searching

## Binary 

### O \(log n\)

What does the time complexity O\( log n\) actually mean?

```text
[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16]   # n = 16
# divide it in middle
[1,2,3,4,5,6,7,8]   # 16 * (1/2) = 8
# once again
[1,2,3,4]   # 8 * (1/2) = 4
# ...
[1,2]   # 4 * (1/2) = 2
# ...
[1]   # 2 * (1/2) = 1

# summ up
16 * (1/2)^4 = 1
# we can have general rule
n * (1/2)^k = 1
# solve by k
n = 2^k
#which is

log n = k  # base 2, k = is the number of division by half
```

## in sorted Matrix

* keey is to start from top-right corner BC moving only left or right will dfinitely cut out non-target nums

```text
# pseudo-code
#starting from top-right corner
do-while: x,y are still in bound of matrix
    if curr_pos = target: win
    elif curr_pos > target: move left
    else:
        # implicite curr_pos < target: move down
end of while
# implicite out of bound
return None
```

