# Iterator Pattern

According to GoF definition, an iterator pattern provides a way to access the elements of an aggregate object (List, ArrayList etc.) sequentially without 
exposing its underlying representation. It is behavioral design pattern.<br>


---

![iterator](https://github.com/fbasatemur/design_patterns/blob/main/diagrams/beh_iterator.png)

---

```c++
//
// Created by fbasatemur on 10.11.2021.
//

#include <iostream>
#include <string.h>

class Person
{
    std::string name;
    int age;

public:
    Person* next = nullptr;
    Person(std::string name, int age): name(name),age(age){};
    friend std::ostream& operator<<(std::ostream& os, Person* obj)
    {
        return os << obj->name << " " << obj->age;
    }
};

class IQueue{
public:
    virtual Add(std::string name, int age) = 0;
    virtual Remove() = 0;
    virtual Person* GetHead() = 0;
};

class Queue: public IQueue{
private:
    Person* _head = nullptr;
    Person* _current = nullptr;

public:
    Add(std::string name, int age) override {
        if(_head == nullptr)
        {
            _current = new Person(name, age);
            _head = _current;
        }
        else {
            _current->next = new Person(name, age);
            _current = _current->next;
        }
    };

    Remove() override {
        if(_head != nullptr){
            Person* tempHead = _head;
            _head = _head->next;
            delete tempHead;
        }
        else
            std::cout << "All of deleted" << std::endl;
    };

    Person* GetHead() override{
        return _head;
    }
};

class PersonIterator: public Person{
private:
    static Person* _iter;
    static IQueue* _queue;

public:
    static Person* GetIter(IQueue* queue){
        _queue = queue;
        _iter = queue->GetHead();
        return _iter;
    }
    static Person* NextIter() {
        _iter = _iter->next;
        return _iter;
    }
    static Person* Remove(){
        _queue->Remove();
        return _queue->GetHead();
    }
};

Person* PersonIterator::_iter = nullptr;
IQueue* PersonIterator::_queue = nullptr;

int main(){

    IQueue* queue = new Queue;
    queue->Add("john", 12);
    queue->Add("curl", 15);
    queue->Add("tommy", 20);

    Person* iter = PersonIterator::GetIter(queue);
    while (iter != nullptr) {
        std::cout << iter << std::endl;
        iter = PersonIterator::NextIter();
    }

    iter = PersonIterator::GetIter(queue);
    while (iter != nullptr) {
        std::cout << iter << " is deleting" << std::endl;
        iter = PersonIterator::Remove();
    }

    delete iter, delete queue;
    return 0;
}
```