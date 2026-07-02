# Understanding Classes and Access Modifiers (Public & Private) in C++
## 1. What is a Class in C++?

A **class** is a blueprint for creating objects. It defines data (properties) and functions (behaviors).

- **Real-world analogy**: A class is like the engineering design of a car. Each actual car you create is an **object** made from that design.
- In C++: Classes help organize code and control access to data.

### Basic Class Structure

Create a file `BankAccount.cpp`:

```cpp
#include <iostream>
#include <string>

class BankAccount {
    // Data and functions go here
};

int main() {
    // We will create objects here later
    return 0;
}
```

**How to create objects** (instances):

```cpp
int main() {
    BankAccount account1;
    BankAccount account2;
    
    // account1 and account2 are separate bank accounts
    return 0;
}
```

---

## 2. Why Use Access Modifiers?

Access modifiers decide **who can access** class members (data and functions).

- **Real-world analogy**: 
  - Your **public** email address — anyone can send you messages.
  - Your **private** ATM PIN — only you should use it.

This concept is called **Encapsulation** — hiding internal details and exposing only safe ways to interact.

---

## 3. Public Access Modifier

**`public:`** means any code outside the class can access these members.

### When to use `public`:
- Functions that other parts of the program need to call (like deposit or check balance).
- Usually the class itself is accessible.

### Example with Public:

```cpp
#include <iostream>
using namespace std;

class BankAccount {
public:
    void deposit(double amount) {
        cout << "Deposited: $" << amount << std::endl;
    }
    
    double getBalance() {
        return 500.75;
    }
};

int main() {
    BankAccount myAccount;
    
    // Public members are accessible from main()
    myAccount.deposit(100.00);
    double bal = myAccount.getBalance();
    cout << "Current balance: $" << bal << std::endl;
    
    return 0;
}
```

**Compile and run** (using g++):
```bash
g++ BankAccount.cpp -o bank
./bank
```

**Output**:
```
Deposited: $100
Current balance: $500.75
```

---

## 4. Private Access Modifier

**`private:`** means only code **inside the same class** can access these members.

### When to use `private`:
- Sensitive data like balance or passwords.
- Internal helper logic.

### Example with Private:

```cpp
#include <iostream>

class BankAccount {
private:
    double balance = 500.75;   // Private data
    
public:
    double getBalance() {
        return balance;        // Allowed inside class
    }
    
    void deposit(double amount) {
        if (amount > 0) {
            balance = balance + amount;  // Modify private data safely
            std::cout << "Deposited: $" << amount << std::endl;
        } else {
            std::cout << "Invalid deposit amount!" << std::endl;
        }
    }
};

int main() {
    BankAccount myAccount;
    
    // This would cause compilation error:
    // myAccount.balance = 1000;   // ← Not allowed!
    
    // Correct way
    myAccount.deposit(50.0);
    std::cout << "Balance: $" << myAccount.getBalance() << std::endl;
    
    return 0;
}
```

**Key benefit**: Prevents accidental or wrong changes to important data from outside the class.

---

## 5. Complete Working Example in C++

Here is the full code `BankAccount.cpp`:

```cpp
#include <iostream>
#include <string>

class BankAccount {
private:
    double balance;
    std::string accountHolder;
    
public:
    // Constructor - runs when object is created
    BankAccount(std::string name, double initialBalance) {
        accountHolder = name;
        balance = initialBalance;
    }
    
    void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            std::cout << accountHolder << " deposited $" << amount << std::endl;
        }
    }
    
    void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            std::cout << accountHolder << " withdrew $" << amount << std::endl;
        } else {
            std::cout << "Insufficient funds or invalid amount!" << std::endl;
        }
    }
    
    double getBalance() {
        return balance;
    }
    
    std::string getAccountHolder() {
        return accountHolder;
    }
};

int main() {
    BankAccount johnAccount("John Doe", 1000.0);
    
    johnAccount.deposit(250.0);
    johnAccount.withdraw(300.0);
    
    std::cout << "Final balance for " 
              << johnAccount.getAccountHolder() 
              << ": $" << johnAccount.getBalance() << std::endl;
    
    return 0;
}
```

**Expected Output**:
```
John Doe deposited $250
John Doe withdrew $300
Final balance for John Doe: $950
```

---

## 6. Key Rules Summary

| Modifier   | Accessible From                  | Use Case Example                     |
|------------|----------------------------------|--------------------------------------|
| `public:`  | Anywhere (other classes, main)   | `deposit()`, `getBalance()`          |
| `private:` | Only inside same class           | `balance`, `accountHolder`           |

**Best Practice**:
- Make **data members** (variables) `private`.
- Provide `public` functions to read or safely modify them.
- Use a **constructor** to initialize private data when creating objects.

---