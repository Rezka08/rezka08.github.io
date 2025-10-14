---
title: "Introduction to Unit Testing"
date: 2025-10-12 13:00:00
categories: [Software Testing and Quality Assurance]
tags: [STQA]
image: /assets/images/STQA/pengantar-unit-testing.png
---

## Introduction to Unit Testing

Unit testing is the practice of verifying the smallest testable parts of an application — functions, methods, or classes — in isolation. Unit tests are typically written and run by developers to catch bugs early, document expected behavior, and make refactoring safer and faster. The slides provided by Group 5 highlight the core ideas, common frameworks, and the Arrange–Act–Assert pattern used in unit tests.

---

## Why unit testing matters

- **Catch bugs early:** defects found during unit testing are cheaper and faster to fix than those found later.  
- **Improve code quality:** tests force clearer APIs and encourage modular design.  
- **Enable safe refactoring:** a good test suite lets you change implementation with confidence.  
- **Serve as living documentation:** tests show how code is supposed to behave.

---

## The AAA pattern (Arrange — Act — Assert)

A common structure for unit tests is AAA:

1. **Arrange:** set up initial conditions and inputs (create objects, set state).  
2. **Act:** execute the function or method under test.  
3. **Assert:** verify the outputs or side-effects match expectations.

This pattern keeps tests clear and maintainable.

---

## Popular unit testing frameworks (by ecosystem)

- **JUnit 5** — Java (and JVM languages). Mature, annotation-driven, widely used.  
- **pytest** — Python. Minimal boilerplate, powerful fixtures, great error reporting.  
- **Jest** — JavaScript/TypeScript. Zero-config for many setups, snapshot testing included.  

Group 5’s slides cover these frameworks and show simple live-coding examples for `pytest` and `JUnit 5`.

---

## Quick best practices

- Keep tests **small** and **focused** (one assertion target per test when sensible).  
- Make tests **deterministic** — avoid randomness or reliance on external state.  
- **Isolate** external dependencies (use mocks/stubs for databases, network, file I/O).  
- Prefer **fast** tests — slow tests discourage frequent runs. Run slow/integration tests separately.  
- Give tests **descriptive names** and include clear failure messages.  
- Run tests automatically in CI on every push/PR.

[View Group 5 Presentation Slides](https://drive.google.com/file/d/1qSDpXubcQlTiRXuM2tdDO30rPo-3GkCS/view?usp=sharing)

---

## Short runnable examples

Below are minimal examples you can paste into your project and run. They demonstrate AAA and are intentionally small so you can try them quickly.

### Python — `pytest` (example: BankAccount)

`bank_account.py`
```python
class BankAccount:
    def __init__(self, initial=0):
        self.balance = initial

    def deposit(self, amount):
        if amount < 0:
            raise ValueError("amount must be non-negative")
        self.balance += amount

    def withdraw(self, amount):
        if amount > self.balance:
            raise ValueError("insufficient funds")
        self.balance -= amount
```

`test_bank_account.py`
```python
import pytest
from bank_account import BankAccount

def test_deposit_increases_balance():
    # Arrange
    acc = BankAccount(initial=100)
    # Act
    acc.deposit(50)
    # Assert
    assert acc.balance == 150

def test_withdraw_raises_on_insufficient_funds():
    acc = BankAccount(initial=30)
    with pytest.raises(ValueError):
        acc.withdraw(100)
```

#### Run
```bash
pytest -q
```

---

### Java — JUnit 5 (example: Calculator)

`src/main/java/com/example/Calculator.java`
```java
package com.example;

public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
}
```

`src/test/java/com/example/CalculatorTest.java`
```java
package com.example;

import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;

class CalculatorTest {
    @Test
    void addsTwoNumbers() {
        // Arrange
        Calculator calc = new Calculator();
        // Act
        int result = calc.add(2, 3);
        // Assert
        assertEquals(5, result);
    }
}
```

#### Run (maven):
```bash
mvn test
```

#### Run (gradle):
```bash
./gradlew test
```

---

### JavaScript — Jest (example: util function)
`utils.js`
```java
function add(a, b) {
  return a + b;
}
module.exports = add;
```

`utils.test.js`
```java
const add = require("./utils");

test("adds two numbers", () => {
  // Arrange & Act & Assert (small example)
  expect(add(1, 2)).toBe(3);
});
```

#### Run
```bash
# install jest first if needed
npm init -y
npm install --save-dev jest
# then in package.json set "test": "jest"
npm test
```

### From live-coding to real projects
The Group 5 materials show live-coding examples of unit tests for small apps (e.g., Shopping Cart, BankAccount) — use those as templates for your project tests. Start by writing tests for the core logic (calculations, business rules), then add tests for edge cases and invalid inputs.