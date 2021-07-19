---
description: >-
  wychodzimy od problemu implemntacji Loggera następnie pokazane problemy przy
  utzrymaniu i rozwoju kodu
---

# RUNTIME POLYMORPHISM

## summary

1. wychodimy od miplementacji prostego Loggera
2. jak rozwiazać zarzadzanie wieksza ilością implementacji Loggera np Console i File?
3. brniemy w rozwiązania typu `enum class` czyli rownoległe utrzymywanie kilku implementacji oraz dodatkowego obiektu który utzymuej wiedze nt. dostępnych implementacji -&gt; w następstwie musi się pojawić "narzędzie" w obiekcie który konsumuje Loggera które na podstwie obiektu w którym przetrzymujemy informacje o dostępnych implementacjach będzie urzywał tego którego potrzebujemy
4. rozwiązanie w punkcie powyżej generuje co najmniej kilka problemów \(w przypadku rozbudowy kodu\):
   1. utzymanie spójnej deklaracji Loggera, ta sama struktura dla wszystkich implementacji
   2. zmiana kodu w co najmniej trzech miejscach, nowa implementacja Loggera, obiekt do przechowywania informacji o wszystkich implementacjach, narzedzie wewnątrz consumenta Loggera np swich-case work-flow code block.

There must be a better way!

1. 
## Polymorphism

* interfaces
* object composition
* inheritance
* compile-time poly.
* runtime poly.

> _Polymorphic code_ is code you write once and can reuse with different types.

## Example

* logging with a custom `ConsoleLogger` class

```cpp
struct ConsoleLogger {
    void log_transfer(long from, long to, double amount) {
        printf("%ld -> %ld: %f\n", from, to, amount);
    }
};

struct Bank {
    void make_transfer(long from, long to, double amount) {
    --snip--
    logger.log_transfer(from, to, amount);
    }
    ConsoleLogger logger; // tutaj ciekawie chyba lepiej dziedziczyć?
};

int main() {
    Bank bank;
    bank.make_transfer(1000, 2000, 49.95);
    bank.make_transfer(2000, 4000, 20.00);
}
```

{% hint style="danger" %}
What if I need several Loggers and switch beetwen in runtime?

\(1\) u can use `enum class`
{% endhint %}

```cpp
// chyba karkołomne rozwiązanie w oparciu o switch-case
struct FileLogger {
    void log_transfer(long from, long to, double amount) {
    --snip--
    printf("[file] %ld,%ld,%f\n", from, to, amount);
    }
};

struct ConsoleLogger {
    void log_transfer(long from, long to, double amount) {
     printf("[cons] %ld -> %ld: %f\n", from, to, amount);
    }
};

enum class LoggerType {
    Console,
    File
};

struct Bank {
    Bank() : type { LoggerType::Console } {}
    void set_logger(LoggerType new_type) {
      type = new_type;
    }
    
    void make_transfer(long from, long to , double amount) {
      --snip--
      switch(type) {
      case LoggerType::Console: {
        consoleLogger.log_transfer(from, to, amount);
        break;
      } case LoggerType::File: {
        fileLogger.log_transfer(from, to, amount);
        break;
      } default: {
        throw std::logic_error("Unknown Logger type encountered.");
      } }
    }
private:
  LoggerType type;
  ConsoleLogger consoleLogger;
  FileLogger fileLogger;
};

int main() {
  Bank bank;
  bank.make_transfer(1000, 2000, 49.95);
  bank.set_logger(LoggerType::File);
  bank.make_transfer(3000, 2000, 75.00);
}

// Wady tego rozwązania ujawniają się gdy trzeba dodać nowy Logger
/*
1. You need to write a new logger type
2. need to add new enum value to the enum class LoggerType
3. must add a new case in the switch-case stmt
4. need add the new logging class as a member to Bank
*/
```

{% hint style="info" %}
There must be a better way.

Consider an alternative approach where `Bank` holds a pointer to a logger. This way, you can set the pointer directly and get rid of `LoggerType` entirely. Y**ou exploit the fact that your loggers have thte same function prototype**. This is the idea behind an _interface_: the `Bank` class doesn't need to know the implementation details of the `Logger` reference it holds, just how to invoke its methods.
{% endhint %}

### Interface

* interface
* implementation
* consumers of class

### Object Composition and Implementation Inheritance

## Defining Interfaces

* `virtual`
* `=0` you _require_ a derived class to implement the method
* `override`
* class inheritance `struct DerivedClass : BaseClass {`

```cpp
// defining interface
struct Logger {
    virtual ~Logger() = default;
    virtual void log_transfer(long from, long to, double amount) = 0;
};

struct ConsoleLogger : Logger {
    void log_transfer(long from, long to, duble amount) override {
        printf("%ld -> %ld: %f\n", from, to, amount);
    }
};
```

### Base Class Inheritance

```cpp
// class inheritance
struct DerivedClass : BaseClass {
    --snip--
};
```

```cpp
// bardzo ważny przykład który pokazuje co umożliwia dziedziczenie class
struct BaseClass {};
struct DerivedClass : BaseClass {};
void are_belong_to_us(BaseClass& base) {}

int main() {
    DerivedClass derived;
    are_beleong_to_us(derived); // GAME-CHANGER: ten fragment pozostaje niezmienny
    // w dalszym developmencie kodu
}
```

### Member Inheritance

{% hint style="info" %}
private member do not inherited by derived classes.
{% endhint %}

### virtual Methods

If you want to permit a derived class to override a base class's methods, you use the `vurtial` keyword. By adding `virtual` to a method's definition, you declare that a derived class's implementation should be used if one is supplied. Whitin the implementation, you add the `override` keyword to the method's declaration.

### Pure-Virtual Classes and Virtual Destructors

### Using Interfaces

As a _consumer_ you can only deal in references or pointers to interfaces. The compiler cannot know ahead of time how much memory to allocate for the underlying type: if the compliler could know the underlying type, you would be better off using trmplates.

> **Constructor injection**, you typically use an interface reference. BC reference cannot be reseated, they won't change for the lifetime of the object.
>
> **Property injection**, you use a method to set a pointer member. This allows you to change the object to which the member points.

## Updating the Bank Logger

```cpp
// teraz zacznie się wszystko wyjaśniać
--snip--
// Include Logger interface and its multiple logger implementations.
struct Logger {
    virtual ~Logger() = default;
    virtual void log_transfer(long from, long to, double amount) = 0;
};

struct ConsoleLogger : Logger {
    void log_transfer(long from, long to, double amount) override {
        printf("[cons] %ld - %ld: %f\n", from, to, amount);
    }
};

struct FileLogger : Logger {
    void log_transfer(long from, long to, double amount) override {
        printf("[file] %ld,%ld,%f\n", from, to, amount);
    }
};

// konstrukcja User/Consumer-a pozostanie niezmienna
// ale logger jest stały dla life-time bank object
// ^--- yes, there is a better way
struct Bank {
    Bank(Logger& logger) : logger{ logger } {}
    void make_transfer(long from, long to, double amount) {
        --snip--
        logger.log_transfer(from, to, amount);
    }
private:
    Logger% logger;
};

int main() {
    ConsoleLogger logger;
    Bank bank{ logger };
    bank.make_transfer(1000, 2000, 49.95);
}

// The Bank class constructor sets the value of logger using a member initializer.
//References can't be reseated, so the object tht logger points to 
//doesn't change for the lifetime of Bank.
//You fix (hard-link) your logger choice upon Bank Construction.
```

