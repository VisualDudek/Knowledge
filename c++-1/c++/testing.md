# TESTING

## Unit Tests

### example

* lead architect wants you to expose an observe method so you can subscribe to SpeedUpdate and CarDetected events on the service bus.
  * \(1\) you decide to build a class called _AutoBrake_ that you'll initialize in the program's entry point. The _AutoBrake_ class will keep a reference to the _publish_ method of the service bus, and it will subscirbe to _SpeedUpdate_ and _CarDetected_ events through its _observe_ method.
* \(2\) servise integrates into the car's software

```cpp
struct SpeedUpdate {
    double velocity_mps;
};

struct CarDetected {
    double distance_m;
    double velocity_mps;
};

struct BreakCommand {
    double time_to_collison_s;
};

// publish the BreakCommand using ServiceBuss object that has a publish method
struct ServiceBus {
    void publish(const BreakCommand&);
    --snip--
};

// (1)
template <typename T>
struct AutoBrake {
    AutoBrake(const T& publish);
    void observe(const SpeedUpdate&);
    void observe(const CarDetected&);
private:
    const T& publish;
    --snip--
};
```

```cpp
// (2)
int main() {
    ServiceBus bus;
    AutoBrake auto_brake{ [&bus] (const auto& cmd) {
                            bus.publish(cmd);
                        }
    };
    
    while (true) {  // Service bus's event loop
        auto_brake.observe(SpeedUpdate{ 10L });
        auto_brake.observe(CarDetected{ 250L, 25L });
    }
}
```

* before we can write tests, need to write a _skeleton class_, which implements an interface but provides no functionality.
* _AutoBrake_ has a single constructor that takes the template parameter _publish_, which you save off into a _const_ member.
* you provide two differernt observe functions

```cpp
template <typename T>
struct AutoBrake {
    AutoBrake(const T& publish) : publish{ publish } {}
    void observe(const SpeedUpdate& cd) { }
    void observe(const CarDetected& cd) { }
    void set_collision_threshold_s(double x) {  // setter
        collision_threshold_s = x;
    }
    double get_collision_threshold_s() const {  // getter
        return collision_threshold_s;
    }
    double get_speed_mps() const {  // getter
        return speed_mps;
    }
    
private:
    double collision_threshold_s;
    double speed_mps;
    const T& publish;
};
```

### Assertion

```cpp
constexpr void assert_that(bool statement, const char* message) {
    if (!statement) throw std::runtime_error{ message };
}

int main() {
    assert_that(1 + 2 > 2, "ABC");
    assert_that(24 == 42, "This assertion will generate an exception");
}
```

{% hint style="info" %}
... empty implementation is sometimes called a stub.
{% endhint %}

* **Requirement**: Initial Speed is Zero
* first construct an _AutoBrake_ with an empty _BreakCommand publish_ function

```cpp
void initial_speed_is_zero() {
    AutoBrake auto_brake{ [](const BrakeCommand&) {} };
    assert_that(auto_brake.get_speed_mps() == 0L, "speed not equal 0");
}
```

* **Test Harnesses**: is code that executes unit tests

```cpp
void run_test(void(*unit_test)(), cost char* name) {
    try {
        unit_test();
        printf("[+] Test %s susccesful.\n", name);
    } catch (const std::exception& e) {
        printf("[-] Test failure in %s. %s.\n", name, e.what());
    }
}

// run it in main
int main() {
    run_test(initial_speed_is_zero, "initial speed is 0");
}
```

* **Getting the Test to Pass**: we need to intialize _speed\_mps_ to zero in the constructor of _AutoBrake_ 

```cpp
template <typename T>
struct AutoBrake {
    AutoBrake(const T& publish) : speed_mps{}, publish{ publish } {}
    --snip--
};
```

* **Requirement**: Default Collision Threshold is Five
* add next test into test harness
* test failure? fix it

```cpp
void initial_sensitivity_id_five() {
    AutoBrake auto_brake{ [](const BrakeCommand&) {} };
    assert_that(auto_brake.get_collision_threshold_s() == 5L,
        "sensitivity is not 5");
}

int main() {
    --snip--
    run_test(initial_sensitivity_is_five, "initial sensitiviti is 5");
}
```

