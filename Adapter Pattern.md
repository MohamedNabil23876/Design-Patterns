
# BAD DESIGN ❌


```cpp
#include <iostream>

class OldPrinter {
public:
    void printOld(std::string text) {
        std::cout << "Old Printer: " << text << "\n";
    }
};

class Printer {
public:
    virtual void print(std::string text) = 0;
};

int main() {

    OldPrinter printer;


    printer.print("Hello"); // 


    printer.printOld("Hello");

}
```


---

# 1️⃣ Target Interface


```cpp
class Printer {
public:
    virtual void print(std::string text) = 0;
};


class OldPrinter {
public:
    void printOld(std::string text) {
        std::cout << "Old Printer: " << text << "\n";
    }
};

class PrinterAdapter : public Printer {

private:
    OldPrinter oldPrinter;

public:
    void print(std::string text) override {


        oldPrinter.printOld(text);

    }
};


int main ()
{
Printer * ptr = new  PrinterAdapter()
ptr->print();


}
```

