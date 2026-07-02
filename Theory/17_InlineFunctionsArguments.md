# Inline Functions, Default Arguments, and Constant Arguments in C++

## 1. Inline Functions

### What are Inline Functions?
Normally, when you call a function, the CPU jumps to the function's code, runs it, and jumps back. This "jump" takes a small amount of time (overhead).

An **inline function** tells the compiler: "Please copy the body of this function directly at the place where I call it." This removes the jump overhead. The function behaves like normal code inserted at the call site.

**Important note**: The compiler may ignore your `inline` request if the function is too big or complex. It is only a **hint**.

### Why Use Inline Functions?
- Faster execution for very small functions (like 1-3 lines).
- No function call overhead.
- Good for performance-critical code like game loops or math utilities.

**When NOT to use**: Big functions (more than 5-10 lines) — it can make your program bigger (code bloat) and slower due to larger executable size.

### Syntax
```cpp
inline returnType functionName(parameters) {
    // body - keep it short!
}
```

### Simple Example
```cpp
#include <iostream>

inline int add(int a, int b) {
    return a + b;
}

int main() {
    int result = add(5, 3);  // Compiler may replace this with: int result = 5 + 3;
    std::cout << "Sum is: " << result << std::endl;
    return 0;
}
```

### Real-World Example: Game Development
Imagine you are building a simple 2D game. You need to calculate distance between two points many times per frame (60 times per second).

```cpp
#include <iostream>
#include <cmath>

inline float distance(float x1, float y1, float x2, float y2) {
    float dx = x2 - x1;
    float dy = y2 - y1;
    return std::sqrt(dx*dx + dy*dy);
}

int main() {
    // Called thousands of times in game loop
    float playerX = 10.0f, playerY = 20.0f;
    float enemyX = 15.0f, enemyY = 25.0f;
    
    float dist = distance(playerX, playerY, enemyX, enemyY);
    std::cout << "Distance to enemy: " << dist << std::endl;
    return 0;
}
```

Without `inline`, the repeated function calls would add unnecessary overhead in a tight game loop.

---

## 2. Default Arguments

### What are Default Arguments?
You can give parameters **default values** in the function declaration. If the caller does not pass a value for that parameter, the default is used.

This makes functions easier to call when you usually want the same value.

### Why Use Default Arguments?
- Reduces typing for common cases.
- Makes APIs more flexible without overloading functions.
- Common in libraries (e.g., logging functions with default log level).

**Rules**:
- Default arguments must be on the right side (trailing parameters).
- You cannot skip a parameter with default in the middle.

### Syntax
```cpp
returnType functionName(type param1, type param2 = defaultValue) {
    // body
}
```

### Simple Example
```cpp
#include <iostream>

// Function with default argument
void printMessage(std::string message, int repeatCount = 1) {
    for(int i = 0; i < repeatCount; i++) {
        std::cout << message << std::endl;
    }
}

int main() {
    printMessage("Hello");           // Uses default: repeatCount = 1
    printMessage("Hi there", 3);     // Overrides default
    return 0;
}
```

**Output**:
```
Hello
Hi there
Hi there
Hi there
```

### Real-World Example: Database Connection
In a web backend, you often connect to a database with default host and port.

```cpp
#include <iostream>
#include <string>

void connectToDatabase(std::string host = "localhost", 
                      int port = 5432, 
                      std::string username = "admin") {
    std::cout << "Connecting to " << host 
              << ":" << port 
              << " as user " << username << std::endl;
    // Actual connection code would go here
}

int main() {
    connectToDatabase();                    // All defaults
    connectToDatabase("production-db.com"); // Only override host
    connectToDatabase("staging.com", 3306); // Override host and port
    return 0;
}
```

This saves you from writing many similar functions.

---

## 3. Constant Arguments (const Parameters)

### What are Constant Arguments?
Adding `const` before a parameter means **"this function promises not to modify this argument"**.

- For basic types (`int`, `float`): prevents accidental change inside function.
- For objects and references (`std::string&`): very important — avoids copying large objects and guarantees no modification.

### Why Use const Arguments?
- Prevents bugs (you cannot accidentally change data).
- Allows compiler optimizations.
- Clear contract: "I will only read this, not write to it."
- Required when passing `const` objects.

### Syntax
```cpp
returnType functionName(const Type param) {  // value
    // or
returnType functionName(const Type& param) { // reference - very common
    // body - cannot do param = something;
}
```

### Simple Example
```cpp
#include <iostream>
#include <string>

void printLength(const std::string& text) {  // const reference
    // text = "new value";  // ERROR! Cannot modify
    std::cout << "Length: " << text.length() << std::endl;
}

int main() {
    std::string name = "Grok";
    printLength(name);           // No copy of string happens
    std::cout << "Original name is still: " << name << std::endl;
    return 0;
}
```

### Real-World Example: Processing User Data
You have a large `User` object. You want to display info without changing it.

```cpp
#include <iostream>
#include <string>

struct User {
    std::string name;
    int age;
};

void displayUserInfo(const User& user) {  // const reference - efficient + safe
    std::cout << "Name: " << user.name << std::endl;
    std::cout << "Age: " << user.age << std::endl;
    // user.age = 30;  // Compiler error - good protection!
}

int main() {
    User currentUser{"Alice", 28};
    displayUserInfo(currentUser);
    
    // currentUser remains unchanged
    std::cout << "After display, age is still: " << currentUser.age << std::endl;
    return 0;
}
```

**Best Practice**: Always use `const` for parameters you don't intend to modify, especially references.

---

## Combining All Three Concepts

Here is one function that uses **inline**, **default arguments**, and **const**:

```cpp
#include <iostream>
#include <string>

// Inline + default + const
inline void logMessage(const std::string& message, 
                      int logLevel = 1) {  // default argument
    std::cout << "[Level " << logLevel << "] " << message << std::endl;
}

int main() {
    logMessage("Application started");           // default level
    logMessage("Error occurred", 3);             // custom level
    return 0;
}
```

---

## Quick Summary Table

| Feature              | Purpose                          | Keyword/ Syntax          | Best For                     |
|----------------------|----------------------------------|--------------------------|------------------------------|
| Inline Function      | Remove call overhead             | `inline`                 | Tiny helper functions        |
| Default Arguments    | Provide common values            | `= value` (right side)   | Flexible APIs                |
| Const Arguments      | Protect data from modification   | `const` / `const &`      | Safety + performance         |
