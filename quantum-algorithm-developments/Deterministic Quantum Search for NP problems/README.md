# Deterministic Quantum Search for N-SAT

## Overview

This repository contains a Jupyter notebook implementing an ancilla-assisted deterministic quantum search procedure for N-SAT instances. The implementation constructs a quantum oracle from the clauses of a Boolean formula in conjunctive normal form (CNF). Each clause is evaluated reversibly using a workspace qubit, and the clause results are combined to mark satisfying assignments. The workspace is then uncomputed so that it can be reused in subsequent search iterations.

The search procedure prepares a uniform superposition over candidate assignments and calibrates the initial success probability through a rotation of an auxiliary qubit. The calibrated oracle and diffusion operations are repeated for the required number of iterations. The final probabilities are obtained from the simulated quantum state, rather than from measurement-shot frequencies.

The notebook includes 3-SAT and 4-SAT examples with four or five variables, covering instances with one or two satisfying assignments. The examples illustrate deterministic amplification when the number of satisfying assignments is known.

## Repository Contents

- **`N_SAT_Deterministic_(3) (3).ipynb`**: Main notebook containing the oracle construction, deterministic search implementation, state-vector simulation, and example N-SAT instances.
- **`README.md`**: Overview, dependencies, and instructions for running the notebook.

The notebook defines the following functions:

| Function | Purpose |
| --- | --- |
| `single_oracle` | Reversibly evaluates an individual CNF clause. |
| `oracle` | Combines clause evaluations, applies the oracle marking operation, and uncomputes the clause workspace. |
| `diffuser` | Implements the diffusion operation used in the search. |
| `grover` | Repeats the calibrated oracle and diffusion operations. |
| `get_theta` | Computes the required iteration count and auxiliary-qubit rotation angle from the number of solutions. |
| `create_nsat_circuit` | Builds the circuit, runs the simulator, and returns the rotation angle, iteration count, and state probabilities. |
| `get_nonzero_states` | Extracts nonzero state probabilities and optionally plots them. |

## Requirements

- Python 3 (the uploaded notebook was executed using Python 3.13)
- Jupyter Notebook or JupyterLab
- Qiskit
- Qiskit Aer
- NumPy
- Matplotlib
- `pylatexenc` for circuit drawings with the Matplotlib drawer

Install the dependencies in the Python environment used by Jupyter:

```bash
python -m pip install jupyterlab qiskit qiskit-aer numpy matplotlib pylatexenc
```

The first notebook cell also installs `qiskit`, `qiskit_aer`, and `pylatexenc` if they are not already available.

## Running the Code

1. Clone or download the repository and open a terminal in the repository directory.
2. Install the dependencies listed above.
3. Start JupyterLab and open the notebook:

   ```bash
   jupyter lab "N_SAT_Deterministic_(3) (3).ipynb"
   ```

4. Run the cells in order, starting with the imports and function definitions. The later cells define individual N-SAT instances and execute their simulations.
5. Inspect the printed rotation angle, iteration count, and dictionary of nonzero state probabilities. The four-variable, unique-solution 4-SAT example also displays a probability plot.

To evaluate another instance, define its CNF clauses as lists of signed integers, set `n` to the number of Boolean variables, and set `n_sol` to the **actual number of satisfying assignments**. A positive integer denotes the corresponding variable, and a negative integer denotes its negation. Then execute:

```python
theta, n_iterations, probabilities = create_nsat_circuit(n, n_sol, clauses)
print(theta, n_iterations, get_nonzero_states(n, probabilities, plot=True))
```

The rotation angle `theta` is expressed in radians. State labels follow Qiskit's bit-order convention, with the first variable displayed as the rightmost bit.

### Notes on the Results

The simulation saves and reads the quantum state using Qiskit Aer. Although the backend is called with `shots=1`, the displayed probabilities are calculated from the saved statevector, not estimated from one measurement shot. The exact calibration requires the number of satisfying assignments to be known in advance.

The early two-clause demonstration sets `n_sol = 1`, but its CNF formula actually has **14 satisfying assignments**. That preliminary cell should not be used as a validated deterministic-search result without correcting `n_sol`. The subsequent displayed 3-SAT and 4-SAT examples use solution counts consistent with their clause sets.
