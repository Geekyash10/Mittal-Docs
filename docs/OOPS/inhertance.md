
# C++ Inheritance – Concepts, Diagrams, and Interview-Style Questions

## What is Inheritance?

Inheritance is an object-oriented programming feature that allows one class (derived class) to inherit properties and behavior (methods and members) from another class (base class).

### Types of Inheritance in C++

| Type               | Description                                                    |
|--------------------|----------------------------------------------------------------|
| Single Inheritance | One derived class inherits from one base class                 |
| Multiple Inheritance | One derived class inherits from more than one base class     |
| Multilevel Inheritance | A derived class becomes base for another class             |
| Hierarchical Inheritance | Multiple classes inherit from a single base class        |
| Hybrid Inheritance | Combination of two or more types of inheritance                |
| Virtual Inheritance | Solves ambiguity in diamond problem using `virtual` keyword   |

---

## Output-Based / Debug-the-Code Practice Questions

### Q1. Virtual Base Constructor Order

```cpp
class A {
public:
    A() { cout << "A "; }
};

class B : virtual public A {
public:
    B() { cout << "B "; }
};

class C : virtual public A {
public:
    C() { cout << "C "; }
};

class D : public B, public C {
public:
    D() { cout << "D "; }
};

int main() {
    D obj;
}
```

Answer: A B C D

---

### Q2. Function Overriding + Pointer to Base

```cpp
class Base {
public:
    void show() { cout << "Base "; }
};

class Derived : public Base {
public:
    void show() { cout << "Derived "; }
};

int main() {
    Base* ptr = new Derived();
    ptr->show();
}
```

Answer: Base

---

### Q3. Now with Virtual Function

```cpp
class Base {
public:
    virtual void show() { cout << "Base "; }
};

class Derived : public Base {
public:
    void show() override { cout << "Derived "; }
};

int main() {
    Base* ptr = new Derived();
    ptr->show();
}
```

Output: Derived

---

### Q4. Diamond Ambiguity Without Virtual Base

```cpp
class A {
public:
    void greet() { cout << "Hello from A "; }
};

class B : public A {};
class C : public A {};
class D : public B, public C {};

int main() {
    D obj;
    obj.greet();  // Ambiguous
}
```

Error: Ambiguous call to `greet()`

Fix:

```cpp
obj.B::greet();  // or obj.C::greet();
```

Or use:

```cpp
class B : virtual public A {};
class C : virtual public A {};
```

---

### Q5. Function Hiding in Inheritance

```cpp
class Base {
public:
    void greet() { cout << "Base "; }
};

class Derived : public Base {
public:
    void greet(int x) { cout << "Derived " << x; }
};

int main() {
    Derived d;
    d.greet();  // Error
}
```

Error: No matching function — `Base::greet()` is hidden

Fix:

```cpp
using Base::greet;
```

---

### Q6. Destructor Not Virtual

```cpp
class Base {
public:
    ~Base() { cout << "~Base "; }
};

class Derived : public Base {
public:
    ~Derived() { cout << "~Derived "; }
};

int main() {
    Base* ptr = new Derived();
    delete ptr;
}
```

Output: ~Base  
Fix:

```cpp
virtual ~Base() { ... }
```

---

### Q7. Inheritance + Access Specifier

```cpp
class A {
protected:
    int val = 42;
};

class B : private A {
public:
    void show() { cout << val; }
};

int main() {
    B b;
    b.show();      // OK
    // cout << b.val;  // Error
}
```

Output: 42

---

## Interview MCQs on Inheritance

Q1. What does virtual inheritance solve in C++?  
Answer: D) Diamond problem

Q2. What is object slicing?  
Answer: C) Losing derived part in base object copy

Q3. Which inheritance hides public and protected members of base as private?  
Answer: C) Private

Q4. Which of the following causes ambiguity in inheritance?  
Answer: C) Diamond problem without virtual inheritance

Q5. What happens if base class destructor is not virtual, and derived object is deleted via base pointer?  
Answer: B) Only base destructor is called

