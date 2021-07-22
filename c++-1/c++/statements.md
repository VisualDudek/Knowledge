# STATEMENTS

## Expression stmt

* _expression statement_ is an expression followed by a sebicolon `( ; )`

## Compound stmt

* also called blocks, sequence of stmt enclosed by braces `{ }`.
* obj with automatic storage duration declared within a block scope have lifetimes bound by the block.

## Declaration stmt

* introduce identifiers, such as functions, templates, and namespaces

### functions

* signature or prototype, The declaration doesn't need to include parameter names, only their types.
* you can separate method declarations from their definitions.
  * with definition need to use scope resolution operator to get into class namespace
  * will be handy when dealing with multi-source-file program

```cpp
// signature do not need parameter names only types
void foo(int&);
...
void foo(int& name) {}
```

### namespaces

* keyword `namespace`
* can be nested
  * create by nested blocks or using scope-resolution operator `::`
* use _directive_ `using` to avoid a lot of typing. Imort namescope into the current namespace.

```cpp
namespace foo {
    // all symbols declared within this block
    //belong to the foo namespace
}
```

```cpp
// extreme example

namespace foo::bar {
    enum class Color {
        Red,
        Blue
    };
}

int main() {
    auto x {foo::bar::Color::Red};
}
```













END

