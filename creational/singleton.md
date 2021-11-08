# Singleton Pattern

Singleton design pattern is in the category of creational design pattern.

The purpose is to ensure that only one instance of a class is created. In other words, when an instance is wanted to be created from any class, it is created if there is no instance created before. 
If it has been created before, the existing instance is used. Thus, the number of objects can be controlled and limited to 1 only object.

---

![singleton](https://github.com/fbasatemur/design_patterns/blob/main/diagrams/cre_singleton.png)

---

```c++

class Singleton{
private:
    static Singleton* _instance;

public:
    static Singleton* GetInstance();
};

Singleton* Singleton::_instance = 0;

Singleton *Singleton::GetInstance() {
    if(_instance == 0) _instance = new Singleton();
    return _instance;
}

int main(){

    auto * instance = Singleton::GetInstance();
    delete instance;
    return 0;
}

```