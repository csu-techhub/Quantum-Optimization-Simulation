# VQE Laboratory Series

Seven Google-Colab-ready notebooks for an introductory **Quantum Optimization and Simulation** course. The sequence starts with short Python/Qiskit preparation and gradually builds toward VQE, molecular geometry, noise, and an optional materials-model example.

The labs are designed for learners who are **not necessarily STEM professionals**. Most code is supplied, mathematical detail is introduced only when it supports the experiment, and the notebooks use small **YOUR TURN** questions rather than large programming assignments.

## Laboratory sequence

| Lab | Topic | Main idea |
|---|---|---|
| **Lab08** | Python, Colab, NumPy, and Qiskit refresher | Minimal Python/NumPy/Qiskit skills needed later |
| **Lab09** | Pauli operators and Pauli rotations | Connect matrices, circuits, and `AerSimulator` |
| **Lab10** | Pauli measurements and energy reconstruction | Build an energy from Z/X/Y measurement settings |
| **Lab11** | From energy estimation to VQE optimization | Use the **same two-qubit circuit as QLab3**, optimize with COBYLA, then study fake-device noise |
| **Lab12** | Complete four-qubit H₂ VQE | Jordan–Wigner mapping, paired double excitation, PySCF Hamiltonian, COBYLA |
| **Lab13** | H₂ potential-energy curve and bond length | Repeat VQE over geometry; fake-device noise and optional hardware |
| **Lab14 (optional)** | Small materials-model VQE | Apply the VQE workflow to a transverse-field Ising model |

## Common notebook style

Each notebook uses the same general pattern:

1. short explanation of the goal;
2. brief learning objectives;
3. setup/import cell;
4. mostly completed code;
5. small **YOUR TURN** questions;
6. expected interpretation or collapsible suggested answers where useful;
7. a short reflection section.

Qiskit displays bitstrings in `q_(n-1)...q_0` order. When the chemistry discussion uses logical orbital order `q0, q1, ...`, the notebooks state the convention explicitly.

## Simulator, noise, and hardware progression

The series intentionally introduces execution realism gradually.

- **Lab08-09:** basic `AerSimulator` use.
- **Lab10:** finite-shot Pauli measurements and energy reconstruction.
- **Lab11:** COBYLA optimization using the same measured-energy method, followed by a fake IBM-device noise model. A shot-count experiment demonstrates that more shots reduce random variation but do not remove systematic hardware-noise bias. Real IBM hardware is optional.
- **Lab12:** full four-qubit H₂ VQE using `EstimatorV2`.
- **Lab13:** geometry optimization is performed with ideal simulation first; fake-device noise and optional real hardware are then demonstrated at one selected geometry rather than over the entire curve.
- **Lab14:** optional application to a small materials/condensed-matter model.

## Circuit-execution counters

Lab10–Lab13 print an execution count so learners can see that hybrid algorithms may require many quantum evaluations.

- In **Lab10–Lab11**, the counter records the measurement circuits explicitly executed. One energy estimate uses three measurement settings (Z, X, and Y).
- In **Lab12–Lab13**, the counter records `EstimatorV2` energy-evaluation requests. A primitive may internally use more than one physical measurement circuit to estimate an observable.

This distinction is stated inside the notebooks.

## Noise and the role of shots

A noisy result should not be expected to match the ideal simulator exactly.

A useful mental model is

\[
\text{measured result}
\approx
\text{ideal result}
+
\text{systematic noise bias}
+
\text{sampling fluctuation}.
\]

Increasing shots reduces sampling fluctuation approximately as \(1/\sqrt{N_{\rm shots}}\), but it does not by itself remove gate, decoherence, or readout errors. QLab4 demonstrates this directly. The notebooks also mention circuit optimization and more advanced error-mitigation ideas without making them required material.

## Optional real IBM Quantum hardware

Lab11 and Lab13 contain optional IBM Runtime templates.

These sections may require:

- an IBM Quantum account and credentials;
- an available backend;
- queue time;
- current Qiskit Runtime access.

To keep hardware use modest, the required optimization is done on simulators first. The optional hardware sections evaluate only a small number of optimized circuits.

IBM Runtime APIs and available devices can change, so the optional cells may need a small update in a future course offering.

## Suggested software versions

The notebooks currently target approximately:

```text
qiskit ~= 2.5
qiskit-aer ~= 0.17
qiskit-algorithms ~= 0.4
qiskit-nature ~= 0.8
qiskit-ibm-runtime ~= 0.47
pyscf ~= 2.8
```

## References

1. IBM Quantum Learning — VQE: https://quantum.cloud.ibm.com/learning/en/modules/computer-science/vqe
2. IBM Quantum documentation — Qiskit Runtime and V2 primitives: https://quantum.cloud.ibm.com/docs/
3. Qiskit Nature documentation: https://qiskit-community.github.io/qiskit-nature/
