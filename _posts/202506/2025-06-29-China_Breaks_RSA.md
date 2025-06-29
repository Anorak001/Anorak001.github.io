---  
title: China breaks RSA encryption with a quantum computer
description: >-
    China’s quantum computer cracks a small RSA key, signaling future risks to encryption.
author: anorak
date: 2025-06-29 04:30:00 +0530
categories: [NEWS, ISSUE]
tags: [Quantum-Computing, Cryptography, RSA, Cybersecurity]
pin: False
---   

  


## Overview

China’s quantum computer has successfully cracked a small RSA key, signaling future risks to encryption. This breakthrough demonstrates the growing capabilities of quantum hardware and the urgency for organizations to prepare for a post-quantum world.

  

## What Happened?

- **Researchers at Shanghai University** used a quantum annealing processor (built by D‑Wave Systems) to factor a 22‑bit RSA integer.
- This is the largest RSA key cracked on this class of quantum hardware, surpassing previous attempts that stopped at 19 bits.
- The experiment was led by Wang Chao and colleagues.

  

## Understanding the Key Concepts

### What is RSA Encryption?

- **RSA** is a widely used encryption algorithm introduced in 1977.
- Security relies on the difficulty of factoring large semiprime numbers (numbers with exactly two prime factors).
- **Classic computers** require sub-exponential time to break large RSA keys (e.g., 2048 bits), making them secure for now.

### What is Quantum Annealing?

- **Quantum annealing** is a quantum computing method designed to solve optimization problems by finding the lowest energy state of a system.
- D‑Wave’s quantum annealers use thousands of qubits and operate at extremely low temperatures (about 15 mK).
- Unlike universal quantum computers, annealers are specialized for certain problem types.

### What is a Qubit?

- A **qubit** is the basic unit of quantum information, analogous to a bit in classical computing.
- Qubits can exist in multiple states simultaneously (superposition), enabling quantum computers to process complex problems more efficiently.

### What is the Ising Model?

- The **Ising model** is a mathematical model used in physics and quantum computing to describe interactions between particles (or qubits).
- In this context, it helps translate factoring problems into a form that quantum annealers can solve.

  

## Key Findings

- The team factored a 22-bit RSA integer by:
    - Translating the problem into a **Quadratic Unconstrained Binary Optimization (QUBO)** problem.
    - Using the D‑Wave Advantage system to solve it.
- The approach reduced noise and improved accuracy by adjusting parameters in the Ising model.
- The same method was applied to other ciphers (Present and Rectangle), marking the first substantial threat to multiple full-scale SPN (Substitution–Permutation Network) algorithms.

  

## Why Does This Matter?

- **22-bit keys are not used in practice**—modern RSA keys are 2048 bits or larger.
- However, this experiment shows that quantum hardware is improving and could eventually threaten real-world encryption.
- Each hardware upgrade (e.g., D‑Wave’s upcoming Zephyr-topology processor with 7000+ qubits) brings us closer to breaking larger keys.

  

## Quantum vs. Classical Attacks

- **Classical computers** have only managed to break RSA keys up to 829 bits (RSA-250) after weeks of computation.
- **Quantum computers** can, in theory, break RSA much faster using algorithms like Shor’s algorithm, but current universal quantum computers are limited by error correction and qubit count.
- **Quantum annealing** offers a different approach, reframing factoring as an optimization problem rather than using Shor’s algorithm.

  

## What is Shor’s Algorithm?

- **Shor’s algorithm** is a quantum algorithm that can factor large numbers exponentially faster than classical algorithms.
- It requires universal, gate-based quantum computers, which are still in early development stages.

  

## Implications for Cybersecurity

- **Post-quantum cryptography** is becoming urgent:
    - In August 2024, NIST released the first federal standards (FIPS 203, 204, 205) for post-quantum cryptography.
    - In March 2025, HQC was selected for the next wave of standards.
- **Businesses and organizations** should:
    - Audit their cryptographic systems for vulnerable algorithms (RSA, ECC).
    - Begin testing and deploying quantum-safe libraries (e.g., Open Quantum Safe).
    - Implement hybrid key exchange methods and crypto-agility (the ability to swap algorithms easily).

  

## What Should Organizations Do?

- **Start with an internal audit** to identify all uses of RSA and other vulnerable algorithms.
- **Plan for migration** to quantum-safe cryptography, even if full migration will take years.
- **Adopt hybrid schemes** that combine classical and quantum-safe algorithms for added security.
- **Prioritize sensitive data** (health records, diplomatic cables, etc.) for early migration.

  

## Limitations and Next Steps

- The Shanghai result required significant classical pre- and post-processing, and many runs to find the correct factors.
- Larger keys remain secure for now, but history shows that cryptanalytic breakthroughs can scale quickly.
- Continued hardware improvements and smarter algorithms will keep pushing the boundaries.

  

## Further Reading

- The study is published in the [Chinese journal of Computers](https://cirics.uqo.ca/chinese-researchers-break-rsa-encryption-with-a-quantum-computer/).

  

## Glossary

- **RSA**: An encryption algorithm based on the difficulty of factoring large numbers.
- **Quantum Annealing**: A quantum computing technique for solving optimization problems.
- **Qubit**: The quantum equivalent of a classical bit.
- **Ising Model**: A mathematical model for describing interactions in quantum systems.
- **Shor’s Algorithm**: A quantum algorithm for factoring large numbers efficiently.
- **Crypto-agility**: The ability to quickly switch cryptographic algorithms in a system.
