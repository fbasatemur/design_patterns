# State Pattern

State is a behavioral design pattern that lets an object alter its behavior when its internal state changes. <br>

The state pattern is used if you want an object with states to behave differently when its state changes.
We can give many examples, the case of not receiving another card when we insert our card into the ATM, 
the cases of turning it off if it is on by pressing the same button on the television remote, turning it on if it is closed, etc. <br>

It is possible to control a situation with an if-else or switch structure, but using if-else or switch for each behavior will create confusion.
Also, when we add a new behavior, we will need to update almost the entire program.
But updates can be performed more easily if the state pattern is used.<br>

**Problem**
The behavior of the lamp is applied in the example:
- The lamp has two states. These are Lamp on state and off state.
- When we press the on/off button on the remote of the lamp, we expect the behavior of closing if the lamp is on and opening if it is closed.  


---

![state](https://github.com/fbasatemur/design_patterns/blob/main/diagrams/beh_state.png)

---

```c++
//
// Created by fbasatemur on 5.11.2021.
//

#include <iostream>
#include <typeinfo>

class LampState {
public:
    virtual void Open() = 0;

    virtual void Close() = 0;
};

class OpenState : public LampState {
public:
    void Open() { std::cout << "Lamp is already on!" << std::endl; }

    void Close() { std::cout << "Lamp state: closing.." << std::endl; }
};

class CloseState : public LampState {
public:
    void Open() { std::cout << "Lamp state: opening.." << std::endl; }

    void Close() { std::cout << "Lamp is already off!" << std::endl; }
};

class Machine {
private:
    LampState *_lampState = nullptr;
public:
    void SetLampState(LampState *lampState) { this->_lampState = lampState; }

    Machine() {
        SetLampState(new CloseState);
    }

    void OnOpen() {
        _lampState->Open();
        if (typeid(CloseState) == typeid(*_lampState)) {
            delete _lampState;
            SetLampState(new OpenState);
            std::cout << "Lamp state: on" << std::endl;
        }
    }

    void OnClose() {
        _lampState->Close();
        if (typeid(OpenState) == typeid(*_lampState)) {
            delete _lampState;
            SetLampState(new CloseState);
            std::cout << "Lamp state: off" << std::endl;
        }
    }
};

int main() {

    Machine *state = new Machine();
    // lamp is Close when it's initialize
    state->OnClose();
    state->OnOpen();
    state->OnOpen();
    state->OnClose();

    return 0;
}
```