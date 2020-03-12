# Closures

nonlocal scope - When a variable is assigned in an enclosing function, it is nonlocal to its nested functions. A nonlocal variable can be accessed by the function in which it was defined and all its nested functions.

```python
a = [1, 2, 3]
b = 5
def func(x, y):
    x.append(4)
    y = y + 1
    
func(a, b)
print("a=", a)  #  Output is a=[1, 2, 3, 4]
print("b=", b)  #  Output is b=5
```

 We re passing two variables `a` and `b` to `func`. `a` is a reference to a list which is a mutable object, and `b` is a reference to an integer which is immutable. `func` **receives a copy** of `a` as `x` and a copy of `b` as `y`.

 An _inner function_ \(or _nested function_\) is a function that is defined inside another function \(the outer function\). The local variables of the outer function are said to be nonlocal to its inner function.

{% hint style="info" %}
The inner function can access the nonlocal variables but cannot change them. Reassigning them simply creates a new local variable with the same name in the inner function, and does not affect the nonlocal variable. So if you want to make a change to a nonlocal variable in a nested function, you must use the `nonlocal` keyword.
{% endhint %}

```python
def f():
    x = 1
    y = 2
    def g():
        nonlocal y
```

 If you try to call g\(\) outside f\(\), U will get Error!  But what if we could find a way to call it outside `g()`. Then we would have a second problem. After exiting the outer function, its local variables \(which are nonlocal to `g()`\) are not present in the memory anymore. In that case, the inner function can not access them anymore. Closures make it possible to call an inner function outside the outer function and still access its nonlocal variables.

```python
def f():
    x = 5
    y = 10
    return x
h=f()
```

 So after running `h=f()`, all the local variables of `f()` are gone, and you cannot access `x` and `y` anymore. But we still have the value of `x` that it is returned and stored in `h`.

```python
def f(x):
    def g(y):
        return y
    return g
a = 5
b = 1
h=f(a)
h(b)  # Output is 1

h.__name__ # Output is 'g'
```

{% hint style="warning" %}
but what happens if the outer function has some nonlocal variables?
{% endhint %}

```python
def f(x):
    z = 2
    def g(y):
        return z*x + y
    return g
a = 5
b = 1
h = f(a)
h(b)  # Output is 11
```

 If we run it, we can see that it works without error and `g(y)` can access both the variables `x` and `z`. But how is that possible? After running `f(x)`, we are not in the scope of `f(x)` anymore and the variables `x` and `z` should not be accessible. Why `g(y)` can still access them? That is because the inner function `g(y)` is now a _closure_.

 A closure is an inner function with an extended scope that encompasses nonlocal variables of the outer function. So it remembers the nonlocal variables in the enclosing scopes even if they are not present in memory. A variable like `y` which is in the local scope of the inner function `g(y)` is called a _bound variable_. A function that only has bound variables is called a _close term_. On the other hand, a nonlocal variable like `z` is called a _free variable_ since it is free to be defined outside `g(y)`, and a function that contains free variables is called an _open term_.

 The name “closure” comes from the fact that it captures the bindings of its free \(nonlocal\) variables and is the result of _closing_ an open term.

```python
h = f(a) # Closure, U are binding free vars
h(b) 
```

 Python can keep track of the free variables for each function. We can simply use `__code__.co_freevars` attribute to see which free variables are captured by the inner function. For example for the closure defined in Listing 9 we can write: `h.__code__.co_freevars`

 It is important to note that to have a closure, the inner function should _access_ the nonlocal variables of the outer function. When no free variable is accessed inside the inner function, it does not capture them since it is already a closed term and does not need to be closed.

