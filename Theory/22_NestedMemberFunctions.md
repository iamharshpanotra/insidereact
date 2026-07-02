# Nesting of Member Functions in C++

## 1. What is Nesting of Member Functions?

**Simple Definition**:  
When a member function (method) inside a class calls another member function of the **same class**, it is called **nesting**.

- You don't need any special keyword.
- The called function must be a member of the same class.
- It helps break big logic into smaller, reusable pieces.

**Key Point**: The inner function can access all private/protected data members and other member functions easily because they belong to the same object (`this` pointer is shared).

---

## 2. Basic Syntax

```cpp
class ClassName {
private:
    // data members
    int value;

public:
    // member functions
    void outerFunction() {
        // This is nesting!
        innerFunction();     // calling another member function
    }

    void innerFunction() {
        // logic here
        cout << "Inner function called!" << endl;
    }
};
```

---

## 3. Complete Working Example

Let's create a simple **Bank Account** class. This is very common in real projects.

```cpp
#include <iostream>
using namespace std;

class BankAccount {
private:
    string accountHolder;
    double balance;

public:
    // Constructor
    BankAccount(string name, double initialBalance) {
        accountHolder = name;
        balance = initialBalance;
    }

    // Nested member function example
    void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            cout << "Deposited: $" << amount << endl;
            showBalance();        // <-- Nesting happens here!
        } else {
            cout << "Invalid deposit amount!" << endl;
        }
    }

    void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            cout << "Withdrawn: $" << amount << endl;
            showBalance();        // <-- Nesting again!
        } else {
            cout << "Insufficient balance or invalid amount!" << endl;
        }
    }

    // This function is being called by others (nested)
    void showBalance() {
        cout << "Current Balance for " << accountHolder 
             << ": $" << balance << endl;
    }
};

int main() {
    BankAccount myAccount("Alice", 1000.0);
    
    myAccount.deposit(500.0);   // Calls showBalance() internally
    myAccount.withdraw(200.0);  // Calls showBalance() internally
    
    return 0;
}
```

**Output**:
```
Deposited: $500
Current Balance for Alice: $1500
Withdrawn: $200
Current Balance for Alice: $1300
```

---

## 4. Real-World Example: Employee Management System

Imagine you are working on a payroll system for a company.

```cpp
class Employee {
private:
    string name;
    double salary;
    int workingDays;

public:
    Employee(string empName, double empSalary) {
        name = empName;
        salary = empSalary;
        workingDays = 0;
    }

    void markAttendance(int days) {
        workingDays += days;
        calculateBonus();           // Nesting!
    }

    void calculateBonus() {
        double bonus = 0;
        if (workingDays > 25) {
            bonus = salary * 0.10;  // 10% bonus
        }
        displaySalaryWithBonus(bonus);  // Another nesting!
    }

    void displaySalaryWithBonus(double bonus) {
        cout << "Employee: " << name << endl;
        cout << "Base Salary: $" << salary << endl;
        cout << "Bonus: $" << bonus << endl;
        cout << "Total: $" << (salary + bonus) << endl << endl;
    }
};

int main() {
    Employee emp("Bob", 50000);
    emp.markAttendance(28);  // This triggers multiple nested calls
    return 0;
}
```

**Why is this useful in real projects?**
- `markAttendance()` handles input.
- `calculateBonus()` contains business logic.
- `displaySalaryWithBonus()` handles output.
- Code stays clean and maintainable.

---

## 5. Important Rules & Things to Remember

1. **Access**: The nested function can access **private** members directly.
2. **Order of Declaration**: You can call a function declared **after** the calling function (because of class scope).
3. **Recursion**: A function can call itself (direct nesting) — but be careful with infinite loops.
4. **Static Member Functions**: Can call other static members only.
5. **No Infinite Nesting**: Don't create deep unnecessary chains.

**Common Beginner Mistake**:
```cpp
void functionA() {
    functionB();  // Error if functionB is not in same class
}
```

**Correct**:
Both functions must be inside the same class.

---

## 6. Benefits of Nesting Member Functions

- **Code Reusability**: Write logic once, call from multiple places.
- **Better Readability**: Break complex functions into smaller ones.
- **Maintainability**: Easy to debug and update.
- **Encapsulation**: All logic stays inside the class.

---