* **Requirement**: sensitivity must always be greater than one
* czyli sprawczamy czy rzuci wyjatkiem jesli bedziemy probować ustawic sensitivity na mniej niz 1, jesli rzuci wyjatkiem to wszystko jes ok i wychodzimy z funkcji, jesli nie rzuci wyjatkiem to sami zglaszamy failed test poprzez wywolanie assert\_that z false
* add to test harnes
* fix it

```cpp
void sensitivity_greater_than_1() {
    AutoBrake auto_brake{ [](const BrakeCommand&) {} };
    try {
        auto_brake.set_collision_threshold_s(0.05L);
    } catch (const std::exception&) {
        return;
    }
    assert_that(false, "no exception thrown");
}

// fix it
...
    void set_collision_threshold_s(double x) {
        if (x < 1) throw std::exception{ "Collision less than 1." };
        // exception musiałem zamienic na runtime_error
        collision_threshold_s = x;
    }
```

* **Req**: save the car's speed between updates
* bardzo ciekawy fix w tej architekturze tj. przekazywanie parametru w klasie
  * zobacz jak jet przekazywany atrybut do klasy
* there is several assers in this test

```cpp
void speed_is_saved() {
    AutoBrake auto_brake{ [](const BrakeCommand&) {} };
    auto_brake.observe(SpeedUpdate{ 100L });
    assert_that(100L == auto_brake.get_speed_mps(), "speed is not saved to 100");
    auto_brake.observe(SpeedUpdate{ 50L });
    assert_that(50L == auto_brake.get_speed_mps(), "speed is not saved to 50");
    auto_brake.observe(SpeedUpdate{ 0L });
    assert_that(0L == auto_brake.get_speed_mps(), "speed is not saved to 0");
}

// fix
...
    void observe(cost SpeedUpdate& x) {
        speed_mps = x.velocity_mps;
    }
```

* **Req**: AutoBrake publishes a BrakeCommand when collision detected
* it time collision is greater than zero and less than or equal to collision\_threshold\_s you invoke publish with a BrakeCommand
* fix it with `observe(const CarDetected& cd)` definition

```cpp
void alert_when_imminent() {
    int brake_commands_published{}; // will keep track of the number of times
                            //taht the publish callback is invoked.
    AutoBrake auto_brake{
        [&brake_commands_published](const BrakeCommand&) {
            brake_commands_published++;
        }
    }; // labda used to construct your auto_brake
    //will pass by reference into labda local var
    auto_brake.set_collision_threshold_s(10L);
    auto_brake.observe(SpeedUpdate{ 100L });
    auto_brake.observe(CarDetected{ 100L, 0L }); // detect car 100 meters away 
    //traveling  at 0 meters per second
    //AutoBrake should determine that a collision will occure in 1 second
    //this should trigger a callback, which will increment local var pased 
    //into labda
    assert_that(brake_commands_published == 1, "brake cmd published not one");
}
```

{% hint style="danger" %}
capture lambda wywala seg falut:

nie da rady incrementować local var poza scopem
{% endhint %}

{% hint style="info" %}
kod poniżej pomaga w zrozumieniu jak działa przekazywana w init funkcja lambda

nadal nie widze edge w takiej konstrukcji
{% endhint %}

```cpp
void observe(const CarDetected& cd) {
    const auto relative_velocity_mps = speed_mps - cd.velocity_mps;
    const auto time_to_collision_s = cd.distatnce_m / relative_velocity_mps;
    if (time_to_collision_s > 0 &&
        time_to_collision_s <= collision_threshold_s) {
        publish(BreakCommand{ time_to_collision_s });
    }
}
```

### adding a service-bus interface

You want to refactor the service bus. You want to accept a `std::function` to subscribe to each service, as in the new `IServiceBus` interface.

```cpp
#include <functional>

using SpeedUpdateCallback = std::function<void(const SpeedUpdate&)>;
using CarDetectedCallback = std::function<void(const CarDetected&)>;

struct IServiceBus {
    virtual ~IServiceBus() = default;
    virtual void publish(const BrakeCommand&) = 0;
    virtual void subscribe(SpeedUpdateCallback) = 0;
    virtual void subscribe(CarDetectedCallback) = 0;
};
```











--- END---

