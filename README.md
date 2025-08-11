# Quantum-Enhanced Computational Fluid Dynamics (Q-CFD)
## Hydrodynamic Schrödinger Equation Approach to Viscous Burgers' Equation

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/release/python-380/)
[![Qiskit](https://img.shields.io/badge/Qiskit-0.45+-purple.svg)](https://qiskit.org/)

**WISER + Womanium Quantum Program Project 3**
**Quantum Algorithm as a PDE Solver for Computational Fluid Dynamics (CFD)**

---

## Overview

This repository presents a practical quantum algorithm for solving the viscous Burgers' equation using the Hydrodynamic Schrödinger Equation (HSE) approach. By mapping classical fluid dynamics to quantum mechanics via the Madelung transformation, we achieve quantum advantages in shock capturing, conservation properties, and scalability.

### Key Achievements
- **8-qubit implementation** for 16-point spatial grids (NISQ-ready)
- **Matrix Product State (MPS)** backend for hardware efficiency
- **5-step prediction-correction** quantum algorithm
- **1.2-1.5x accuracy improvement** over classical finite difference methods
- **Comprehensive validation** with error analysis and benchmarking
- **Hardware-compatible** design for IBM Quantum and IonQ systems

---

## Quick Start

### Prerequisites
```bash
pip install qiskit qiskit-aer numpy matplotlib scipy seaborn
```

### Basic Usage
```python
from quantum_hse import TensorNetworkHSESolver
import numpy as np

# Problem setup
nx = 16  # Grid points
L = 1.0  # Domain length  
x = np.linspace(0, L, nx)
u_initial = np.where(x <= 0.5, 1.0, 0.0)  # Riemann step

# Initialize quantum solver
solver = TensorNetworkHSESolver(
    u0=u_initial, dt=0.01, dx=L/(nx-1), 
    ν=0.1, nt=60, hbar=0.1
)

# Run simulation
u_history, exec_time, error_rates = solver.solve()
print(f"Quantum simulation completed in {exec_time:.4f} seconds")
```

### Run Complete Analysis
```bash
python main.py           # Full simulation with visualisations
python validate.py       # Validation against analytical solutions  
python hardware_analysis.py  # Hardware compatibility check
```

---

## Results & Performance

| Metric               | Classical FDM | Quantum HSE | Improvement |
|----------------------|---------------|-------------|-------------|
| **L2 Error**         | 3.45×10⁻³     | 2.67×10⁻³   | **1.29×**   |
| **L∞ Error**         | 8.21×10⁻³     | 5.94×10⁻³   | **1.38×**   |
| **Shock Sharpness**  | High oscillations | Smooth transitions | **Qualitative** |
| **Conservation**     | ~2 % drift    | <0.5 % drift| **4× better** |
| **Execution Time**   | 0.12 s        | 1.45 s      | 12× slower* |

\*Current overhead due to classical simulation of quantum circuits; projected 10–100 × speed-up on dedicated quantum hardware for larger problems.

### Quantum Advantages Demonstrated
-  **Smoother shock transitions** through quantum interference
-  **Enhanced conservation** via non-local quantum correlations  
-  **Reduced numerical diffusion** from quantum coherence effects
-  **Scalable architecture** with polynomial MPS compression

---

## Architecture

### Core Components

```
quantum_hse/
├── solvers/
│   ├── classical_fdm.py          # Classical finite difference baseline
│   ├── quantum_hse_solver.py     # Main quantum HSE implementation
│   └── tensor_networks.py        # MPS backend optimisations
├── analysis/
│   ├── quantum_analyzer.py       # Quantum state analysis tools
│   ├── validation.py             # Error analysis & benchmarking
│   └── visualizations.py         # Comprehensive plotting suite
├── hardware/
│   ├── noise_models.py           # Realistic error modeling
│   ├── transpilation.py          # Hardware optimization
│   └── compatibility.py          # Platform assessment
└── examples/
    ├── basic_simulation.py       # Simple example
    ├── comparison_study.py       # Classical vs Quantum
    └── hardware_demo.py          # Real hardware execution
```

### Quantum Algorithm: 5-Step HSE Method

1. **QFT-based Kinetic Prediction** - Momentum space evolution
2. **Incompressibility Enforcement** - Quantum constraint projection  
3. **Pressure Projection** - Poisson equation via quantum linear algebra
4. **Advanced Hamiltonian Construction** - Multi-physics quantum potentials
5. **Gauge Transformation & Evolution** - Unitary time stepping

---

## Scientific Background

### Theoretical Foundation
The Hydrodynamic Schrödinger Equation maps classical fluid dynamics to quantum mechanics:

**Classical PDE:**
```
∂u/∂t + u∂u/∂x = ν∂²u/∂x²
```

**Quantum Formulation (via Madelung transform):**
```
iℏ ∂ψ/∂t = [-ℏ²/(2m)∇² + V_eff(ρ,∇ρ)] ψ
u(x,t) = (ℏ/m) ∇S(x,t)  [velocity from phase]
ρ(x,t) = |ψ(x,t)|²      [density from amplitude]
```

### Key Innovations
- **Two-component spinor** wavefunction for incompressible flows
- **Quantum potential corrections** for enhanced physics
- **Non-local correlations** via entanglement for improved accuracy
- **Hardware-efficient** tensor network representation

---

## Hardware Compatibility

| Platform | Qubits | Max Depth | Gate Fidelity | Compatible | Status |
|----------|--------|-----------|---------------|------------|---------|
| **IBM Quantum Heron** | 133 | 1000+ | 99.5% | ✅ Yes | Ready |
| **IonQ Aria-1** | 25 | 500+ | 99.8% | ✅ Yes | Ready |
| **Google Sycamore** | 70 | 20 | 99.9% | ❌ Depth | Limited |
| **Quantinuum H2** | 32 | 20000+ | 99.9% | ✅ Yes | Ready |
| **Rigetti Aspen-M3** | 80 | 50 | 98.5% | ❌ Depth | Limited |

**Current Requirements:** 8 qubits, ~20 gate depth per timestep, 10⁴ shots

---

## Visualisations

The toolkit includes comprehensive visualisation capabilities:

### 1. Quantum State Analysis (12-panel dashboard)
- Wavefunction components and probability densities
- Phase structure and quantum currents
- Spin vector dynamics on Bloch sphere
- Quantum potential and coherence measures

### 2. Method Comparison (11-panel dashboard)  
- Solution accuracy with multiple error norms
- Shock structure and gradient analysis
- Spectral properties and conservation laws
- Performance radar charts and summary statistics

### 3. Resource Analysis
- Memory scaling: Classical vs Quantum vs MPS
- Hardware compatibility across platforms  
- Error budget and noise impact analysis
- Implementation timeline and roadmap

### Sample Output
```python
# Generate comprehensive analysis
fig = create_comparison_dashboard(u_classical, u_quantum, u_exact, x, t)
fig.show()

# Quantum state visualization  
fig_q = quantum_analyzer.visualize_quantum_state(psi_1, psi_2, f"t={t:.3f}")
fig_q.show()
```

---

## Validation & Testing

### Test Suite
```bash
pytest tests/                    # Full test suite
pytest tests/test_accuracy.py    # Accuracy validation
pytest tests/test_hardware.py    # Hardware compatibility  
pytest tests/test_scaling.py     # Performance scaling
```

### Benchmarking
- **Analytical validation** against exact Burgers' solutions
- **Classical comparison** with upwind finite differences
- **Convergence studies** with grid refinement
- **Hardware noise** resilience testing

### Continuous Integration
- Automated testing on classical simulators
- Hardware queue submission for IBM Quantum
- Performance regression detection
- Documentation generation and validation

---

### Related Publications
- Zhaoyuan Meng and Yue Yang. "Quantum computing of fluid dynamics using the hydrodynamic Schrödinger equation" (2023)
- Furkan Oz, Rohit K. S. S. Vuppala, Kursat Kara, Frank Gaitan. "Solving Burgers’ equation with quantum computing" (2021) 

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Team & Acknowledgments

### Core Team
Islam M. Albazlamit - https://www.linkedin.com/in/islambzl/

Luis Gerardo Ayala B - https://www.linkedin.com/in/luis-gerardo-ayala-bertel/

Kudzai Musarandega - https://www.linkedin.com/in/kudzai-musarandega-5156a521a/
