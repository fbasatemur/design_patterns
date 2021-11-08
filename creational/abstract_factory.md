# Abstract Factory Pattern

Abstract factory design pattern, also called as factory of factories, comes 
under creational pattern as this pattern provides one of the best ways to create an object.<br>

Abstract factory pattern, unlike factory method pattern, generates objects using multiple factory classes.
It isolates the client code from implementation classes and eases the exchanging of object families.<br>

Usage of Abstract Factory Pattern
- When the system needs to be independent of how its object are created, composed, and represented.
- When the family of related objects has to be used together, then this constraint needs to be enforced.
- When you want to provide a library of objects that does not show implementations and only reveals interfaces.
- When the system needs to be configured with one of a multiple family of objects.

Problem: Let's create objects of different shapes (triangle, rectangle) and colors (red, green, blue). 
Any shape can be a any color. For example red->triangle, green->rectangle, blue->triangle etc.<br>

In this case,
1. Interfaces must be produced for shapes and colors. -> IShape, IColor
2. A Factory should build for each interface -> ShapeFactory, ColorFactory
3. A common interface should be created to interact with these factories and factory production should be carried out using this interface. -> IFactory, FactoryProducer

---

![abstract_factory](https://github.com/fbasatemur/design_patterns/blob/main/diagrams/cre_abstract_factory.png)

---

```c++
//
// Created by fbasatemur on 6.11.2021.
//

#include <iostream>
#include <string>

// IShape is interface for rectangle and triangle
class IShape {
public:
    virtual void Draw() = 0;
};

class Rectangle : public IShape {
public:
    void Draw() override {
        std::cout << "Rectangle drawn" << std::endl;
    }
};

class Triangle : public IShape {
public:
    void Draw() override {
        std::cout << "Triangle drawn" << std::endl;
    }
};

// IColor is interface for red, green and blue
class IColor {
public:
    virtual void Fill() = 0;
};

class Red :public IColor {
public:
    void Fill() override {
        std::cout << "Red filled" << std::endl;
    }
};

class Green :public IColor {
public:
    void Fill() override {
        std::cout << "Green filled" << std::endl;
    }
};

class Blue :public IColor {
public:
    void Fill() override {
        std::cout << "Blue filled" << std::endl;
    }
};

// IFactory is interface for ShapeFactory and ColorFactory 
// Color or shape production will be done using these getters.
class IFactory {
public:
    virtual IColor *GetColor(const std::string color) = 0;
    virtual IShape *GetShape(const std::string shape) = 0;
};

class ShapeFactory : public IFactory {
public:
    IColor *GetColor(const std::string color) override {
        return nullptr;
    }

    IShape *GetShape(const std::string shape) override {
        if (shape == "rectangle")
            return new Rectangle;

        else if (shape == "triangle")
            return new Triangle;

        return nullptr;
    }
};

class ColorFactory : public IFactory {
public:
    IColor *GetColor(const std::string color) override {
        if (color == "red")
            return new Red;

        else if (color == "green")
            return new Green;

        else if (color == "blue")
            return new Blue;

        return nullptr;
    }

    IShape *GetShape(const std::string shape) override {
        return nullptr;
    }
};

// FactoryProducer is used to produce color or shape factories.
class FactoryProducer {
public:
    static IFactory *GetFactory(std::string choice) {
        if (choice == "color")
            return new ColorFactory;

        else if (choice == "shape")
            return new ShapeFactory;

        return nullptr;
    }
};

int main() {
    IFactory *shapeFactory = FactoryProducer::GetFactory("shape");
    IShape *shape = shapeFactory->GetShape("triangle");
    shape->Draw();
    IFactory *colorFactory = FactoryProducer::GetFactory("color");
    IColor *color = colorFactory->GetColor("blue");
    color->Fill();

    delete shape, delete color, delete colorFactory, delete shapeFactory;
    return 0;
}
```