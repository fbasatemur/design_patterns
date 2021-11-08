# Factory Pattern

Factory design pattern is in the category of creational design pattern.

When producing objects from multiple relational classes, instead of producing an object from the object class itself, 
the Factory Pattern ensures that the required object is produced over a single instance.
Object is created using factory class. The object to be produced is connected via the interface.

---

![factory](https://github.com/fbasatemur/design_patterns/blob/main/diagrams/cre_factory.png)

---

```c++

//
// Created by fbasatemur on 1.11.2021.
//

#include <iostream>

class Shape{
public:
    virtual void Draw(){};
};

class Triangle: public Shape{
public:
    void Draw() override{
        std::cout<< "Triangle drawn" <<std::endl;
    }
};

class Rectangle: public Shape{
public:
    void Draw() override{
        std::cout<< "Rectangle drawn" <<std::endl;
    }
};

class Circle: public Shape{
public:
    void Draw() override{
        std::cout<< "Circle drawn" <<std::endl;
    }
};

class ShapeFactory{
public:
    enum shapeTypes{ triangle, rectangle, circle };

    static Shape* GetShape(shapeTypes types){
        switch (types) {
            case triangle:
                return new Triangle;
                break;
            case rectangle:
                return new Rectangle;
                break;
            case circle:
                return new Circle;
                break;
            default:
                return nullptr;
        }
    }
};

int main(){
    Shape* triangle = ShapeFactory::GetShape(ShapeFactory::shapeTypes::triangle);
    triangle->Draw();
    Shape* rectangle = ShapeFactory::GetShape(ShapeFactory::shapeTypes::rectangle);
    rectangle->Draw();
    Shape* circle = ShapeFactory::GetShape(ShapeFactory::shapeTypes::circle);
    circle->Draw();

    delete triangle, delete rectangle, delete circle;
    return 0;
}

```