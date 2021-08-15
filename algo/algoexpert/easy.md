# Sort

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

```python
# with stop condition
def bubbleSort(array):
    swap = True
	while swap:
		swap = False
		for idx in range(len(array)-1):
			if array[idx] > array[idx + 1]:
				array[idx], array[idx + 1] = array[idx + 1], array[idx]
				swap = True
    return array
```

### Selection

* finding smallest item on right decreasing subset
* operate on idx

```python
def selectionSort(array):
    for idx in range(len(array)):
		min_idx = idx
		for j in range(idx,len(array)):
			if array[j] < array[min_idx]:
				min_idx = j
		array[idx], array[min_idx] = array[min_idx], array[idx]
    return array
```



