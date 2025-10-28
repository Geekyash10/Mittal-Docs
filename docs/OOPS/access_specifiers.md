


---

##  Core Concepts:

- `public`: Accessible everywhere.
- `private`: Accessible only within the class.
- `protected`: Accessible within the class and its derived classes.

---

## OUTPUT-BASED TRICKY QUESTIONS ON ACCESS SPECIFIERS

### Q1. Accessing Private Member From Outside

```cpp
class A {
private:
    int x = 10;
};

int main() {
    A obj;
    cout << obj.x;
}
```

 **Compilation Error** – x is private, can't be accessed directly outside the class.

---

### Q2. Accessing Protected Member From Outside

```cpp
class A {
protected:
    int y = 20;
};

int main() {
    A obj;
    cout << obj.y;
}
```

 **Compilation Error** – protected members are not accessible outside, only inside class or derived class.

---

### Q3. Accessing Protected Member From Derived Class

```cpp
class Base {
protected:
    int x = 5;
};

class Derived : public Base {
public:
    void show() {
        cout << x;
    }
};

int main() {
    Derived d;
    d.show();
}
```

 **Output:** 5

---

### Q4. Friend Function Accessing Private Member

```cpp
class Secret {
private:
    int code = 999;
    friend void reveal(Secret s);
};

void reveal(Secret s) {
    cout << s.code;
}

int main() {
    Secret s;
    reveal(s);
}
```

 **Output:** 999

---

### Q5. Public Inheritance with Protected Member

```cpp
class A {
protected:
    int x = 10;
};

class B : public A {
public:
    void display() {
        cout << x;
    }
};

int main() {
    B obj;
    obj.display();
}
```

 **Output:** 10

---

### Q6. Private Inheritance – Accessing Public Members

```cpp
class A {
public:
    int x = 100;
};

class B : private A {
public:
    void show() {
        cout << x;
    }
};

int main() {
    B b;
    b.show();
}
```

 **Output:** 100  
 `b.x` not accessible in `main()` due to private inheritance.

---

### Q7. Protected Inheritance – Accessing Public Member

```cpp
class A {
public:
    int x = 42;
};

class B : protected A {
public:
    void show() {
        cout << x;
    }
};

int main() {
    B obj;
    obj.show();
}
```

 **Output:** 42  
 `obj.x` not accessible in `main()` due to protected inheritance.

---

### Q8. Access Private Member via Public Method

```cpp
class A {
private:
    int data = 50;
public:
    int getData() {
        return data;
    }
};

int main() {
    A a;
    cout << a.getData();
}
```

 **Output:** 50

---

### Q9. Friend Class Accessing Private Member

```cpp
class A {
private:
    int val = 77;
    friend class B;
};

class B {
public:
    void print(A obj) {
        cout << obj.val;
    }
};

int main() {
    A a;
    B b;
    b.print(a);
}
```

 **Output:** 77

---

### Q10. Access Control in Struct vs Class

```cpp
struct S {
    int x;
};

class C {
    int y;
};

int main() {
    S s;
    s.x = 10;
    cout << s.x;

    C c;
    c.y = 20;  //  Error
}
```

 **Output:** 10  
 `c.y = 20` is a **compilation error** – struct members are public by default, class members are private by default.

---

##  INTERVIEW-STYLE THEORY + MCQs

**Q1. Which access specifier is default for class members?**  
Answer: **B) private**

**Q2. What is the default access modifier for struct members?**  
Answer: **C) public**

**Q3. Which access specifier allows access only from within the same class?**  
Answer: **B) private**

**Q4. When a class is privately inherited, how are the base class’s public members treated in the derived class?**  
Answer: **C) They become private**

**Q5. What is the access level of protected members in derived class with public inheritance?**  
Answer: **B) protected**

---

## Summary Table – Access in Inheritance

| Inheritance Type | Public in Base → | Protected in Base → | Private in Base → |
|------------------|------------------|----------------------|--------------------|
| public           | public           | protected            | Not inherited      |
| protected        | protected        | protected            | Not inherited      |
| private          | private          | private              | Not inherited      |
