# Recursion and Recursive Functions in C++

## 1. What is Recursion? (Real-World Analogy)

**Recursion** is when a function **calls itself** to solve a smaller version of the same problem.

### Real-World Example: Russian Doll (Matryoshka)
Imagine you open a big doll. Inside it is a smaller doll. You open that and find an even smaller one. You keep doing this until you reach the tiniest doll that doesn't open anymore.

- The action of "opening the doll" = **recursive call**
- The tiniest doll = **base case** (stop condition)

Another example:  
You want to find a book in a big library. The librarian says: "Check shelf A. If not there, the book tells you to check shelf B." You follow the instruction (call the same process again) until you find the book or reach the end.

---

## 2. Two Important Parts of Every Recursive Function

Every recursive function **must** have:

1. **Base Case** (Stopping Condition)  
   - This tells the function "Stop! Don't call yourself again."  
   - Without this, your program will crash with "stack overflow" (too many calls).

2. **Recursive Case**  
   - The part where the function calls **itself** with a **smaller** problem.

Think of it as:
```cpp
int recursive_function(int problem) {
    if (base_case_condition) {   // Stopping point
        return simple_answer;
    } else {
        return recursive_function(smaller_problem);  // Call yourself
    }
}
```

---

## 3. Example 1: Factorial (Most Common Beginner Example)

**Problem**: Calculate 5! (5 factorial)  
5! = 5 × 4 × 3 × 2 × 1 = 120

### Real-World Use
- Used in probability, permutations (how many ways to arrange items), and some algorithms.

### C++ Code

```cpp
#include <iostream>

int factorial(int n) {
    // Base Case - Stop when we reach 1
    if (n == 1) {
        return 1;
    }
    // Recursive Case - Call itself with smaller number
    else {
        return n * factorial(n - 1);
    }
}

int main() {
    cout << factorial(5) << std::endl;  // Output: 120
    return 0;
}
```

### Step-by-Step Execution (How it unfolds)

```
factorial(5)
    → 5 * factorial(4)
         → 4 * factorial(3)
              → 3 * factorial(2)
                   → 2 * factorial(1)
                        → returns 1 (base case)

Now it multiplies back:
2 * 1 = 2
3 * 2 = 6
4 * 6 = 24
5 * 24 = 120
```

---

## 4. Example 2: Fibonacci Sequence

**Problem**: Find the nth Fibonacci number.  
Sequence: 0, 1, 1, 2, 3, 5, 8, 13, 21...

### C++ Code

```cpp
#include <iostream>

int fibonacci(int n) {
    // Base Cases (two of them!)
    if (n == 0) {
        return 0;
    }
    if (n == 1) {
        return 1;
    }
    // Recursive Case
    return fibonacci(n - 1) + fibonacci(n - 2);
}

int main() {
    std::cout << fibonacci(6) << std::endl;  // Output: 8
    return 0;
}
```

---

## 5. Example 3: Real-World - Sum of Numbers in an Array

### C++ Code

```cpp
#include <iostream>

int sum_array(int arr[], int size) {
    // Base Case: size is 0
    if (size == 0) {
        return 0;
    }
    // Recursive Case: take first number + sum of rest
    return arr[0] + sum_array(arr + 1, size - 1);
}

int main() {
    int numbers[] = {1, 2, 3, 4, 5};
    std::cout << sum_array(numbers, 5) << std::endl;  // Output: 15
    return 0;
}
```

---

## 6. Recursion vs Iteration (Loop)

| Situation                  | Use Recursion                          | Use Loop (Iteration)              |
|---------------------------|----------------------------------------|-----------------------------------|
| Tree/Folder structure     | Natural choice                         | Possible but harder               |
| Factorial / Fibonacci     | Clean code                             | Faster and uses less memory       |
| Simple counting           | Overkill                               | Better (for loop)                 |
| Deep nesting (1000 levels)| Risk of stack overflow                 | Safer                             |

---

## 7. Common Mistakes + Debugging

- Always add a proper base case.
- Use `cout` inside the function to trace calls (shown in the guide).

---

## 8. Practice Problems (with C++ starter)

**Countdown Example**:

```cpp
#include <iostream>

void countdown(int n) {
    if (n <= 0) {
        std::cout << "Done!" << std::endl;
        return;
    }
    std::cout << n << std::endl;
    countdown(n - 1);
}

int main() {
    countdown(5);
    return 0;
}
```

