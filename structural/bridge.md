# Bridge Pattern

Bridge is a structural design pattern that lets you split a large class or a set of closely related classes 
into two separate hierarchies—abstraction and implementation—which can be developed independently of each other.<br>

**Problem**  
In the example, the document should be able to be printed in both Desktop and Web Format types for both Sales and Employee. <br>
For this purpose, instead of defining Desktop and Web classes for each of the Sales and Employee classes, 
Interface for Desktop and Web document is created and the document can be printed in the desired format by connecting with this interface.

---

![bridge](https://github.com/fbasatemur/design_patterns/blob/main/diagrams/str_bridge.png)

---

```c++
//
// Created by fbasatemur on 3.11.2021.
//
/*
 *      Report -------> IReportFormat
 *       /\                 /  \
 *      /  \               /    \
 *   Sales  Employee   Desktop  Web
*/

#include <iostream>
#include <string>

class IReportFormat{
public:
    virtual void Generate(){};
};

class DesktopFormat: public IReportFormat{
public:
    void Generate() override{
        std::cout << "Desktop Format Report" << std::endl;
    }
};

class WebFormat: public IReportFormat{
public:
    void Generate() override{
        std::cout << "Web Format Report" << std::endl;
    }
};

class Report{
public:
    IReportFormat* _iReportFormat = nullptr;
    virtual void Display() = 0;
};

class SalesReport: public Report{
public:
    SalesReport(IReportFormat* iReportFormat){ _iReportFormat = iReportFormat;}
    void Display(){
        std::cout << "Sales Report Display" << std::endl;
        _iReportFormat->Generate();
    }
};

class EmployeeReport: public Report{
public:
    EmployeeReport(IReportFormat* iReportFormat){ _iReportFormat = iReportFormat;}
    void Display(){
        std::cout << "Employee Report Display" << std::endl;
        _iReportFormat->Generate();
    }
};

int main(){

    Report* report1 = new EmployeeReport(new DesktopFormat());
    report1->Display();
    Report* report2 = new SalesReport(new WebFormat());
    report2->Display();

    delete report1, delete report2;
    return 0;
}
```