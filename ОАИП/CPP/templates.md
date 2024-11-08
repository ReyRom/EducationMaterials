В C++ шаблоны позволяют создавать функции и классы, которые могут работать с разными типами данных, сохраняя при этом код простым и понятным. Шаблоны предоставляют мощные инструменты для создания обобщенного кода, который может работать с разными типами данных и параметрами, что значительно улучшает переиспользуемость и поддерживаемость кода.

### 1. Шаблон функции
Шаблоны функций позволяют создавать функции, которые могут работать с любыми типами данных. Вот пример простой шаблонной функции для нахождения максимального значения из двух значений:

```cpp
#include <iostream>
template <typename T>
T getMin(T a, T b) {
    return (a < b) ? a : b;
}

int main() {
    std::cout << getMin(10, 20) << std::endl;       // для int
    std::cout << getMin(10.5, 20.3) << std::endl;   // для double
    std::cout << getMin('a', 'z') << std::endl;     // для char
    return 0;
}
```

Здесь `T` — это тип-параметр, который можно использовать внутри функции `getMin`. При вызове функции компилятор автоматически определяет тип на основе переданных аргументов.

### 2. Шаблон класса
Шаблоны классов позволяют создавать классы, которые могут работать с разными типами данных. Например, можно создать шаблонный класс для хранения пары значений:

```cpp
#include <iostream>
template <typename T, typename U>
class Pair {
public:
    T first;
    U second;

    Pair(T a, U b) : first(a), second(b) {}

    void display() {
        std::cout << "First: " << first << ", Second: " << second << std::endl;
    }
};

int main() {
    Pair<int, double> p1(1, 2.5);
    p1.display();

    Pair<std::string, char> p2("Hello", 'A');
    p2.display();

    return 0;
}
```

В этом примере шаблонный класс `Pair` может хранить пару значений разных типов (`T` и `U`), что позволяет его использовать для разных комбинаций типов.

### 3. Шаблоны с несколькими параметрами
Шаблоны могут принимать несколько типов данных. Например, в классе `Pair` выше было использовано сразу два типа (`T` и `U`). Можно также использовать шаблоны функций с несколькими параметрами:

```cpp
#include <iostream>
template <typename T, typename U>
void printPair(T a, U b) {
    std::cout << "First: " << a << ", Second: " << b << std::endl;
}

int main() {
    printPair(10, 20.5);           // int и double
    printPair("Hello", 'A');       // string и char
    return 0;
}
```

### 4. Специализация шаблонов
Иногда требуется написать отдельную реализацию шаблона для определенного типа данных. Например:

```cpp
#include <iostream>
template <typename T>
class Calculator {
public:
    static T add(T a, T b) {
        return a + b;
    }
};

// Специализация для типа `std::string`
template <>
class Calculator<std::string> {
public:
    static std::string add(std::string a, std::string b) {
        return a + " " + b;
    }
};

int main() {
    std::cout << Calculator<int>::add(5, 10) << std::endl;          // 15
    std::cout << Calculator<std::string>::add("Hello", "World") << std::endl;  // Hello World
    return 0;
}
```

Здесь класс `Calculator` специально реализован для типа `std::string`, чтобы строки объединялись с пробелом между ними.

### 5. Шаблоны с параметрами-значениями
C++ также позволяет передавать константы как параметры шаблонов. Например:

```cpp
#include <iostream>
template <typename T, int size>
class Array {
private:
    T arr[size];
public:
    void fill(T value) {
        for (int i = 0; i < size; i++) {
            arr[i] = value;
        }
    }
    void display() const {
        for (int i = 0; i < size; i++) {
            std::cout << arr[i] << " ";
        }
        std::cout << std::endl;
    }
};

int main() {
    Array<int, 5> intArray;
    intArray.fill(3);
    intArray.display();

    Array<double, 3> doubleArray;
    doubleArray.fill(4.5);
    doubleArray.display();

    return 0;
}
```

Здесь массив `Array` имеет размер, определяемый параметром шаблона `size`, что делает его гибким для работы с разными размерами.

