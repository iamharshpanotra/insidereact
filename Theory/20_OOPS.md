# Object Oriented Programming in C++

Think of OOP like building with Lego blocks. Instead of writing all code in one long list (procedural style), OOP lets you create "blueprints" (classes) for real-world things and build objects from them. This makes code reusable, organized, and easier to maintain.

## 1. Why OOP? Real-World Example

Imagine you're building a **Banking App**. Without OOP:
- You might have separate functions for deposit, withdraw for savings, checking accounts – lots of repeated code.

With OOP:
- Create a "Account" blueprint. All account types inherit from it. Saves time and reduces bugs.

OOP helps in large projects like games (Player, Enemy classes), web apps (User, Product classes), etc.

## 2. Classes and Objects

**Class**: A blueprint or template.  
**Object**: An actual instance created from the class.

### Real-World Example: Car
- Class `Car` defines properties (color, model) and behaviors (drive, stop).
- Objects: `myToyota`, `yourHonda` – each with their own data.

```cpp
#include <iostream>
#include <string>
using namespace std;

// Class definition
class Car {
public:
    // Properties (data members)
    string model;
    string color;
    int speed;

    // Behavior (member function)
    void drive() {
        cout << "The " << color << " " << model << " is driving at " << speed << " km/h!" << endl;
    }
};

int main() {
    // Creating objects
    Car myCar;
    myCar.model = "Toyota Camry";
    myCar.color = "Red";
    myCar.speed = 80;
    myCar.drive();  // Output: The Red Toyota Camry is driving at 80 km/h!

    Car yourCar;
    yourCar.model = "Honda Civic";
    yourCar.color = "Blue";
    yourCar.speed = 60;
    yourCar.drive();

    return 0;
}
```

**Key Points**:
- `public:` makes members accessible outside the class.
- Use `.` (dot) to access members on objects.

## 3. Constructors

Special function called automatically when object is created. Initializes data.

```cpp
class Car {
public:
    string model;
    string color;
    int speed;

    // Constructor
    Car(string m, string c, int s) {
        model = m;
        color = c;
        speed = s;
        cout << "A new " << model << " car created!" << endl;
    }

    void drive() {
        cout << "Driving..." << endl;
    }
};

int main() {
    Car myCar("Toyota", "Red", 80);  // Constructor called automatically
    myCar.drive();
    return 0;
}
```

**Default Constructor**: No parameters. You can have multiple (overloading).

## 4. Encapsulation (Data Hiding)

Bundle data and methods together. Hide internal details using `private`.

Protects data from accidental changes.

```cpp
class BankAccount {
private:
    double balance;  // Hidden

public:
    BankAccount(double initial) {
        balance = initial;
    }

    // Getter
    double getBalance() {
        return balance;
    }

    // Setter with validation (real-world: can't withdraw more than balance)
    void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            cout << "Deposited: $" << amount << endl;
        }
    }

    void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            cout << "Withdrawn: $" << amount << endl;
        } else {
            cout << "Insufficient funds!" << endl;
        }
    }
};

int main() {
    BankAccount account(1000.0);
    account.deposit(500);
    cout << "Balance: $" << account.getBalance() << endl;  // Safe access
    account.withdraw(2000);  // Validation prevents error
    return 0;
}
```

## 5. Inheritance

Create new class based on existing one. "Is-a" relationship.

Real-world: `SavingsAccount` is a `BankAccount`.

```cpp
// Base class
class BankAccount {
protected:  // Accessible to derived classes
    double balance;

public:
    BankAccount(double b) : balance(b) {}

    void deposit(double amt) { if (amt > 0) balance += amt; }
    double getBalance() { return balance; }
};

// Derived class
class SavingsAccount : public BankAccount {
private:
    double interestRate;

public:
    SavingsAccount(double b, double rate) : BankAccount(b), interestRate(rate) {}

    void addInterest() {
        balance += balance * interestRate;
        cout << "Interest added!" << endl;
    }
};

int main() {
    SavingsAccount sa(1000, 0.05);
    sa.deposit(100);  // Inherited
    sa.addInterest();
    cout << "Balance: " << sa.getBalance() << endl;
    return 0;
}
```

**Types**: Single, Multiple, Multilevel, Hierarchical.

## 6. Polymorphism

"Many forms". Same method name, different behavior.

### Function Overriding (Runtime Polymorphism)

```cpp
class Shape {
public:
    virtual void draw() {  // virtual for polymorphism
        cout << "Drawing shape" << endl;
    }
};

class Circle : public Shape {
public:
    void draw() override {
        cout << "Drawing Circle" << endl;
    }
};

class Rectangle : public Shape {
public:
    void draw() override {
        cout << "Drawing Rectangle" << endl;
    }
};

int main() {
    Shape* shape1 = new Circle();
    Shape* shape2 = new Rectangle();
    
    shape1->draw();  // Calls Circle's draw
    shape2->draw();  // Calls Rectangle's draw
    
    delete shape1;
    delete shape2;
    return 0;
}
```

**Function Overloading**: Same name, different parameters (compile-time).

## 7. Abstraction

Hide complex implementation, show only essentials. Use **Abstract Classes** (with pure virtual functions).

```cpp
// Abstract class
class Payment {
public:
    virtual void processPayment(double amount) = 0;  // Pure virtual
    virtual ~Payment() {}  // Virtual destructor
};

class CreditCardPayment : public Payment {
public:
    void processPayment(double amount) override {
        cout << "Processing $" << amount << " via Credit Card" << endl;
        // Complex logic hidden
    }
};

int main() {
    Payment* payment = new CreditCardPayment();
    payment->processPayment(99.99);
    delete payment;
    return 0;
}
```