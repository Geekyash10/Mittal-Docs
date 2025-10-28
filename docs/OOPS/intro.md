# BASICS
There are three main types of programming languages:

## 1. Machine-Level Language

- This is the lowest-level language.
- Instructions are directly executed by the CPU.
- Uses binary numbers (0 and 1) which are hard to read and write.

🔹 **Example**:
```text
10110000 01100001
```
This might mean something like: *“move the number 97 into a register”*.  
But it's very difficult to understand just by looking!

---

## 2. Assembly-Level Language

- A bit easier than machine language.
- Uses short words called **mnemonics** instead of 0s and 1s.
- Still closely related to hardware.
- Needs an **assembler** to convert to machine code.

🔹 **Example**:
```asm
MOV A, 5
ADD A, 10
```

This means:
- Move the number 5 into register A.  
- Add 10 to A.

---

## 3. High-Level Language

- Human-readable, like English instructions.
- Not tied to specific hardware.
- Needs a **compiler** or **interpreter** to run.
- Used in most modern programming.

🔹 **Example in Python**:
```python
a = 5
a = a + 10
print(a)
```

This means:
- Start with 5 in variable `a`.  
- Add 10 to `a`.  
- Print the result.

---


## Procedure Oriented Programming (POP)

Procedure Oriented Programming (POP) is a programming method where the focus is on writing functions or procedures to perform specific tasks. In this approach, a program is divided into smaller parts called functions, and each function handles a particular task. The instructions are executed in a step-by-step manner. Functions often share global data, which makes it easier to access and modify data across the program. While POP is suitable for small and simple applications, it becomes harder to manage and maintain as the program grows larger. It also lacks strong support for data security and code reuse compared to modern programming approaches.


---

## Object Oriented Programming (OOP)

Object Oriented Programming (OOP) is a programming approach where the main focus is on objects — which are like real-world things that contain both data (called attributes) and functions (called methods) that work on the data. In OOP, programs are built using classes, which act like blueprints for creating objects. This approach helps in organizing code better, making it easier to understand, reuse, and maintain.


---

# Four Pillars of OOP and Basic terms

### 1. Data Abstraction
Hiding implementation details from the user. Users only interact with simple functions without knowing how they work internally.
**Example:** Using a TV remote - you press a button to increase volume, but you don't need to know the internal electronics.

### 2. Data Encapsulation
Wrapping data and functions into a single unit (class). The data is protected and only accessible through class methods.
### 3. Data Inheritance
Allows a class to inherit properties and methods from another class. This helps reduce code duplication by reusing common functionality.

### 4. Polymorphism
The ability of a function or operator to take multiple forms. Includes function overloading and operator overloading.

### 5. Static vs Dynamic Binding

Static Binding (Without Virtual) -> Since speak() is not virtual, the compiler binds the call at compile-time based on the pointer type Animal*.
It doesn't care that a points to a Dog object. So it calls Animal::speak() — not Dog::speak()

```cpp
#include <iostream>
using namespace std;

class Animal {
public:
    void speak() {  // Not virtual
        cout << "Animal speaks" << endl;
    }
};

class Dog : public Animal {
public:
    void speak() {
        cout << "Dog barks" << endl;
    }
};

int main() {
    Animal* a = new Dog();
    a->speak();  // Output: Animal speaks
    return 0;
}
```

Dynamic Binding (With Virtual) -> Since speak() is marked as virtual, the compiler defers the decision to run-time. At run-time, the program checks the actual object (Dog) and calls Dog::speak().

```cpp
#include <iostream>
using namespace std;

class Animal {
public:
   virtual void speak() {  //  virtual
        cout << "Animal speaks" << endl;
    }
};

class Dog : public Animal {
public:
    void speak() {
        cout << "Dog barks" << endl;
    }
};

int main() {
    Animal* a = new Dog();
    a->speak();  // Output: Dog barks
    return 0;
}
```

### 6. Objects
Objects are basic run-time entities in object-oriented systems. They can represent anything like a person, place, bank account, etc.

**Example:** 
Consider Amit who is 25 years old with a salary of 2500. In a program, Amit can be represented as an object with data: `(name: Amit, age: 25, salary: 2500)`

### 7. Class
A class is a group of objects that share common properties for both data and methods.



