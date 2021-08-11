# Solutions

## Built-in

* need to use unpack operator, same as with `zip()` 

```python
# print items of list obj each in new line uing sep='\n'
l = [1,2,3]
print(*l, sep='\n')
```

## lists

* in user-solution there is lots of space for improvements
  * can optimize input block
  * finding second\_highest score can be done in one line
  * **intesting usage of join**

```python
// sorted list of student(s) having second lowest grade
l = []
s = set()

if __name__ == '__main__':
    for _ in range(int(input())):
        name = input()
        score = float(input())
        s.add(score)
        l.append([name, score])
        
l.sort(key=lambda x: x[1])
ss = list(s)
ss.sort()
r = ss[1]
res = list(filter(lambda x: x[1] == r,l))
res.sort()

for item in res:
    print(item[0])
```

```python
// better
marksheet = []
for _ in range(0,int(input())):
    marksheet.append([input(), float(input())])

second_highest = sorted(list(set([marks for name, marks in marksheet])))[1]
print('\n'.join([a for a,b in sorted(marksheet) if b == second_highest]))
```

## f-String, String

```python
# print each char of 'abc' in new line using .join()
print(('\n').join('abc'))
```

```python
# use on string five string validators
s = "qA2"
print(any([x.isalnum() for x in s]))
print(any([x.isalpha() for x in s]))
print(any([x.isdigit() for x in s]))
print(any([x.islower() for x in s]))
print(any([x.isupper() for x in s]))

# better
t = type(s)
for method in [t.isalnum, t.isalpha, t.isdigit, t.islower, t.isupper]:
    print(any(method(c) for c in s)
    
# or U can use in list: str.isalnum ...

## this is tricky:
# uses all 5 methods on each character and creates a list for each,
# containing the results of each method used on the character.
newList = [[c.isalnum(), c.isalpha(), c.isdigit(), c.islower(), c.isupper()] for c in s]

# rotates lists clockwise to get lists of each method instead
rotated = list(zip(*newList))
#explanation:
# each method is used on each character
newList = [[True, True, False, False, True ] # methods used on P
           [True, False, True, False, False] # methods used on 1
           [True, False, True, False, False] # methods used on 2
           [True, True, False, False, True]] # methods used on A

# rotates clockwise to get lists of methods' returned values
rotated = [[True,  True,  True,  True ] # results for .isalnum()
           [True,  False, False, True ] # results for .isalpha()
           [False, True,  True,  False] # results for .isdigit()
           [False, False, False, False] # results for .islower()
           [True,  False, False, True]] # results for .isupper()
```

## itertools

```python
itertools.permutations('ABCD', 2)
#can be daone using product and exclude entries with reapeted elements
filter(lambda x: x[0] != x[1],itertools.product('ABCD', 'ABCD'))
```

## tips & tricks

```python
for item in permutations(ss, n):
    print(''.join(item))
# can be shortened by unpack operator and print sep
print(*[''.join(i) for i in permutations(sorted(s),int(n))],sep='\n')
```

