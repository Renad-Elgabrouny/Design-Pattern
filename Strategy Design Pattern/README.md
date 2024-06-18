# Strategy Design Pattern: A Comprehensive Guide

## Table of Contents
1. [Introduction to Design Patterns](#Introduction-to-Design-Patterns)
2. [Key Concepts](#Key-Concepts)
3. [Advantages of Using Design Patterns](#Advantages-of-Using-Design-Patterns)
4. [Duck Example in C++](#Duck-Example-in-C++)
5. [Problems with This Approach](#Problems-with-This-Approach)
6. [Solution Using Design Patterns](#Solution-Using-Design-Patterns)
    - [Step 1: Define Behaviors](#Step-1:-Define-Behaviors)
    - [Step 2: Implement Specific Behaviors](#Step-2:-Implement-Specific-Behaviors)
    - [Step 3: Integrate Behaviors with Duck](#Step-3:-Integrate-Behaviors-with-Duck)
7. [Summary](#summary)

## Introduction to Design Patterns

Design Patterns are typical solutions to common problems in software design. They are like pre-defined templates that can be adapted to solve design problems in different situations. Instead of reinventing the wheel, developers can use design patterns to save time and avoid common pitfalls.

## Key Concepts

1. **Encapsulation**: Grouping related variables and functions together.
2.**Inheritance**: A way to form new classes using classes that have already been defined.
3. **Polymorphism**: Allowing methods to do different things based on the object it is acting upon.

   
## Advantages of Using Design Patterns

1. Reusability: Specific behavior classes can be reused by different duck types.
2. Flexibility: Behaviors can be changed at runtime.
3. Maintainability: Adding new behaviors doesn't require changing the existing duck classes.


## Duck Example in C++

```cpp
#include <iostream>
using namespace std;

// Base Duck class
class Duck {
public:
    void quack() {
        cout << "Quack!" << endl;
    }
    void swim() {
        cout << "Swimming!" << endl;
    }
    virtual void display() = 0; // Pure virtual function making Duck an abstract class
};

// MallardDuck class inheriting from Duck
class MallardDuck : public Duck {
public:
    void display() override {
        cout << "I'm a Mallard Duck!" << endl;
    }
};

// RubberDuck class inheriting from Duck
class RubberDuck : public Duck {
public:
    void display() override {
        cout << "I'm a Rubber Duck!" << endl;
    }
    void quack() {
        cout << "Squeak!" << endl;
    }
};

// DecoyDuck class inheriting from Duck
class DecoyDuck : public Duck {
public:
    void display() override {
        cout << "I'm a Decoy Duck!" << endl;
    }
    void quack() {
        // Decoy ducks don't quack
    }
};


```


## Problems with This Approach

1. Inflexible Design: Adding or modifying behavior isn't easy. For example, if you want a rubber duck to squeak instead of quack, you need to override the quack method in the RubberDuck class.
2. Code Duplication: Common behaviors (like quack and swim) are duplicated across different duck types.
3. Maintenance Issue: If a new type of duck or new behavior is introduced, you need to modify existing code, risking errors and making maintenance harder.
  



## Solution Using Design Patterns
we can use the Strategy Pattern. This pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable. It lets the algorithm vary independently from the clients that use it.

### Step 1: Define Behaviors

```cpp
// Interface for QuackBehavior
class QuackBehavior {
public:
    virtual void quack() = 0;
};

// Interface for FlyBehavior
class FlyBehavior {
public:
    virtual void fly() = 0;
};
```

### Step 2: Implement Specific Behaviors
```cpp
// Implementation of QuackBehavior for normal quacking
class Quack : public QuackBehavior {
public:
    void quack() override {
        cout << "Quack!" << endl;
    }
};

// Implementation of QuackBehavior for squeaking
class Squeak : public QuackBehavior {
public:
    void quack() override {
        cout << "Squeak!" << endl;
    }
};

// Implementation of FlyBehavior for flying with wings
class FlyWithWings : public FlyBehavior {
public:
    void fly() override {
        cout << "I'm flying!" << endl;
    }
};

// Implementation of FlyBehavior for ducks that can't fly
class FlyNoWay : public FlyBehavior {
public:
    void fly() override {
        // Do nothing - can't fly
    }
};

```

### Step 3: Integrate Behaviors with Duck
```cpp
// Base Duck class using behaviors
class Duck {
protected:
    QuackBehavior* quackBehavior;
    FlyBehavior* flyBehavior;

public:
    Duck() : quackBehavior(nullptr), flyBehavior(nullptr) {}
    virtual ~Duck() {
        delete quackBehavior;
        delete flyBehavior;
    }
    
    void performQuack() {
        if (quackBehavior) quackBehavior->quack();
    }

    void performFly() {
        if (flyBehavior) flyBehavior->fly();
    }

    void swim() {
        cout << "Swimming!" << endl;
    }

    virtual void display() = 0; // Pure virtual function making Duck an abstract class

    void setQuackBehavior(QuackBehavior* qb) {
        if (quackBehavior) delete quackBehavior;
        quackBehavior = qb;
    }

    void setFlyBehavior(FlyBehavior* fb) {
        if (flyBehavior) delete flyBehavior;
        flyBehavior = fb;
    }
};

// MallardDuck class inheriting from Duck
class MallardDuck : public Duck {
public:
    MallardDuck() {
        quackBehavior = new Quack();
        flyBehavior = new FlyWithWings();
    }

    void display() override {
        cout << "I'm a Mallard Duck!" << endl;
    }
};

// RubberDuck class inheriting from Duck
class RubberDuck : public Duck {
public:
    RubberDuck() {
        quackBehavior = new Squeak();
        flyBehavior = new FlyNoWay();
    }

    void display() override {
        cout << "I'm a Rubber Duck!" << endl;
    }
};

// DecoyDuck class inheriting from Duck
class DecoyDuck : public Duck {
public:
    DecoyDuck() {
        quackBehavior = new Quack(); // Decoys don't quack, but for simplicity
        flyBehavior = new FlyNoWay();
    }

    void display() override {
        cout << "I'm a Decoy Duck!" << endl;
    }
};
```

## Summary
* Design Patterns: Reusable solutions to common problems.
* Strategy Pattern: Encapsulates behaviors (e.g., quacking, flying) and makes them interchangeable.
* Composition Over Inheritance: Use objects to achieve flexible code.
