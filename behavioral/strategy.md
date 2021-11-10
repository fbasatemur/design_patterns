# Strategy Pattern

There may be more than one method (algorithm) to perform an operation. 
The strategy design pattern (also known as the policy pattern) is used to select and apply a method according to the situation. 
Each method (algorithm) is implemented for a class. 
That is, the algorithm of a class at runtime can be changed according to the strategy. Thus, it is a behavioral design pattern.<br>

For instance, a class that performs validation on incoming data may use the strategy pattern to select a validation algorithm depending on the type of data, the source of the data, user choice, or other discriminating factors. 
These factors are not known until run-time and may require radically different validation to be performed.<br>

---

![strategy](https://github.com/fbasatemur/design_patterns/blob/main/diagrams/beh_strategy.png)

---

```c++
//
// Created by fbasatemur on 10.11.2021.
//

#include <iostream>

class Strategy{
public:
    virtual int Operation(int num1, int num2) = 0;
};

class OperationAdd: public Strategy{
public:
    int Operation(int num1, int num2) override {
        return num1 + num2;
    }
};

class OperationSubtract: public Strategy{
public:
    int Operation(int num1, int num2) override {
        return num1 - num2;
    }
};

class OperationMultiply: public Strategy{
public:
    int Operation(int num1, int num2) override {
        return num1 * num2;
    }
};

class Context{
    Strategy* _strategy;
public:
    Context(Strategy* strategy){
        _strategy = strategy;
    }
    int ExecuteOperation(int num1, int num2){
        _strategy->Operation(num1, num2);
    }
};


int main(){

    Context* context = new Context(new OperationAdd);
    std::cout << "Add: " << context->ExecuteOperation(3,4) << std::endl;
    delete context;

    context = new Context(new OperationSubtract);
    std::cout << "Sub: " << context->ExecuteOperation(3,4) << std::endl;
    delete context;

    context = new Context(new OperationMultiply);
    std::cout << "Mul: " << context->ExecuteOperation(3,4) << std::endl;
    delete context;

    return 0;
}
```