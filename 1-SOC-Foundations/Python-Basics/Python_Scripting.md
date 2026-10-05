# 🐍 Python Scripting: Simple Automation Demo

**Module:** Software Basics  
**Chapter:** Python: Simple Demo  
**Objective:** Explore the fundamentals of Python scripting, understand variable states, trace conditional flow matrices (`if/elif/else`), and analyze iteration constraints using `while` loops.

---

## 🏗️ The Three Pillars of Imperative Programming

Security analysts leverage Python to automate log ingestion, parse indicator files, and orchestrate rapid containment alerts. This room demonstrates the three structural components foundational to modern software:

1. **Variables:** Virtual containers used by the program to store data values dynamically in volatile memory (e.g., tracking current session states).
2. **Conditionals:** Logical decision points (`if`, `elif`, `else`) that route the script's execution flow based on comparison parameters.
3. **Loops (Iteration):** Multi-step blocks that repeat instructions sequentially as long as a specific boolean condition evaluates to true.

---

## 💻 Code Evolution: "Guess the Number" Script Breakdown

The application picks a random target integer between `1` and `20` using the `random` standard library, tracks user input parameters, provides operational boundary validation, and iterates until the target constraint is satisfied.

### 📜 Version 1: The Linear Draft (Single Attempt Boundary)
This layout executes a simple top-to-bottom sequence. It captures user text, maps it to an integer array, verifies the input via conditional logic, and terminates regardless of accuracy.

```python
import random  # Provides tools for picking random numbers

secret = random.randint(1, 20)  # Pick an integer where: 1 <= secret <= 20
tries = 0
guess = 0  # Initialized outside the valid range to allow subsequent processing

print("I'm thinking of a number between 1 and 20")

text = input("Take a guess: ")  # Captured as a text string primitive
guess = int(text)  # Explicit type conversion from string to integer

tries = tries + 1  # Long-form increment tracking the session attempt

# Evaluate structural constraints using if / elif / else
if guess < 1 or guess > 20:
    print("That number is out of range. Try again.")
elif guess < secret:
    print("Too low, try again.")
elif guess > secret:
    print("Too high, try again.")
else:
    print("You got it in", tries, "tries!")
```

### 📜 Version 2: The Iterative Build (Infinite Attempt Tracking via `while` Loop)
To allow continuous testing without script crash termination, the conditional matrix is embedded inside a dynamic `while` block using the **"Does Not Equal" (`!=`)** comparison operator.

```python
import random

secret = random.randint(1, 20)
tries = 0
guess = 0

print("I'm thinking of a number between 1 and 20")

# Loop persists iteratively as long as the comparison state evaluates to True
while guess != secret:
    text = input("Take a guess: ")
    guess = int(text)
    
    tries = tries + 1

    if guess < 1 or guess > 20:
        print("That number is out of range. Try again.")
    elif guess < secret:
        print("Too low, try again.")
    elif guess > secret:
        print("Too high, try again.")
    else:
        print("You got it in", tries, "tries!")
```

---

## 🔍 Log Execution Trace Matrix

An analysis of how Python's engine evaluates boolean logic gates at runtime during active loop tracking:

* **Boundary Violation Input (`guess = 30` | `secret = 10`):** The statement `guess < 1 or guess > 20` matches the right-hand logic gate (`30 > 20 = True`). The script instantly routes to the initial warning message.
* **Low Threshold Input (`guess = 5` | `secret = 10`):** The first boundary check fails. Execution shifts to the `elif guess < secret` block, which scales as true (`5 < 10`), firing the "Too low" alert parameter.
* **Equality Break Condition (`guess = 10` | `secret = 10`):** When the input matches the target asset value exactly, the final `else` fallback is executed. On the next loop evaluation step, the check `10 != 10` resolves as **False**, breaking the iteration cycle and terminating cleanly.
---

## 📐 Conditional Logic Matrix & Flow Analysis

To properly evaluate data inputs, programs use conditional logic structures. Below is the mapping between human thought processes (Pseudo-code) and active Python implementation used to parse user interactions.

### 1. Pseudo-code Logic Framework
```text
If the input is less than 1 or greater than 20:
    Print "Out of range." (Stop and prompt again)
Else if the input is less than the target value:
    Print "Too low." (Stop and prompt again)
Else if the input is greater than the target value:
    Print "Too high." (Stop and prompt again)
Else:
    Print "Success/Correct Match."
```

### 2. Operational Execution Flow (`if / elif / else`)
When executing a sequential evaluation matrix, the interpreter drops to subsequent checks **only** if the prior conditions evaluate to `False`. The moment any statement matches as `True`, its nested block triggers and the rest of the conditional tree is skipped.

```python
if guess < 1 or guess > 20:
    print("That number is out of range. Try again.")
elif guess < secret:
    print("Too low, try again.")
elif guess > secret:
    print("Too high, try again.")
else:
    print("You got it in", tries, "tries!")
```

### 3. Comprehensive Numerical Use Cases

To visualize execution sequencing, consider a static environment where the targeted value is fixed (`secret = 10`):

*   **Scenario A: Out-of-Bounds Input (`guess = 30`)**
    *   *Evaluation:* The engine tests `30 < 1` (False) `or` `30 > 20` (True). Because the `or` condition requires only one matching parameter, the total test evaluates to **True**. The system prints the structural range warning and halts further evaluation steps.
*   **Scenario B: Lower Boundary Input (`guess = 5`)**
    *   *Evaluation:* The first boundary test resolves to False (`5` is neither `< 1` nor `> 20`). The script steps down to evaluate the first `elif` condition (`5 < 10`), which resolves to **True**. The system prints the "Too low" hint parameter.
*   **Scenario C: Upper Boundary Input (`guess = 15`)**
    *   *Evaluation:* The initial bounds check resolves to False. The next statement (`15 < 10`) resolves to False. The program drops down to test the second `elif` condition (`15 > 10`), which matches as **True**, printing the "Too high" hint parameter.
