# Solutions

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

