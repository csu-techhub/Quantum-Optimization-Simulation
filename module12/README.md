# QAOA Laboratory Series

Eight Google-Colab-ready labs aligned with the Quantum Optimization and Simulation Module 1/2 lecture progression.

Each lab has:
- `*_STUDENT.ipynb`: scaffolded notebook with small TODOs.
- `*_INSTRUCTOR.ipynb`: same lab plus completed solutions / answer key.

Sequence:
1. Python, Colab, Qiskit quick start
2. H, RZ, RX and QAOA parameters
3. ZZ/RZZ and cost operator
4. Cost + mixer on 3-node Max-Cut
5. Complete 5-node Max-Cut from gates + built-in QAOA
6. COBYLA hybrid optimization loop; p=2/SPSA extensions
7. Ideal/noisy simulator + optional IBM hardware
8. Portfolio optimization

The notebooks install Qiskit 2.x-era packages in Colab. Real-hardware execution in Lab 7 requires the learner's own IBM Quantum access credentials.

1Python, Colab & Qiskit Quick Start
2From Quantum Gates to QAOA Parameters
3Building the QAOA Cost Operator
4Cost + Mixer: Your First Complete QAOA
5Complete 5-Node Max-Cut QAOA
6Closing the Hybrid Loop with COBYLA
7Ideal Simulator, Noise & Real Hardware
8QAOA for Portfolio Optimization

Lab 5 builds QAOA from the gates up first—including the six CX–RZ(-?)–CX cost blocks—and only afterward compares it with Qiskit's high-level QAOA implementation. Lab 7 defaults to Aer and keeps IBM hardware as an optional section. Lab 8 uses four hypothetical assets and an exactly-two-assets constraint, avoiding dependence on live financial data. 