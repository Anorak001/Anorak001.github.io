---
title: Fuzzing For NOOBS
description: >-
 Detailed article on the fuzzing and its techniques
author: anorak
date: 2024-12-22 00:30:00 +0530
categories: [GUIDE,CYBERSECURITY]
tags: [Cybersecurity,  fuzzing, Vulnerabilities, ]
pin: true
---
# Fuzzing 101

**Quality assurance technique for the good folks, and a Goldmine of zero-day vulnerabilities for attackers**

## What is Fuzzing?

Fuzzing is the art of automatic bug detection. It is an automated process ideal for situations when you don’t know exactly what you’re looking for. It involves rapidly throwing invalid, unexpected, or random data as inputs to a program. The goal is to stress-test the program and intentionally cause crashes to reveal hidden bugs.

It’s a dream for vulnerability researchers and bug hunters because you not only find bugs but also understand what caused them and sometimes even how to exploit them.

However, most fuzzers nowadays are only effective in spotting vulnerabilities related to system crashes; for other kinds of vulnerabilities, they are almost useless.

In this blog, we will cover the basics of fuzz testing concepts and tools.

---

## What is the difference between fuzzing and normal testing?

**User testing**, or normal testing, ensures the software is user-friendly and meets user expectations. Test cases include scenarios that users may encounter when using the program (valid input). The main goal is to ensure that situations resulting in errors are handled gracefully by the program.

**Fuzz testing**, however, focuses on identifying security vulnerabilities and weaknesses in the software’s implementation. Fuzz test cases involve providing random or semi-random inputs to the program (invalid inputs) to test its robustness and response to unexpected inputs.

```python
# Divides two numbers and returns the result

def divide_numbers(a, b):
    try:
        result = a / b
    except ZeroDivisionError:
        result = None
    return result
```

In the above code:
- **User testing** may include testing for:
  - Division by zero
  - Floating-point division
  - Division of negative numbers

```python
# Division by zero
divide_numbers(10, 0)

# Floating-point division
divide_numbers(5.5, 2)

# Division of negative numbers
divide_numbers(-10, 2)
```

- **Fuzz testing** involves testing invalid cases, such as:
  - Random integer or floating-point values
  - Negative and zero values combined
  - Completely invalid input such as text

```python
# Random integer/floating-point values
divide_numbers(987654321, 123)
divide_numbers(3.14, 2.71828)

# Negative and zero values combined
divide_numbers(0, -5)

# Invalid input (nonsensical input)
divide_numbers("fff", 3)
divide_numbers(9, "ves")
divide_numbers(None, 1)
```

---

## Fuzzers

How do we perform fuzz testing? One of the great advantages of fuzz testing is its automation. To perform fuzzing, we create a script to "fuzz" the program of our choice. This script, called the "fuzzer," generates multiple test cases (inputs) and feeds them into the program while monitoring for crashes.

### Components of a Fuzzer

1. **Mutator**
2. **Instrumentation**
3. **Monitor**
4. **Crash Logger**

Let’s explore each component in more detail.

---

### Fuzzers: Mutator

The primary objective of a fuzzer is to generate several test cases, feed them to a target, and watch for crashes.

#### Approaches for Developing Test Cases:

1. **Mutation-based test cases**:
Mutation fuzzing, also known as "dumb" or "random" fuzzing, randomly mutates a supplied seed input to generate large numbers of unusual inputs. This approach is suitable for unstructured input, such as strings.

**Example:**

- **Original Input**: "Hello, world!"
- **Test Cases**:
  - "Hollo, world!"
  - "Hello, worl!"
  - "Hello, world!!"
  - "Hell, world!"

Tools like **Radamsa** require minimal work but are unlikely to find new paths.

2. **Generation-based test cases**:
Generation fuzzing, also known as "smart" or "guided" fuzzing, builds test cases based on the program’s expected input format using a grammar file. This approach is suitable for structured input, such as PDFs or network packets.

**Example:**

- **Original Input:**
  - Timestamp: "2023-09-09 15:45:23"
  - Source IP: "192.168.1.100"
  - Destination IP: "10.0.0.1"
  - Source Port: "5000"
  - Destination Port: "6000"
  - Packet Length: "64 bytes"
  - Payload: "[hexadecimal or textual representation of packet payload]"

- **Test Case 1:**
  - Timestamp: "2023-04-31 15:45:23"   — Invalid: April has only 30 days
  - Source Port: "12345"              — Invalid: Random port
  - Destination Port: "54321"         — Invalid: Random port

Tools like **Dharma** require more work but are more likely to find new paths.

---

### Fuzzers: Instrumentation

Instrumentation tracks code coverage by modifying or adding code to the software being tested. This helps determine which portions of the code are untested by the input, allowing us to adjust the input.

**Example:**

A simple calculator program with three operations:

```python
import sys

if len(sys.argv) != 4:
    print("Usage: python filename.py <operation> <num1> <num2>")
    sys.exit(1)

choice = int(sys.argv[1])
num1 = int(sys.argv[2])
num2 = int(sys.argv[3])

if choice == 1:
    print(f"{num1} + {num2} = {num1 + num2}")
elif choice == 2:
    print(f"{num1} - {num2} = {num1 - num2}")
elif choice == 3:
    if num2 != 0:
        print(f"{num1} / {num2} = {num1 / num2:.2f}")
    else:
        print("Error: Division by zero.")
sys.exit(0)
```

Using instrumentation, we can log the code coverage paths taken by test cases to ensure all paths are tested.

---

### Fuzzers: Monitor

Monitoring involves understanding the expected program behavior to detect anomalies caused by fuzzing inputs. Tools like **WinDbg** or libraries like **pykd** can be used to monitor program behavior and gather data such as:

- List of loaded modules
- Current register values
- Current call stack
- Disassembly instructions
- Crash analysis results

---

### Fuzzers: Crash Logger

Crash logging captures and records information about exceptions during fuzzing, such as:

1. Access Violation   (0xC0000005)
2. Division by Zero   (0xC0000094)
3. Stack Overflow     (0xC00000FD)
4. Integer Overflow   (0xC0000095)
5. Heap Corruption    (0xC0000374)

By overriding the **ExceptionHandler** class, we can classify and log crashes for analysis.

**Example:**

```python
import pykd

class ExceptionHandler(pykd.eventHandler):
    def __init__(self):
        pykd.eventHandler.__init__(self)
        self.accessViolationOccured = False

    def onException(self, exceptInfo):
        if exceptInfo.exceptionCode == 0xC0000005:  # Access Violation
            self.accessViolationOccured = True
        return pykd.eventResult.NoChange
```

To avoid duplications, compare major and minor hashes of crashes before logging.

---

## Fuzzing: Basic Workflow

1. Choose an attack surface.
2. Create or find test cases.
3. Use mutation or generation techniques.
4. Fuzz the program with test cases.
5. Monitor and log crashes.
6. Analyze and triage crashes.

---

## Fuzz Testing Best Practices

1. **Improve Testing Speed**: Use parallel fuzzing.
2. **Reduce Test Cases**: Use inputs with higher code coverage.
3. **Track Code Coverage**: Employ efficient instrumentation techniques.
4. **Efficient Crash Categorization**: Know crash types to speed up analysis.

---

By understanding these fundamental concepts, you can leverage fuzz testing to uncover vulnerabilities and improve software robustness effectively.

