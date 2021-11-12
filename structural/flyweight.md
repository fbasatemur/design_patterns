# Flyweight Pattern

Flyweight design pattern is in the category of structural design pattern.
Its purpose is to reduce memory usage by using common memory areas instead of creating similar objects over and over.
In other words, it is to minimize the memory usage so that it does not take up too much space in the memory by grouping the same objects repeatedly.<br>

The flyweight design pattern is applied in 3 steps:
- Fields that may be different in each instance should be reserved. (Extensic state) Create() is an extensic state at below example.
- The reserved fields must be sent to the relevant method as a parameter over the interface.
- Factory class must be defined to create an object.

**Problem**  
In our example we will use the tetris game, shapes will be I, J and T;
- Extensic state fields in these objects; color and speed. These fields are sent as parameters to the Create method in the Piece interface.
- Next we will create a factory class to create the objects.
- Factory sınıfında, If the related object has been created before, this object is returned. 
If the object has not been created before, then it is created with the new keyword. In both cases, the fields of the object, 
which we call extensic state, are filled according to the properties of the new object to be created.

---

![flyweight](https://github.com/fbasatemur/design_patterns/blob/main/diagrams/str_flyweight.png)

---

```c++
#include <iostream>
#include <map>
#include <string>

class Piece {
public:
    virtual void Create(std::string color, int speed) = 0;
};

class I : public Piece {
private: std::string label;
public:
    I() {
        label = "I";
    }
    void Create(std::string color, int speed) override{
        std::cout << "color: " << color << " speed: " << speed << std::endl;
    }
};

class T : public Piece {
private: std::string label;
public:
    T() {
        label = "T";
    }
    void Create(std::string color, int speed) override {
        std::cout << "color: " << color << " speed: " << speed << std::endl;
    }
};

class J : public Piece {
private: std::string label;
public:
    J() {
        label = "J";
    }
    void Create(std::string color, int speed) override {
        std::cout << "color: " << color << " speed: " << speed << std::endl;
    }
};

class PieceFactory
{
private:
    std::map <std::string, Piece*> pieceMap;
public:
    Piece* GetPiece(std::string pieceType) {
        Piece* piece;
        auto search = pieceMap.find(pieceType);
        if (search != pieceMap.end()) {
            piece = search->second;
        }
        else {
            if (pieceType == "I") {
                piece = new I();
            }
            else if (pieceType == "T") {
                piece = new T();
            }
            else if (pieceType == "J") {
                piece = new J();
            }
            else {
                return nullptr;
            }
            pieceMap[pieceType] = piece;
        }
        return piece;
    }
};

int main()
{
    PieceFactory pf;
    Piece* piece = nullptr;

    for (int i = 0; i < 20; ++i) {
        int in = i % 3;
        if (in == 0) {
            piece = pf.GetPiece("I");
            piece->Create("red", i * 2);
        }
        else if (in == 1) {
            piece = pf.GetPiece("T");
            piece->Create("green", i * 2);
        }
        else { // in == 2
            piece = pf.GetPiece("J");
            piece->Create("blue", i * 2);
        }
    }

    return 0;
}
```