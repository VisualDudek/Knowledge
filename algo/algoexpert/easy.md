# Easy

## Sort

### BubbleSort

* bombelki uciekają do góry
* każdy bombelek to swap\(x\[i\], x\[i+1\]\)

```python
# brute-forece solution
def bubbleSort(array):
    for i in range(len(array)):
		for j in range(len(array)-1):
			if array[j] > array[j+1]:
				array[j], array[j+1] = array[j+1], array[j]
	return array
```

```python
# take advatege of fact thet in each iteration there will be +one sorted end of
#array
def bubbleSort(array):
	n = len(array)
	for i in range(n-1):
		for j in range(n-1-i):
			if array[j] > array[j+1]:
				array[j], array[j+1] = array[j+1], array[j]
	return array
```

