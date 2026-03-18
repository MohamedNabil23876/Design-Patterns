## Bad Design Example 

```cpp
// ❌ BAD: Constructor with too many parameters — unreadable and error-prone

class Pizza {
public:
    // Which bool is which? Easy to swap arguments accidentally!
    Pizza(std::string size, bool extraCheese, bool pepperoni,
          bool mushrooms, bool olives, bool thinCrust,
          int bakingTemp, int bakingMinutes) {
        // ...
    }
};

int main() {
    // What does this mean? Is thin crust on or off?
    // Did I accidentally swap bakingTemp and bakingMinutes?
    Pizza p("large", true, false, true, false, true, 220, 15);
    // Completely unreadable! ❌
    return 0;
}
```

## Good C++ Implementation ✅

```cpp
#include <iostream>
#include <string>
#include <vector>

// The product we're building
class Pizza {
public:
    std::string           size;
    bool                  extraCheese  = false;
    bool                  pepperoni    = false;
    bool                  mushrooms    = false;
    bool                  thinCrust    = false;
    int                   bakingMinutes = 12;
    std::vector<std::string> extras;

    void describe() const {
        std::cout << "=== Pizza Order ===\n"
                  << "Size:          " << size << "\n"
                  << "Extra Cheese:  " << (extraCheese ? "Yes" : "No") << "\n"
                  << "Pepperoni:     " << (pepperoni   ? "Yes" : "No") << "\n"
                  << "Mushrooms:     " << (mushrooms   ? "Yes" : "No") << "\n"
                  << "Thin Crust:    " << (thinCrust   ? "Yes" : "No") << "\n"
                  << "Baking Time:   " << bakingMinutes << " minutes\n";
        for (auto& e : extras) std::cout << "Extra:         " << e << "\n";
        std::cout << "==================\n";
    }
};

// The Builder — constructs Pizza step by step
class PizzaBuilder {
    Pizza pizza_;
public:
    // Each method returns *this to enable method chaining (Fluent Interface)
    PizzaBuilder& setSize(const std::string& size) {
        pizza_.size = size;
        return *this;
    }
    PizzaBuilder& addExtraCheese() {
        pizza_.extraCheese = true;
        return *this;
    }
    PizzaBuilder& addPepperoni() {
        pizza_.pepperoni = true;
        return *this;
    }
    PizzaBuilder& addMushrooms() {
        pizza_.mushrooms = true;
        return *this;
    }
    PizzaBuilder& setThinCrust() {
        pizza_.thinCrust = true;
        return *this;
    }
    PizzaBuilder& setBakingTime(int minutes) {
        pizza_.bakingMinutes = minutes;
        return *this;
    }
    PizzaBuilder& addExtra(const std::string& item) {
        pizza_.extras.push_back(item);
        return *this;
    }

    // Final step — returns the fully constructed product
    Pizza build() {
        if (pizza_.size.empty()) {
            throw std::logic_error("Pizza must have a size!");
        }
        return pizza_;
    }
};

int main() {
    // Readable, self-documenting, hard to make mistakes ✅
    Pizza myPizza = PizzaBuilder()
        .setSize("Large")
        .addExtraCheese()
        .addPepperoni()
        .setThinCrust()
        .setBakingTime(s
        .addExtra("Jalapeños")
        .build();

    myPizza.describe();

    // A completely different pizza — same builder, different configuration
    Pizza simplePizza = PizzaBuilder()
        .setSize("Small")
        .addMushrooms()
        .build();

    simplePizza.describe();

    return 0;
}
```

