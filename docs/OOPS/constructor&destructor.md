
# C++ Constructors and Destructors -- Interview Notes & Practice

##  What is a Constructor?

- A **constructor** is a special function automatically called when an object is created.
- It **initializes** object members.
- Has the **same name** as the class and **no return type**.

### Types of Constructors:

1. **Default Constructor:** No arguments.
2. **Parameterized Constructor:** Takes arguments.
3. **Copy Constructor:** Initializes an object using another object.

```cpp
class Demo {
public:
    Demo() { }                      // Default
    Demo(int x) { }                 // Parameterized
    Demo(const Demo& d) { }         // Copy
};
```

---

##  What is a Destructor?

- A **destructor** is a special function called when an object goes **out of scope** or is explicitly **deleted**.
- Cleans up resources (memory, files, etc.)
- Has the same name as the class, prefixed with `~`, and **no arguments or return type**.

```cpp
~Demo() {
    // cleanup code
}
```

---

##  Default, Parameterized, and Copy Constructors

```cpp
class A {
public:
    A() { cout << "Default "; }
    A(int x) { cout << "Param "; }
    A(const A& other) { cout << "Copy "; }
};

int main() {
    A a;
    A b(10);
    A c = b;
}
```

> **Output:** `Default Param Copy`

---

##  Destructor Basics

```cpp
class A {
public:
    A() { cout << "C "; }
    ~A() { cout << "D "; }
};

int main() {
    A obj;
}
```

> **Output:** `C D`

---

##  Constructor Overloading

```cpp
class Over {
public:
    Over() { cout << "No Arg "; }
    Over(int x) { cout << "Int Arg "; }
    Over(double y) { cout << "Double Arg "; }
};

int main() {
    Over a;
    Over b(5);
    Over c(2.5);
}
```

> **Output:** `No Arg Int Arg Double Arg`

---

##  Static vs Dynamic Objects

```cpp
class Test {
public:
    Test() { cout << "C "; }
    ~Test() { cout << "D "; }
};

int main() {
    static Test a;
    Test* b = new Test();
    delete b;
}
```

> **Output:** `C C D D`  
> (Static destructor runs at end of program)

---

##  Inheritance Constructor/Destructor Order

```cpp
class Base {
public:
    Base() { cout << "Base "; }
    ~Base() { cout << "~Base "; }
};

class Derived : public Base {
public:
    Derived() { cout << "Derived "; }
    ~Derived() { cout << "~Derived "; }
};

int main() {
    Derived d;
}
```

> **Output:** `Base Derived ~Derived ~Base`

---

##  Virtual Destructor

```cpp
class Parent {
public:
    Parent() { cout << "P "; }
    virtual ~Parent() { cout << "~P "; }
};

class Child : public Parent {
public:
    Child() { cout << "C "; }
    ~Child() { cout << "~C "; }
};

int main() {
    Parent* p = new Child();
    delete p;
}
```

> **Output:** `P C ~C ~P` 

---

##  Static Members Initialization

```cpp
class A {
    static int x;
public:
    A() { cout << "Ctor "; }
};

int A::x = [](){ cout << "StaticInit "; return 5; }();

int main() {
    A obj;
}
```

> **Output:** `StaticInit Ctor`

---

##  Object Slicing

```cpp
class Base {
public:
    Base() { cout << "Base "; }
    ~Base() { cout << "~Base "; }
};

class Derived : public Base {
public:
    Derived() { cout << "Derived "; }
    ~Derived() { cout << "~Derived "; }
};

void fun(Base b) {}

int main() {
    Derived d;
    fun(d);
}
```

> **Output:** `Base Derived Base ~Base ~Derived ~Base`  
> (Derived part sliced in `fun`)

---

##  Constructor / Destructor Order in Multi-level Inheritance

```cpp
class A {
public:
    A() { cout << "A "; }
    ~A() { cout << "~A "; }
};

class B : public A {
public:
    B() { cout << "B "; }
    ~B() { cout << "~B "; }
};

class C : public B {
public:
    C() { cout << "C "; }
    ~C() { cout << "~C "; }
};

int main() {
    C obj;
}
```

> **Output:** `A B C ~C ~B ~A`

---

##  Memory Leak Scenario

```cpp
class Leak {
public:
    Leak() { cout << "Create "; }
    ~Leak() { cout << "Destroy "; }
};

int main() {
    Leak* l = new Leak();
    // delete l; // Not called
}
```

> **Output:** `Create`  
>  `Destroy` not called --- **Memory leak**
