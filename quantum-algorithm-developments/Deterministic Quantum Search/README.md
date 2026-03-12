# Deterministic Quantum Search - OpenQASM Implementations

## Overview
This repository contains the OpenQASM source code for the circuit-level examples demonstrated in Section 5 of the paper **"Deterministic Quantum Search for Arbitrary Initial Success Probabilities"**. 

Standard quantum search algorithms are inherently probabilistic and offer no quantum advantage when the initial success probability exceeds 0.5. The code in this repository demonstrates a novel deterministic approach that introduces an external control mechanism via an ancilla qubit. This guarantees the successful identification of target states with 100% certainty across arbitrary initial success probabilities.

## Repository Contents

This repository includes two OpenQASM files corresponding to the specific examples outlined in the manuscript:

### 1. `example_1.qasm`
* **Description:** Demonstrates the deterministic search when the initial success probability is less than 0.5.
* **Search Space:** Uniform superposition of 8 states.
* **Target States:** `|101>`, `|110>`, and `|111>`.
* **Mechanism:** The ancilla qubit is initialized using an Ry rotation gate with an angle of approximately 1.2309 radians. A modified oracle then marks the combined states by adding a phase of π, allowing the diffusion operator to amplify the target states with complete certainty.

### 2. `example_2.qasm`
* **Description:** Demonstrates the deterministic search maintaining a quantum advantage even when the initial success probability exceeds 0.5.
* **Search Space:** Uniform superposition of 8 states.
* **Target States:** `|000>`, `|001>`, `|011>`, `|101>`, and `|111>`.
* **Mechanism:** With an initial success probability of 5/8 (0.625), standard algorithms fail to provide an advantage. By initializing the ancilla qubit with an appropriate Ry rotation gate and applying the controlled oracle and diffusion operators, this circuit demonstrates 100% accuracy in measuring the target states.

## Running the Code

These OpenQASM files represent standard quantum circuits and can be executed on various quantum simulators and platforms. They are fully compatible with environments like **Qiskit** and can be seamlessly imported and evaluated on **Qniverse: A Unified Quantum Computing Platform**.
