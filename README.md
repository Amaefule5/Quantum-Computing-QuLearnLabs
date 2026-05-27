# 🔬 Quantum Capstone Project: Grover's Search Algorithm

> **A complete, modular implementation of Grover's quantum search algorithm using Qiskit 2.0**
>
> Built as a capstone project for the QuLearnLabs AI-SEQ Course 2026  
> Author: Amaefule

---

## 📖 Table of Contents

- [What is Grover's Algorithm?](#what-is-grovers-algorithm)
- [Project Overview](#project-overview)
- [Live Demo Results](#live-demo-results)
- [Project Structure](#project-structure)
- [Quick Start Guide](#quick-start-guide)
- [How It Works](#how-it-works)
- [Test Results](#test-results)
- [Key Concepts](#key-concepts)
- [Module Breakdown](#module-breakdown)
- [Troubleshooting](#troubleshooting)
- [License](#license)

---

## 🎯 What is Grover's Algorithm?

Grover's algorithm is one of the most important quantum algorithms, providing a **quadratic speedup** for searching unsorted databases.

| Approach | 4 Qubits (N=16 states) | Time Complexity |
|----------|------------------------|-----------------|
| **Classical** | Up to 16 checks | O(N) |
| **Grover's Quantum** | ~3 iterations | O(√N) |

For a database of size N = 2ⁿ:
- Classical search needs up to N checks in the worst case
- Grover's algorithm needs only about √(N) iterations
- For 4 qubits (16 states): **3 iterations vs. 16 checks** = **~5x speedup**

> 💡 **Real-world impact**: While the speedup is "only" quadratic (not exponential like Shor's algorithm), Grover's algorithm is broadly applicable to ANY search problem and is provably optimal for unstructured quantum search.

---

## 🚀 Project Overview

This project implements Grover's algorithm from scratch with:

✅ **2-qubit circuit** — Finds |11⟩ with **100% probability** (1 iteration)  
✅ **4-qubit circuit** — Finds any 4-bit solution with **~96% probability** (3 iterations)  
✅ **Modular architecture** — Clean separation of concerns across 6 source modules  
✅ **Comprehensive test suite** — 33 unit tests covering all 16 possible solutions  
✅ **Statevector & measurement simulation** — Exact probabilities + realistic shot-based stats  
✅ **Visualization tools** — Text-based circuit diagrams and probability histograms  

### Tech Stack
- **Qiskit 2.0** — Quantum circuit framework
- **Qiskit Aer** — High-performance quantum simulator
- **NumPy ≥2.2.6** — Numerical computing
- **Matplotlib ≥3.10.9** — Circuit visualization
- **Python 3.11–3.14** compatible

---

## 🎬 Live Demo Results

### 2-Qubit Grover's Algorithm (Solution: |11⟩)

```
============================================================
  2-QUBIT GROVER'S ALGORITHM
  Solution: |11>
============================================================

[STEP 1] Building circuit...
==================================================
  CIRCUIT INFORMATION
==================================================
  Number of qubits: 2
  Number of classical bits: 0
  Total operations: 12
  Circuit depth: 7

  Gate breakdown:
    cz: 2
    h: 6
    x: 4
==================================================

     +---+   +---++---+   +---++---+
q_0: + H +-*-+ H ++ X +-*-+ X ++ H +
     +---+ | +---++---+ | +---++---+
q_1: + H +-*-+ H ++ X +-*-+ X ++ H +
     +---+   +---++---+   +---++---+

[STEP 2] Statevector simulation (exact probabilities):

  Statevector Probabilities (2 qubits, 4 states):
--------------------------------------------------
|00>: 0.0000 
|01>: 0.0000 
|10>: 0.0000 
|11>: 1.0000 ####################################### <-- SOLUTION
--------------------------------------------------

  Solution |11> probability: 100.0%
  Expected: 100%
  Status: ✅ PASS
```

**Why 100%?** For N=4 states, 1 iteration is *exactly* optimal. The math works out perfectly — a special case that doesn't generalize to larger systems.

---

### 4-Qubit Grover's Algorithm (Solution: |0010⟩)

```
============================================================
  4-QUBIT GROVER'S ALGORITHM
  Solution: |0010>
============================================================

[STEP 1] Building circuit for |0010>...
==================================================
  CIRCUIT INFORMATION
==================================================
  Number of qubits: 4
  Number of classical bits: 0
  Total operations: 88
  Circuit depth: 37

  Gate breakdown:
    h: 40
    mcx: 6
    x: 42
==================================================
  Expected statevector index: 4
  Optimal iterations: 3

[STEP 2] Circuit diagram:
  [Circuit has too many gates for terminal display]
  Use: qc.draw(output='mpl', filename='circuit.png') for image

[STEP 3] Statevector simulation (exact probabilities):

  Statevector Probabilities (4 qubits, 16 states):
--------------------------------------------------
|0000>: 0.0026 
|0001>: 0.0026 
|0010>: 0.0026 
|0011>: 0.0026 
|0100>: 0.9613 ###################################### <-- SOLUTION
|0101>: 0.0026 
|0110>: 0.0026 
|0111>: 0.0026 
|1000>: 0.0026 
|1001>: 0.0026 
|1010>: 0.0026 
|1011>: 0.0026 
|1100>: 0.0026 
|1101>: 0.0026 
|1110>: 0.0026 
|1111>: 0.0026 
--------------------------------------------------

  Solution |0010> probability: 96.1%
  Maximum probability state: |0100>
  Expected: |0010> (index 4)
  Status: ✅ PASS

[STEP 4] Measurement simulation (1024 shots):

   Top measurement results:
    |0100>:  985 shots ( 96.2%) <- SOLUTION!
    |1011>:    5 shots (  0.5%)
    |1111>:    4 shots (  0.4%)
    |0110>:    4 shots (  0.4%)
    |0111>:    4 shots (  0.4%)

  Most frequent result: |0100>
  Status: ✅ PASS
```

**Why ~96% and not 100%?** The optimal iteration count is π/4 × √16 ≈ 3.14, but we can only do integer iterations. 3 iterations gives ~96.1% — close to optimal. Doing 4 iterations would *overshoot* and decrease the probability!

---

### Multi-Solution Test Summary

All 16 possible 4-bit solutions tested:

```
============================================================
  TEST SUMMARY
============================================================

   Testing multiple solutions:
  Solution   Index    Probability     Status
  --------------------------------------------------
  |0000>   0          96.1%        ✅ PASS
  |1111>   15         96.1%        ✅ PASS
  |0010>   4          96.1%        ✅ PASS
  |1000>   1          96.1%        ✅ PASS
  |0101>   10         96.1%        ✅ PASS
  |1010>   5          96.1%        ✅ PASS

   All solutions tested!
```

---

## 📁 Project Structure

```
quantum-capstone/
├── README.md                 # This file
├── requirements.txt          # Python dependencies
├── main.py                   # Entry point - runs full demonstration
├── simulation.py             # Simulation & visualization utilities
│
├── src/                      # Source code modules
│   ├── __init__.py
│   ├── utils.py              # Validation, calculations, helpers
│   ├── gates.py              # Quantum gate constructions (H, X, MCZ)
│   ├── oracle.py             # Oracle implementations (mark solutions)
│   ├── diffusion.py          # Diffusion operators (amplify solutions)
│   ├── grover.py             # Main algorithm assembly
│   └── circuits.py           # Visualization and measurement tools
│
└── tests/                    # Comprehensive test suite
    ├── __init__.py
    ├── test_two_qubit.py     # 2-qubit algorithm tests
    ├── test_four_qubit.py    # 4-qubit algorithm tests
    ├── test_gates.py         # Individual gate tests
    └── test_grover.py        # Full algorithm correctness tests
```

---

## ⚡ Quick Start Guide

### Prerequisites
- Python 3.11, 3.12, 3.13, or 3.14
- pip package manager

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/YOUR_USERNAME/quantum-capstone.git
cd quantum-capstone

# 2. Create virtual environment
python -m venv .venv

# 3. Activate (Windows)
.venv\Scripts\activate

# 4. Activate (macOS/Linux)
# source .venv/bin/activate

# 5. Install dependencies
pip install -r requirements.txt
```

### Running the Project

```bash
# Run the full demonstration
python main.py

# Run all unit tests
python -m unittest discover tests/ -v

# Run specific test files
python -m unittest tests.test_two_qubit -v
python -m unittest tests.test_four_qubit -v
python -m unittest tests.test_gates -v

# Run individual module self-tests
python src/utils.py
python src/gates.py
python src/oracle.py
python src/diffusion.py
python src/grover.py
python src/circuits.py
```

### Visualizing Circuits

```bash
# Save circuit as image (requires matplotlib)
python -c "from src.grover import four_qubit_grover; \
         qc = four_qubit_grover('0010'); \
         qc.draw(output='mpl', filename='circuit.png')"
```

---

## 🧠 How It Works

### The Complete Algorithm

```
┌─────────────┐     ┌─────────┐     ┌───────────┐
│ Initialize  │────→│ Oracle  │────→│ Diffusion │
│  H⊗n|0...0> │     │  Phase  │     │ Amplify   │
└─────────────┘     │  Flip   │     │ Solution  │
                    └─────────┘     └───────────┘
                          ↑                │
                          └────────────────┘
                              Repeat k times
```

### Step-by-Step Breakdown

#### 1. **Initialization** — Create Uniform Superposition
```python
qc.h(range(num_qubits))  # Apply H to all qubits
```
- Transforms |0...0⟩ into an equal superposition of all N = 2ⁿ states
- Every state has amplitude 1/√N

#### 2. **Oracle** — Tag the Solution
```python
# Flip qubits that are '0' in solution
for i in range(num_qubits):
    if solution[i] == "0":
        circuit.x(i)

# Apply multi-controlled-Z (tags |11...1⟩)
apply_multi_controlled_z(circuit, controls, target)

# Unflip to restore original state (now with -1 phase)
for i in range(num_qubits):
    if solution[i] == "0":
        circuit.x(i)
```
- Applies a **-1 phase flip** to exactly ONE state (the solution)
- Uses the **H-MCX-H trick**: MCZ = H(target) × MCX(controls, target) × H(target)
- All other states remain unchanged

#### 3. **Diffusion** — Amplify the Solution
```python
# Inversion about the average
apply_hadamard_all(circuit)      # H on all
apply_pauli_x_all(circuit)       # X on all
apply_multi_controlled_z(...)    # Phase flip |11...1⟩
apply_pauli_x_all(circuit)       # X on all (undo)
apply_hadamard_all(circuit)      # H on all (undo)
```
- Reflects all amplitudes about the average
- The negative solution amplitude becomes **more positive** (amplified!)
- Non-solutions get pushed down (attenuated)

#### 4. **Optimal Iterations**
```
k_opt = floor(π/4 × √N)
```
| Qubits | N = 2ⁿ | Optimal k | Success Probability |
|--------|--------|-----------|---------------------|
| 2 | 4 | 1 | **100%** |
| 3 | 8 | 2 | ~95% |
| 4 | 16 | **3** | **~96%** |
| 5 | 32 | 4 | ~97% |

> ⚠️ **Critical**: Too few iterations = low probability. Too many = probability *decreases* again! It's like a pendulum swinging past the highest point.

---

## ✅ Test Results

### Full Test Suite: 33/33 Passing

```bash
$ python -m unittest discover tests/ -v

Ran 33 tests in 1.411s
OK
```

### Test Coverage

| Test File | Tests | What It Verifies |
|-----------|-------|------------------|
| `test_two_qubit.py` | 5 | 100% probability for \|11⟩, correct gate counts, allowed gates only |
| `test_four_qubit.py` | 6 | All 16 solutions ≥93% probability, only H/X/MCX gates, optimal iterations |
| `test_gates.py` | 14 | Individual gate correctness (H, X, MCZ, oracle marker, diffusion) |
| `test_grover.py` | 8 | Full algorithm correctness, bit ordering, invalid input handling |

### Key Test Highlights

```bash
$ python -m unittest tests.test_four_qubit -v

test_all_solutions_have_high_probability ... ok
test_circuit_has_four_qubits ... ok
test_circuit_uses_only_allowed_gates ... ok
test_invalid_solution_strings_rejected ... ok
test_iteration_count_is_optimal ... ok
test_specific_solution_0010 ... ok

Ran 6 tests in 0.389s
OK
```

---

## 🔑 Key Concepts (For Beginners)

### Superposition
Qubits can exist in multiple states simultaneously. A 4-qubit system exists in a superposition of all 16 possible states at once — this is the "quantum parallelism" that makes Grover's algorithm powerful.

### Oracle
Think of the oracle as a "checker function" that knows the answer but can only give a subtle hint. It tags the solution with a negative sign (-1 phase) without revealing which state it is.

### Diffusion (Amplification)
After the oracle tags the solution, the diffusion operator acts like a "mirror" that reflects all states about their average. Since the solution is now negative (below average), the reflection pushes it UP while pushing everything else DOWN.

### Interference
Quantum interference is what makes this work. The oracle creates a phase difference, and the diffusion operator uses constructive interference to amplify the solution and destructive interference to suppress non-solutions.

### Qiskit Bit Ordering
> **The #1 source of confusion!** Qiskit uses LSB (Least Significant Bit) ordering:
> - q₀ is the **rightmost** bit in the state label
> - Solution string `"0010"` means q₃=0, q₂=0, q₁=1, q₀=0
> - Statevector index = reverse("0010") = "0100" = **4**

---

## 🧩 Module Breakdown

| Module | Purpose | Key Functions |
|--------|---------|---------------|
| `utils.py` | Validation & math helpers | `validate_solution_string()`, `calculate_optimal_iterations()` |
| `gates.py` | Reusable quantum gates | `apply_hadamard_all()`, `apply_multi_controlled_z()`, `apply_oracle_marker()` |
| `oracle.py` | Oracle implementations | `two_qubit_oracle_11()`, `four_qubit_oracle()` |
| `diffusion.py` | Diffusion operators | `two_qubit_diffusion()`, `four_qubit_diffusion()` |
| `grover.py` | Algorithm assembly | `two_qubit_grover_11()`, `four_qubit_grover()`, `create_grover_circuit()` |
| `circuits.py` | Visualization & measurement | `add_measurement()`, `draw_circuit_text()`, `print_circuit_info()` |
| `simulation.py` | Simulation utilities | `simulate_statevector()`, `simulate_aer()`, `verify_solution()` |

---

## 🛠️ Troubleshooting

| Issue | Solution |
|-------|----------|
| `SyntaxError: invalid syntax` in print statements | The code uses actual newlines inside quotes. Ensure `\n` is used instead, or run `main.py` which handles this correctly |
| `ImportError: attempted relative import` | Run modules through `main.py` or use `python -m src.module_name` |
| `MissingOptionalLibraryError: pylatexenc` | Install: `pip install pylatexenc` (needed for matplotlib circuit drawing) |
| `qiskit-aer not installed` | Install: `pip install qiskit-aer` (needed for measurement simulation) |
| NumPy/matplotlib wheel build errors on Python 3.14 | Use Python 3.11–3.13, or ensure you have `numpy>=2.2.6` and `matplotlib>=3.10.9` |

---

## 📚 Further Reading

- [Qiskit Documentation](https://docs.quantum.ibm.com/)
- [Grover's Algorithm — Qiskit Textbook](https://qiskit.org/textbook/ch-algorithms/grover.html)
- [Original Paper: Lov Grover (1996)](https://arxiv.org/abs/quant-ph/9605043)

---

## 📝 License

**QuLearnLabs AI-SEQ Course 2026**  
Amaefule Capstone Project

---

> *"Quantum computing is not just faster computing — it's a fundamentally different way of processing information, and Grover's algorithm is the perfect introduction to that paradigm shift."*
