# Chain Of Responsibility Pattern

Chain of Responsibility is a behavioral design pattern that lets you pass requests along a chain of handlers. 
Upon receiving a request, each handler decides either to process the request or to pass it to the next handler in the chain.<br>
In this pattern, normally each receiver contains reference to another receiver. 
If one object cannot handle the request then it passes the same to the next receiver and so on. <br>

For example, a chain of bank responsibility; <br>
Let's consider a bank with a daily cash withdrawal amount of 10k USD for a person working at the cashier.
A customer who came to this bank said that he wanted to withdraw 95k USD from the person at the cashier.
As per bank rules, this process must be approved by the cashier, middle level manager, bank manager and regional officer, respectively.
When we look at the example, there is an approval structure that is connected to each other in the form of a chain.


---

![cor](https://github.com/fbasatemur/design_patterns/blob/main/diagrams/beh_cor.png)

---

````c++
//
// Created by fbasatemur on 6.11.2021.
//

#include <iostream>
#include <string>

class Employee;

class Withdraw {
public:
    int _amount;
    std::string _customerId, _currentType, _sourceAccountId;

    Withdraw(std::string customerId,
             int amount,
             std::string currentType,
             std::string sourceAccountId) : _amount(amount), _customerId(customerId),
                                            _currentType(currentType), _sourceAccountId(sourceAccountId) {}

    void Process(Employee *emp);
};

class Employee {
public:
    Employee *_nextApproving = nullptr;

    void SetNextApproving(Employee *nextApproving) {
        _nextApproving = nextApproving;
    }

    virtual void ProcessRequest(Withdraw *req) = 0;
};

void Withdraw::Process(Employee *emp) { emp->ProcessRequest(this); }

class Cashier : public Employee {
public:
    void ProcessRequest(Withdraw *req) override {
        if (req->_amount <= 10000)
            std::cout << "Withdrawal confirmed by cashier." << std::endl;
        else if (_nextApproving != nullptr) {
            std::cout << "Cashier transaction limit 10000!" << std::endl;
            std::cout << "Redirecting to MidManager..." << std::endl;
            _nextApproving->ProcessRequest(req);
        }
    }
};

class MidManager : public Employee {
public:
    void ProcessRequest(Withdraw *req) override {
        if (req->_amount <= 50000)
            std::cout << "Withdrawal confirmed by MidManager." << std::endl;
        else if (_nextApproving != nullptr) {
            std::cout << "MidManager transaction limit 50000!" << std::endl;
            std::cout << "Redirecting to BankManager..." << std::endl;
            _nextApproving->ProcessRequest(req);
        }
    }
};

class BankManager : public Employee {
public:
    void ProcessRequest(Withdraw *req) override {
        if (req->_amount <= 75000)
            std::cout << "Withdrawal confirmed by BankManager." << std::endl;
        else if (_nextApproving != nullptr) {
            std::cout << "BankManager transaction limit 75000!" << std::endl;
            std::cout << "Redirecting to RegionalManager..." << std::endl;
            _nextApproving->ProcessRequest(req);
        }
    }
};

class RegionalManager : public Employee {
public:
    void ProcessRequest(Withdraw *req) override {
        if (req->_amount <= 100000)
            std::cout << "Withdrawal confirmed by RegionalManager." << std::endl;
        else if (_nextApproving != nullptr) {
            std::cout << "Max transaction limit 100000!" << std::endl;
            std::cout << "Withdrawal denied ! " << std::endl;
        }
    }
};

int main() {

    Withdraw *withdraw = new Withdraw("93dc-cdbb-4f09-af1a-ed15", 95000, "USD", "82526242838");

    Employee *cashier = new Cashier();
    Employee *midManager = new MidManager();
    Employee *bankManager = new BankManager();
    Employee *regManager = new RegionalManager();

    cashier->SetNextApproving(midManager);
    midManager->SetNextApproving(bankManager);
    bankManager->SetNextApproving(regManager);

    withdraw->Process(cashier);

    delete withdraw, delete cashier, delete midManager, delete bankManager, delete regManager;
    return 0;
}
```