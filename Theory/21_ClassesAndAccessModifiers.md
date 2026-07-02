# Understanding Classes and Access Modifiers (Public & Private)

## 1. What is a Class?

A **class** is a blueprint or template for creating objects. 

- **Real-world analogy**: A class is like the design of a car (blueprint). The actual cars you drive are **objects** created from that blueprint.
- In programming: A class defines **properties** (data) and **behaviors** (methods/functions) that objects will have.

### Real Example: BankAccount Class

Create a new file called `BankAccount.java`:

```java
// This is the class definition (the blueprint)
public class BankAccount {
    
    // Properties (data) will go here later
    // Methods (behaviors) will go here later
    
}
```

**How to use the class** (create objects):

```java
public class Main {
    public static void main(String[] args) {
        
        // Creating objects from the BankAccount blueprint
        BankAccount account1 = new BankAccount();
        BankAccount account2 = new BankAccount();
        
        // account1 and account2 are now two separate bank accounts
    }
}
```

Each object (`account1`, `account2`) has its own copy of the data.

---

## 2. Why Do We Need Access Modifiers?

Access modifiers control **who can see and use** the properties and methods of a class.

**Real-world analogy**: 
- Your **public** phone number — anyone can call you.
- Your **private** bank PIN — only you (or the bank system) should know it.

This is called **Encapsulation** — hiding sensitive data and exposing only what is needed.

---

## 3. Public Access Modifier

**`public`** means: Anyone can access this (from any class, any package).

### When to use `public`:
- Methods that other parts of the program should call (like `deposit()` or `getBalance()`).
- The class itself (most classes start with `public`).

### Example with Public:

```java
public class BankAccount {
    
    // Public method - anyone can use it
    public void deposit(double amount) {
        System.out.println("Deposited: $" + amount);
        // Logic to add to balance would go here
    }
    
    public double getBalance() {
        return 500.75; // Example balance
    }
}
```

**Usage in another class**:

```java
public class Main {
    public static void main(String[] args) {
        BankAccount myAccount = new BankAccount();
        
        // Public methods are accessible
        myAccount.deposit(100.00);
        double bal = myAccount.getBalance();
        System.out.println("Current balance: $" + bal);
    }
}
```

**Output**:
```
Deposited: $100.0
Current balance: $500.75
```

---

## 4. Private Access Modifier

**`private`** means: Only code **inside the same class** can access it.

### When to use `private`:
- For sensitive data (balance, password).
- Helper methods that shouldn't be called directly.

### Example with Private:

```java
public class BankAccount {
    
    // Private property - cannot be accessed directly from outside
    private double balance = 500.75;
    
    // Public method to safely access the private data
    public double getBalance() {
        return balance;   // Allowed because it's inside the class
    }
    
    public void deposit(double amount) {
        if (amount > 0) {
            balance = balance + amount;  // Modifying private balance
            System.out.println("Deposited: $" + amount);
        } else {
            System.out.println("Invalid deposit amount!");
        }
    }
}
```

### What happens if you try to access private from outside?

```java
public class Main {
    public static void main(String[] args) {
        BankAccount myAccount = new BankAccount();
        
        // This will cause a COMPILATION ERROR:
        // System.out.println(myAccount.balance);  // ← Not allowed!
        
        // Correct way - use public methods
        myAccount.deposit(50.0);
        System.out.println("Balance: $" + myAccount.getBalance());
    }
}
```

**Why this is important**: It prevents bugs. No one can accidentally set `balance = -1000000` from outside the class.

---

## 5. Complete Working Example

Here is the full `BankAccount.java`:

```java
public class BankAccount {
    
    // Private data - protected from outside access
    private double balance;
    private String accountHolder;
    
    // Constructor (special method to initialize object)
    public BankAccount(String name, double initialBalance) {
        this.accountHolder = name;
        this.balance = initialBalance;
    }
    
    // Public methods - the "interface" of the class
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println(accountHolder + " deposited $" + amount);
        }
    }
    
    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            System.out.println(accountHolder + " withdrew $" + amount);
        } else {
            System.out.println("Insufficient funds or invalid amount!");
        }
    }
    
    public double getBalance() {
        return balance;
    }
    
    public String getAccountHolder() {
        return accountHolder;
    }
}
```

**Test it** (`Main.java`):

```java
public class Main {
    public static void main(String[] args) {
        BankAccount johnAccount = new BankAccount("John Doe", 1000.0);
        
        johnAccount.deposit(250.0);
        johnAccount.withdraw(300.0);
        
        System.out.println("Final balance for " + 
                          johnAccount.getAccountHolder() + 
                          ": $" + johnAccount.getBalance());
    }
}
```

**Expected Output**:
```
John Doe deposited $250.0
John Doe withdrew $300.0
Final balance for John Doe: $950.0
```

---

## 6. Key Rules Summary

| Modifier | Accessible From          | Use Case Example                     |
|----------|--------------------------|--------------------------------------|
| `public` | Anywhere                 | `deposit()`, `getBalance()`          |
| `private`| Only inside same class   | `balance`, `accountHolder`           |

- Always make **data (fields)** `private`.
- Provide `public` methods (getters/setters) to control access.
- This pattern is called **Encapsulation** — one of the core ideas of Object-Oriented Programming.

---