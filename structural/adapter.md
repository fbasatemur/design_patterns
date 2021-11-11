# Adapter Pattern

Adapter pattern is a structural design pattern (also known as wrapper) that allows the interface of an existing class to be used as another interface. 
Thus, we do not have to edit the code that has been written before. <br>

For example,
- Crypto interface is currently used for encryption operations.
- CryptA and CryptB are derived from the Crypto interface. There is no problem in the operation of these classes.
- However, if Codex class is desired to be used, adapter class should be created for this class, since the methods of this class are different from Crypto interface.<br>

---

![adapter](https://github.com/fbasatemur/design_patterns/blob/main/diagrams/str_adapter.png)

---

```c++
//
// Created by fbasatemur on 2.11.2021.
//

#include <iostream>
#include <string>

class Crypt {
public:
    virtual void Encrypt(std::string text){};
    virtual void Decrypt(std::string text){};
};

class CryptA : public Crypt {
public:
    void Encrypt(std::string text) override{
        std::cout << "#CryptA_Encrypt" << text << std::endl;
    }
    void Decrypt(std::string text) override{
        std::cout << "#CryptA_Decrypt" << text << std::endl;
    }
};

class CryptB : public Crypt {
public:
    void Encrypt(std::string text) override{
        std::cout << "#CryptB_Encrypt" << text << std::endl;
    }
    void Decrypt(std::string text) override{
        std::cout << "#CryptB_Decrypt" << text << std::endl;
    }
};

class Codex{
public:
    void Text2Code(std::string text) {
        std::cout << "#Codex_Text2Code" << text << std::endl;
    }
    void Code2Text(std::string text) {
        std::cout << "#Codex_Code2Text" << text << std::endl;
    }
};

class CodexAdapter: public Crypt{
private: Codex codex;
public:
    CodexAdapter(const Codex& codex){
        this->codex = codex;
    }
    void Encrypt(std::string text) override{
        codex.Text2Code(text);
    }
    void Decrypt(std::string text) override{
        codex.Code2Text(text);
    }
};

int main(){
    Crypt *crypt = new CryptA();
    crypt->Encrypt("_EnA");
    crypt->Decrypt("_DeA");
    delete crypt;

    crypt = new CryptB();
    crypt->Encrypt("_EnB");
    crypt->Decrypt("_DeB");
    delete crypt;

    Codex codex;
    crypt = new CodexAdapter(codex);
    crypt->Encrypt("_AdapterEn");
    crypt->Decrypt("_AdapterDe");

    delete crypt;
    return 0;
}
```