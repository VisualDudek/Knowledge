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

{% hint style="warning" %}
You should never put a `using namespace` directive within a header file. Every source file that includes your header will dump all the symbols from that using directive into the global namespace. This can cause issues that are very difficult to debug.
{% endhint %}

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

### type aliasing

* defines a name that refers to a previously defined name `using type-alias = type-id;`
* can introduce template parameters into type aliases
  * can perform paritial application on template parameters
  * can define a type alias for a template with a fully specified set of template parameters.

```cpp
namespace foo::bar { }

using String = const char[260];
using ABC = foo::bar;

int main() {
    String saying { "Hello world!" };
}
```

```cpp
// partial application on template parameters
//incomplete example

template <typename T, typename F>
struct NarrowCaster const {
    T cast(F value) {}
};

template <typename F>
using short_caster = NarrowCaster<short, F> 

int main() {
    const short_caster<int> caster; //synonymous with NarrowCaster<short, int>
    const auto cyclic_short = caster.cast(142857);
}
```

### structure bindings

* enable unpack objects
* the obj peel off the POD from top to bottom

```cpp
auto [object-1, object-2, ...] = plain-old-data;

struct TextFile {
    bool success;
    const char* contents;
    size_t n_bytes;
};

TextFile read_text_file(const char* path);

int main() {
    const auto [sucess, constent, length] = read_text_file("README.md");
    --snip--
}
```

### attributes

* you introduce attributes using double brackets `[[ ]]` containing a list of one or more comma-separated attribute elements.
* very rare in use

## Selection stmt

{% hint style="info" %}
if -&gt; SEE C++ page
{% endhint %}

{% hint style="danger" %}
nadal nie czaje `constexpr`
{% endhint %}

## Iteration stmt

{% hint style="info" %}
większość pokryte w C++ -&gt; SEE C++ page
{% endhint %}

* you can define your own types that are also valid range expressions
  * need to exposes a `begin` and `end` method. Both return _interator_
  * _Iterator_ is an obj that supports `operator!=` `operator++` and `operator*`

```cpp
// under hood, a range-based for loop looks:
const auto e = range.end();
for(auto b = range.begin(); b != e; ++b) {
    const auto& element = *b; //iterator supports the dereferencer *
                            //so you can extract the pointed-to element
}
```





END

