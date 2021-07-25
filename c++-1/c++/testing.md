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

```cpp
template <typename T>
struct AutoBrake {
    AutoBrake(const T& publish) : publish{ pubblish } {}
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

