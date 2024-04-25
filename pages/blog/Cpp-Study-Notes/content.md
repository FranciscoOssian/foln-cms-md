# Cpp study notes

All of these notes its just about C++. C features and C programming are not included (for focus in C++ features, style, design).

## Contents

- [Contents](#contents)
- [refs](#refs)
- [A tour of c++: The Basics](#a-tour-of-c-the-basics)
- [Scope resolution operator](#scope-resolution-operator)
- [Class](#class)
- [Enumerations](#enumerations)
- [Namespaces](#namespaces)
- ["Using" Directives](#quotusingquot-directives)
- [handling namespace + .h + .cpp](#handling-namespace-h-cpp)
- [Error Handling](#error-handling)
- [Exceptions](#exceptions)

## refs

- [https://en.wikipedia.org/wiki/Scope_resolution_operator](https://en.wikipedia.org/wiki/Scope_resolution_operator)
- The_C++_Programming_Language_4th_Edition_Bjarne_Stroustrup
- [https://www.w3schools.com/cpp](https://www.w3schools.com/cpp)
- [https://pt.stackoverflow.com/questions/87622/o-que-significa-o-em-c](https://pt.stackoverflow.com/questions/87622/o-que-significa-o-em-c)
- [https://learn.microsoft.com/pt-br/cpp/cpp](https://learn.microsoft.com/pt-br/cpp/cpp)
- [https://cplusplus.com/](https://cplusplus.com/)
- [https://www.geeksforgeeks.org/](https://www.geeksforgeeks.org/)

## A tour of c++: The Basics

ISO C++ defines two kinds of entities

- Standard-library
- Core language and I/O operations

## Scope resolution operator

It is used for the following purposes:

- Access a global variable even with a local variable with the same name.

```c++
int temperature = 35;//ºC
int home(){
  int temperature = 10;//ºC
  std::cout << ::temperature; // output 35
  std::cout << temperature;   // output 10
}
```

- To define a function outside a class

```c++
class Dog{
  public:
    int age;
    void bark();
};

void Dog::bark() { // outside of main function
  std::cout << "wolf wolf wolf wolf";
};
```

- To access a class's static variables or namespace variables

```c++
class Bottle{
  public:
    static int max_weight;
};
namespace Bag{
  Brush my_Brush;
  int weight;
}
  
int Bottle::max_weight = 5;//L
int main() {
  Bag::weight = 3;//kg
  std::cout << Bag::weight << '\n' << Bottle::max_weight << '\n';
  return 0;
}

// output "3 \n 5 \n"
```

## Class

> "The central language feature of C++ is the class" 
>
> <cite>Stroustrup, Bjarne. The C++ Programming Language. 4th ed., Addison-Wesley, 2013.</cite>


```c++
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

### Initializer List

```c++
class Point {
  private:
    int x;
    int y;
 
  public:
    Point(int i = 0, int j = 0): x(i), y(j) {}
    /*  can also be written as:
        Point(int i = 0, int j = 0) {
            this->x = i;
            this->y = j;
        }
    */
    int getX() const { return x; }
    int getY() const { return y; }
};
```

### Type of Classes

#### Concrete classes

It's a class that can be initiate directly. They usually contains complete implementations for all methods. In other words, a concrete class provides implementations for all the methods it declares.

#### Abstract classes

An abstract class is a class that contains purely virtual methods. This means that the class does not provide implementations for at least one of its methods, making it unable to be instantiated directly. Abstract classes serve as models for other classes that must provide implementations for virtual methods defined in the abstract classe.

```c++
class Shape {
  public:
    virtual void draw();
};

class Circle : public Shape {
  public:
    void draw() override {
      std::cout << "Drawing a circle\n";
    }
};

int main() {
    Circle circle;
    circle.draw();
    return 0;
}
```

#### Classes in class hierarchies

...

### constructor and destructor

```c++
class Box {
  
  private:
    int width;
    int height;
    int length;
  
  public:
    Box() {} // constructor
      
    Box(int i){  // constructor
      this->width = i;
      this->height = i;
      this->length = i;
    }

    Box(int width, int height, int length){  // constructor
      this->width = width;
      this->height = height;
      this->length = length;
    }

    ~Box() {  // destructor
      // If any resource has been dynamically allocated, it can be deallocated here
    }

    int Volume() { return this->width * this->height * this->lenght; }
};
```

### Invariants

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

- [x] can be nested

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
  class Foo{
    public:
      int bee: 0;
  }
}


int main() {
  BrazilCars::Car car1;
  JapanCars::Car car2;
}
```

### inline namespace

`inline` is a namespace keyword that as if they are changing the scope of your childrens.

```c++
namespace Student{
  int age;
  inline namespace Book{
    void readBook(){
      std::cout << "Reading";
    }
  }
}
Student::readBook();
```

## "Using" Directives

Enables names within a namespace to be used without the namespace-name as an explicit qualifier.

```c++
using namespace JapanCars;

int main(){
  Car myCar;
}
```

You can also define to use just a value of this namespace

```c++
using JapaCars::Car;

int main(){
  Car myAnotherCar;
}
```

## handling namespace + .h + .cpp

Its like definition of structure, you can define the implementation of a method, fuction, ... inside a .h (inside of a definition of structure, class or namespace), but isn't a better pratice. If you will define your namespace (or class, struct, whatever) in a .h, the best way is define this methods in .c file. With this, you will use `::` operator (Scope resolution operator)

```c++
// main.c
#include <iostream>
#include "./FooBar.hpp"

int main() {
  std::cout << "Hello World!\n";
  std::cout << FooBar::Bar() << std::endl;
}
```

```c++
// FooBar.hpp
#pragma once
namespace FooBar
{
    void Foo();
    int Bar();
}
```

```c++
// FooBar.cpp
#include "./FooBar.hpp"

int FooBar::Bar(){
  return 0;
}
```

## Error Handling

### Exceptions

- throw
- try-catch
