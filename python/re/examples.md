# examples

## any single char except the newline

* you need to escape `.` dot
* remember to restrict to bgn `^` and end `$` of line bc you can get more than U want

```python
r"^\S\.\S\.\S\.\S$"   
```

