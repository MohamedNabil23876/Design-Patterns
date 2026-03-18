

```cpp
CreditCardPayment payment;
CreditCardPayment  payment1; 
PayPalPayment      payment2;
CashPayment       payment3;
```

```cpp
PaymentFactory::createPayment("credit");
```

---


# Bad Design ❌

```cpp
#include <iostream>
#include <string>

class Payment {
public:
    virtual void pay() = 0;
};

class CreditCardPayment : public Payment {
public:
    void pay() override {
        std::cout << "Paying with Credit Card\n";
    }
};

class PayPalPayment : public Payment {
public:
    void pay() override {
        std::cout << "Paying with PayPal\n";
    }
};

class CashPayment : public Payment {
public:
    void pay() override {
        std::cout << "Paying with Cash\n";
    }
};
// 
int main() {

    std::string type = "paypal";

    Payment* payment;

    if(type == "credit")
        payment = new CreditCardPayment();
    else if(type == "paypal")
        payment = new PayPalPayment();
    else if(type == "cash")
        payment = new CashPayment();

    payment->pay();
}
```

---



# Good Design ✅


```cpp
class Payment {
public:
    virtual void pay() = 0;
};

class CreditCardPayment : public Payment {
public:
    void pay() override {
        std::cout << "Paying with Credit Card\n";
    }
};

class PayPalPayment : public Payment {
public:
    void pay() override {
        std::cout << "Paying with PayPal\n";
    }
};

class CashPayment : public Payment {
public:
    void pay() override {
        std::cout << "Paying with Cash\n";
    }
};
class PaymentFactory {
public:

    static Payment* createPayment(const std::string& type) {

        if(type == "credit")
            return new CreditCardPayment();

        if(type == "paypal")
            return new PayPalPayment();

        if(type == "cash")
            return new CashPayment();

        return nullptr;
    }
};



int main() {

    Payment* payment = PaymentFactory::createPayment("paypal"); 
                       //   new PayPalPayment();

    payment->pay();

}
```
