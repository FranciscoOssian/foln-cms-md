# Cpp study notes

## refs

- [https://en.wikipedia.org/wiki/Scope_resolution_operator](https://en.wikipedia.org/wiki/Scope_resolution_operator)
- The_C++_Programming_Language_4th_Edition_Bjarne_Stroustrup
- [https://www.w3schools.com/cpp](https://www.w3schools.com/cpp)
- [https://pt.stackoverflow.com/questions/87622/o-que-significa-o-em-c](https://pt.stackoverflow.com/questions/87622/o-que-significa-o-em-c)
- [https://learn.microsoft.com/pt-br/cpp/cpp](https://learn.microsoft.com/pt-br/cpp/cpp)

## A tour of c++: The Basics

ISO C++ defines two kinds of entities
- Standard-library
- Core language and I/O operations

## Scope resolution operator

```c++
class A {
    public:
        static int i; // scope of A
};

namespace B {
    int c = 2;
} // namespace B

int A::i = 4; // scope operator refers to the integer i declared in the class A
int x = B::c; // scope operator refers to the integer c declared in the namespace B
```

## Classes and Objects

```c++
#include <iostream>

class Car {
  public: 
    std::string model;
      float accelerationTimeFrom0To100;
      void showCar();
      Car () {
        std::cout << "Hello World! This is a constructor\n";
      }

    private:
      bool windonIsOpen;
      void openWindow(){
        if(this->windonIsOpen) this->windonIsOpen = false;
        else this->windonIsOpen = true;
      }
      void startEngine();
};

void Car::showCar(){
  std::cout << "showing car\n";
}

int main() {
  Car* car = new Car();
  
  car->showCar();
  std::cout << "Hello World!\n";
}
```

### with "new"

- alocated in heap
- acess with "->"
- need to manage memory alocation

### without "new"

- alocated in stack
- acess with "."
- deleted when escope finish

```c++
void Car::showCar(){
  std::cout << "showing car\n";
}
```

You can define a method after write the class

## Enumerations

```c++
enum class Color { red, blue , green };
enum class Traffic_light { green, yellow, red };
Color col = Color::red;
Traffic_light light = Traffic_light::red;
```

 > You can uso scope resolution operator with Enumerations!

```c++
Traffic_light& operator++(Traffic_light& t)
// prefix increment: ++
{
    switch (t) {
      case Traffic_light::green: return t=Traffic_light::yellow;
      case Traffic_light::yellow: return t=Traffic_light::red;
      case Traffic_light::red: return t=Traffic_light::green;
    }
}
Traffic_light next = ++light; // next becomes Traffic_light::green
```
C++ also offers a less strongly typed ‘‘plain’’ enum (§8.4.2).


## Namespaces

A namespace is a declarative region that provides a scope for identifiers (such as names of types, functions, variables, etc.) within it. Namespaces are used to organize code into logical groups and to avoid name collisions that may occur especially when your codebase includes multiple libraries.

```c++
#include <iostream>

namespace BrazilCars{
  class Car{
    public:
      bool leftHandTraffic: true;
  };
}

namespace JapanCars{
  class Car{
    public:
      bool rightHandTraffic: true;
  };
}


int main() {
  BrazilCars::Car car1;
  JapanCars::Car car2;
}
```