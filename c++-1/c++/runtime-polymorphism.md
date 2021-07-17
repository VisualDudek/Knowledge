# RUNTIME POLYMORPHISM

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
```

