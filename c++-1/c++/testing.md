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
    void set_collision_threshold_s =(double x) {  // setter
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
constexpt void assert_that(bool statement, const char* message) {
    if (!statement) throw std::runtime_error{ mesage };
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



