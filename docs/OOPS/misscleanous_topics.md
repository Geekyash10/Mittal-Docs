# Advanced C++ Memory and Object Management Topics

---

## 10. Virtual Destructors in C++

### What is a Virtual Destructor?

A virtual destructor ensures that when an object is deleted through a base class pointer, the derived class destructor is called first, followed by the base class destructor.

### Why Needed?

In polymorphism, if the destructor is not virtual, only the base class destructor will run — potentially causing resource leaks in the derived class.

### Example Without Virtual Destructor

```cpp
class Base {
public:
    ~Base() { cout << "Base Destructor\n"; }
};

class Derived : public Base {
public:
    ~Derived() { cout << "Derived Destructor\n"; }
};

int main() {
    Base* obj = new Derived();
    delete obj;  // Only Base destructor is called
}
```

### With Virtual Destructor

```cpp
class Base {
public:
    virtual ~Base() { cout << "Base Destructor\n"; }
};
```

### Key Point

- Always declare base class destructors `virtual` if using inheritance and dynamic memory.

---

## 11. Dynamic Memory Management and Rule of 3/5/0

### Overview

C++ allows dynamic memory allocation using `new` and manual deallocation using `delete`. Proper management is crucial in classes that handle resources.

### Rule of 3

If a class uses `new`, it should implement:

1. Destructor
2. Copy Constructor
3. Copy Assignment Operator

### Rule of 5 (C++11+)

Add support for move semantics:

4. Move Constructor
5. Move Assignment Operator

### Rule of 0

If no manual resource management, let the compiler generate all defaults.

### Example of Rule of 3:

```cpp
class MyClass {
    int* data;
public:
    MyClass(int val) { data = new int(val); }
    ~MyClass() { delete data; }
    MyClass(const MyClass& other) {
        data = new int(*other.data);
    }
    MyClass& operator=(const MyClass& other) {
        if (this != &other) {
            delete data;
            data = new int(*other.data);
        }
        return *this;
    }
};
```

---

## 12. Deep Copy vs Shallow Copy

### Shallow Copy

- Copies pointer value.
- Both objects point to the same memory.
- Dangerous: causes double free or data corruption.

### Deep Copy

- Allocates new memory.
- Copies the content.
- Safe: each object owns its own memory.

### Shallow Copy Example

```cpp
class Shallow {
public:
    int* ptr;
    Shallow(int val) { ptr = new int(val); }
    ~Shallow() { delete ptr; }
};

int main() {
    Shallow a(10);
    Shallow b = a;  // default copy → shallow copy
}
```

### Deep Copy Fix

```cpp
Shallow(const Shallow& other) {
    ptr = new int(*other.ptr);  // deep copy
}
```

---

## Tricky Interview Questions

### Virtual Destructors

1. What happens if a base class has a non-virtual destructor?
2. Why should destructors be virtual in inheritance?
3. Can destructors be pure virtual?

### Dynamic Memory Management

4. What is the Rule of 3 and Rule of 5?
5. What is Rule of 0?
6. What happens if destructor is missing in a class with `new`?
7. Why is self-assignment check important in assignment operator?

### Deep vs Shallow Copy

8. What is object slicing?
9. What happens in a shallow copy with raw pointers?
10. How do you implement deep copy in a class?
11. When is shallow copy acceptable?
12. Can the compiler-generated copy constructor cause problems?

---

## Best Practices

- Always define a destructor when using `new`.
- Avoid raw pointers; prefer `std::unique_ptr` or `std::shared_ptr`.
- Follow Rule of 3 or 5 if your class owns resources.
- Prefer deep copies when copying objects with dynamic memory.