# Builder Pattern

Object-oriented programming is based on classes. We create objects from classes. To do this, we use constructors. 
If the number of fields in our class is large, we may need more than one constructor. Therefore, we may feel the need to add a new constructor every time a field is added. 
The Builder Pattern provides a nice solution to get rid of this lengthy number of parameters and complex constructors.<br>

Problem: Let's define a class for human attributes (name, surname, gender, age). When we want to create an object from this class, we may want to use the constructer method. In this case, we may need constructors such as:
- Person(name, age)
- Person(name, surname)
- Person(name, surname, gender, age)

Since we want to make some parameters undefined, the number of constructors and complexity will increase. The builder pattern can be used to solve this problem.
it is builds a complex object using simple objects and using a step by step approach, so it is in the category of creational design pattern. <br>

---

![abstract_factory](https://github.com/fbasatemur/design_patterns/blob/main/diagrams/cre_builder.png)

---

```c++
//
// Created by fbasatemur on 1.11.2021.
//

#include <iostream>
#include <string>
using namespace std;

class Person
{
    string _name, _surname, _gender;
    unsigned int _age;

public:
    Person(string name, string surname, string gender, unsigned int age):_name(name), _surname(surname), _age(age), _gender(gender){}
    friend ostream& operator<<(ostream& os, const Person* obj)
    {
        return os << obj->_name << std::endl
                  << obj->_surname << std::endl
                  << obj->_gender << std::endl
                  << obj->_age << std::endl;
    }

    class PersonBuilder{
        string _name = "none", _surname = "none", _gender = "none";
        unsigned int _age = 0;
    public:
        PersonBuilder(string name):_name(name){};
        PersonBuilder *Surname(string surname){this->_surname = surname; return this;}
        PersonBuilder *Gender(string gender){this->_gender = gender; return this;}
        PersonBuilder *Age(unsigned int age){this->_age = age; return this;}
        Person* Build(){return new Person(_name, _surname, _gender, _age);}
    };

    static PersonBuilder* Create(string name){return new PersonBuilder(name);}
};


int main(){

    Person *p1 = Person::Create("erich")->Surname("gamma")->Gender("m")->Age(60)->Build();
    cout << p1 << endl;
    Person *p2 = Person::Create("ralph")->Age(66)->Build();
    cout << p2 << endl;
    delete p1, delete p2;
    return 0;
}
```