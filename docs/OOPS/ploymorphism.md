# Polymorphism in C++

## What is Polymorphism?
Polymorphism means "many forms". In C++, it refers to the ability of a function or object to behave differently based on the context. It is a key concept in Object-Oriented Programming.

## Types of Polymorphism

### 1. Compile-Time Polymorphism (Static Binding)
Resolved during compilation.

#### a) Function Overloading
Same function name with different parameter lists.

```cpp
class Print {
public:
    void show(int x);
    void show(double y);
    void show(string s);
};
```

#### b) Operator Overloading
Redefining operators for user-defined types.

```cpp
class Complex {
public:
    int real, imag;
    Complex operator + (const Complex& obj);
};
```

### 2. Run-Time Polymorphism (Dynamic Binding)
Resolved during runtime using virtual functions.

#### a) Function Overriding
Derived class provides its own version of a function.

#### b) Virtual Functions
Declared in base class using `virtual` keyword. Enables late binding.

```cpp
class Animal {
public:
    virtual void speak();
};
class Dog : public Animal {
public:
    void speak() override;
};
```

#### c) Base Class Pointer to Derived Object
Allows runtime decision of which function to call.

## Virtual Functions

### Definition
A virtual function is a base class function marked with the `virtual` keyword and meant to be overridden in derived classes. Enables runtime polymorphism.

### Example

```cpp
class Base {
public:
    virtual void display();
};
class Derived : public Base {
public:
    void display() override;
};
```

## Pure Virtual Functions

### Definition
A pure virtual function is a virtual function with no definition in the base class. Syntax ends with `= 0`. It makes the class abstract.

### Syntax

```cpp
class Shape {
public:
    virtual void draw() = 0;
};
```

### Example

```cpp
class Shape {
public:
    virtual void draw() = 0;
};
class Circle : public Shape {
public:
    void draw() override;
};
```

## Difference: Virtual vs Pure Virtual

| Feature | Virtual Function | Pure Virtual Function |
|--------|------------------|------------------------|
| Has Body | Yes | No (can be defined outside) |
| Overriding | Optional | Mandatory |
| Object Creation | Allowed | Not allowed (abstract class) |
| Purpose | Runtime polymorphism | Interface enforcement |

## Object Slicing
When a derived object is assigned to a base object, only the base part is copied.

```cpp
Derived d;
Base b = d; // Slices off derived part
```

## Summary
- Compile-time polymorphism: Overloading
- Runtime polymorphism: Overriding, virtual functions
- Virtual functions: Late binding
- Pure virtual functions: Abstract class, must be overridden