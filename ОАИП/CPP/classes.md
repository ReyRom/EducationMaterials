# Основы объектно-ориентированного программирования в C++

## Введение в ООП

Объектно-ориентированное программирование (ООП) – это парадигма программирования, которая помогает организовывать код так, чтобы его было легче понимать, поддерживать и масштабировать. Основные принципы ООП включают:
1. **Абстракцию**
2. **Инкапсуляцию**
3. **Наследование**
4. **Полиморфизм**

Эти принципы делают код структурированным и облегчают повторное использование его компонентов.

---

## Основные концепции ООП

### 1. Классы и объекты

- **Класс** – это шаблон или чертеж для создания объектов, содержащий атрибуты (состояние) и методы (поведение).
- **Объект** – экземпляр класса, который имеет определенные значения свойств и может вызывать методы, описанные в классе.

**Пример класса:**
```cpp
#include <iostream>
#include <string>

class Car {
public:
    std::string model;
    int year;
    
    void displayInfo() {
        std::cout << "Model: " << model << ", Year: " << year << std::endl;
    }
};
```

**Создание объекта:**
```cpp
int main() {
    Car car1;
    car1.model = "Toyota";
    car1.year = 2020;
    car1.displayInfo();  // Output: Model: Toyota, Year: 2020
    return 0;
}
```

### 2. Инкапсуляция

Инкапсуляция содерожит в себе две концепции: связывание данных и методов для их обработки внутри объекта и ограничение доступа к данным внутри класса, чтобы защитить их от некорректного использования (сокрытие). В C++ для сокрытия используются модификаторы доступа: `private`, `protected`, `public`.

Области видимости для методов и атрибутов помеченных этими модификаторами представлены в таблице.

|             | Класс | Наследник | Вне класса |
|-------------|-------|-----------|------------|
| `private`   |   +   |     -     |      -     |
| `protected` |   +   |     +     |      -     |
| `public`    |   +   |     +     |      +     |

В C++ все методы и атрибуты в **классе**, которые не помечены модификатором доступа считаются `private`

**Пример инкапсуляции:**
```cpp
class BankAccount {
private:
    double balance;

public:
    BankAccount() : balance(0) {}

    void deposit(double amount) {
        if (amount > 0) balance += amount;
    }

    double getBalance() const {
        return balance;
    }
};
```

### 3. Наследование

Наследование позволяет создавать новый класс на основе уже существующего, перенимая его свойства и методы. Это упрощает расширение и изменение классов.

**Пример наследования:**
```cpp
class Animal {
public:
    void eat() {
        std::cout << "Animal eats." << std::endl;
    }
};

class Dog : public Animal {
public:
    void bark() {
        std::cout << "Dog barks." << std::endl;
    }
};

int main() {
    Dog dog;
    dog.eat();   // Output: Animal eats.
    dog.bark();  // Output: Dog barks.
    return 0;
}
```

### 4. Полиморфизм

Полиморфизм позволяет объектам использовать различные формы поведения. В C++ полиморфизм реализуется с помощью виртуальных функций и перегрузки.

- **Компиляционный полиморфизм** (перегрузка методов и операторов)
- **Полиморфизм времени выполнения** (виртуальные методы)

**Пример полиморфизма с перегрузкой:**
```cpp
class Printer {
public:
    // Перегруженный метод для целых чисел
    void print(int value) {
        std::cout << "Integer: " << value << std::endl;
    }

    // Перегруженный метод для чисел с плавающей точкой
    void print(double value) {
        std::cout << "Double: " << value << std::endl;
    }

    // Перегруженный метод для строк
    void print(const std::string& value) {
        std::cout << "String: " << value << std::endl;
    }
};

int main() {
    Printer printer;

    printer.print(42);               // Вызовет print(int)
    printer.print(3.14);             // Вызовет print(double)
    printer.print("Hello, OOP!");    // Вызовет print(const std::string&)

    return 0;
}
```

**Пример полиморфизма с виртуальным методом:**
```cpp
class Animal {
public:
    virtual void sound() {
        std::cout << "Some sound" << std::endl;
    }
};

class Dog : public Animal {
public:
    void sound() override {
        std::cout << "Woof!" << std::endl;
    }
};

class Cat : public Animal {
public:
    void sound() override {
        std::cout << "Meow!" << std::endl;
    }
};

int main() {
    Animal* animal1 = new Dog();
    Animal* animal2 = new Cat();

    animal1->sound();  // Output: Woof!
    animal2->sound();  // Output: Meow!

    delete animal1;
    delete animal2;
    return 0;
}
```