## Bad Design Example ❌

```cpp
// ❌ BAD: Client code directly decides which object to create
// Adding a new vehicle type requires modifying THIS code — OCP violation!

#include <iostream>
#include <string>

class Truck {
public:
    void deliver() { std::cout << "Delivering by Truck 🚚\n"; }
};

class Van {
public:
    void deliver() { std::cout << "Delivering by Van 🚐\n"; }
};

class Motorcycle {
public:
    void deliver() { std::cout << "Delivering by Motorcycle 🏍️\n"; }
};

void planDelivery(const std::string& type) {
    // Every new vehicle = modify this function
    if (type == "truck") {
        Truck t;
        t.deliver();
    } else if (type == "van") {
        Van v;
        v.deliver();
    } else if (type == "motorcycle") {
        Motorcycle m;
        m.deliver();
    }
    // Adding "drone"? Must modify this function ❌
}

int main() {
    planDelivery("truck");
    planDelivery("motorcycle");
    return 0;
}
```


## Good C++ Implementation ✅

```cpp
#include <iostream>
#include <memory>
#include <string>

// ── Product Interface ──────────────────────────────────────
class ITransport {
public:
    virtual void deliver() = 0;
    virtual ~ITransport()  = default;
};

// ── Concrete Products ──────────────────────────────────────
class Truck : public ITransport {
public:
    void deliver() override {
        std::cout << "📦 Delivering by Truck — large cargo on highways\n";
    }
};

class Van : public ITransport {
public:
    void deliver() override {
        std::cout << "📦 Delivering by Van — medium cargo, city routes\n";
    }
};

class Motorcycle : public ITransport {
public:
    void deliver() override {
        std::cout << "📦 Delivering by Motorcycle — small parcels, fast\n";
    }
};

// NEW: Add drone WITHOUT changing any existing code ✅ (OCP)
class Drone : public ITransport {
public:
    void deliver() override {
        std::cout << "📦 Delivering by Drone — remote areas, overnight\n";
    }
};

// ── Creator (Abstract) ─────────────────────────────────────
class Logistics {
public:
    // The Factory Method — subclasses decide what to create
    virtual std::unique_ptr<ITransport> createTransport() = 0;
    virtual ~Logistics() = default;

    // Template method that uses the factory method
    // This code never changes, even when new transports are added
    void planDelivery() {
        auto transport = createTransport(); // polymorphic creation
        std::cout << "Planning route...\n";
        transport->deliver();
        std::cout << "Delivery complete!\n\n";
    }
};

// ── Concrete Creators ──────────────────────────────────────
class RoadLogistics : public Logistics {
public:
    std::unique_ptr<ITransport> createTransport() override {
        return std::make_unique<Truck>();
    }
};

class CityLogistics : public Logistics {
public:
    std::unique_ptr<ITransport> createTransport() override {
        return std::make_unique<Motorcycle>();
    }
};

// Adding new logistics type — NO existing code changes ✅
class AirLogistics : public Logistics {
public:
    std::unique_ptr<ITransport> createTransport() override {
        return std::make_unique<Drone>();
    }
};

int main() {
    // Each logistics type creates the right vehicle automatically
    RoadLogistics road;
    road.planDelivery();

    CityLogistics city;
    city.planDelivery();

    AirLogistics air;
    air.planDelivery();

    return 0;
}
```

