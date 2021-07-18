# COMPILE-TIME POLYMORPHISM

## Templates

> The big idea is that, rather than copying and pasting common code all over ther place, you write a single template; the compiler generates new template instatnces when it encounters a new combination of types in the tmeplate parameters.

## Declaring Templates

{% hint style="info" %}
The coexistence of the `typename` and `class` keyword is unfortunate and confusing. They mean the same thing.
{% endhint %}

```cpp
// template class definition
template <typename X, typename Y, typename Z>
struct MyTemplateClass {
    X foo(Y&);
private:
    Z* member;
};
```

