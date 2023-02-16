# Prototype Pattern

Prototype is a creational design pattern that lets you copy existing objects without making your code dependent on their classes.
It is a design pattern that allows to create an object clone when we need to copy an object more than once (deep copy).<br>

---

![abstract_factory](https://github.com/fbasatemur/design_patterns/blob/main/diagrams/cre_prototype.png)

---

```c++
//
// Created by fbasatemur on 3.11.2021.
//

#include <iostream>
#include <string>

#define PI 3.1415F


class IShape {
public:
    virtual float Area() = 0;
    virtual IShape *Clone() = 0;
};

class Square : public IShape {
private:
    unsigned int _sideLength;
public:
    Square(const unsigned int &sideLength) : _sideLength(sideLength) {}

    unsigned int GetSideLength() { return _sideLength; }

    float Area() override { return _sideLength * _sideLength; }

    IShape *Clone() override {
        return new Square(this->GetSideLength());
    }
};

class Circle : public IShape {
private:
    unsigned int _radius;
public:
    Circle(const unsigned int &radius) : _radius(radius) {}

    unsigned int GetRadius() { return _radius; }

    float Area() override { return PI * float(_radius * _radius); }

    IShape *Clone() override {
        return new Circle(this->GetRadius());
    }
};

class ShapeFactory {
public:
    static IShape *_square;
    static IShape *_circle;

    static void Initialize() {
        _square = new Square(3);
        _circle = new Circle(2);
    }

    static IShape *GetSquare() {
        return _square->Clone();
    }

    static IShape *GetCircle() {
        return _circle->Clone();
    }
};

IShape *ShapeFactory::_circle = nullptr;
IShape *ShapeFactory::_square = nullptr;

int main() {
    ShapeFactory::Initialize();
    IShape *object;

    // All the object were created by cloning the prototypes.
    object = ShapeFactory::GetSquare();
    std::cout << "Square: " << object->Area() << std::endl;

    object = ShapeFactory::GetCircle();
    std::cout << "Circle: " << object->Area() << std::endl;

    delete object;
    return 0;
}
```