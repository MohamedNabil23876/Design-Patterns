

## Bad Design Example ❌

```cpp
// ❌ BAD: Multiple instances can be created — no protection
#include <iostream>
#include <string>

class Logger {
    std::string logFile_;
public:
    Logger(const std::string& file) : logFile_(file) {}

    void log(const std::string& msg) {
        std::cout << "[" << logFile_ << "] " << msg << "\n";
    }
};

int main() {
    // Nothing stops creating multiple loggers pointing to different files!
    Logger logger1("app.log");
    Logger logger2("debug.log");   // ← second instance!
    Logger logger3("errors.log");  // ← third instance!

    // Which one is authoritative? All three exist simultaneously.
    // Different parts of the app write to different files.
    logger1.log("User logged in");
    logger2.log("User logged in");  // duplicate!
    logger3.log("User logged in");  // triplicate!

    return 0;
}
```


```cpp
#include <iostream>

class Logger {
public:

    static Logger& getInstance() {
        static Logger instance; // يتعمل مرة واحدة فقط
        return instance;
    }

    void log() {
        std::cout << "Logging message\n";
    }

private:
    Logger() {
        std::cout << "Logger created once\n";
    }
};

int main() {

    Logger& log1 = Logger::getInstance();
    Logger& log2 = Logger::getInstance();

    log1.log();
    log2.log();

    std::cout << (&log1 == &log2) << std::endl; // true

}
```

