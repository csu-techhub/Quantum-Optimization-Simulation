# Quantum Optimization and Simulation — VQE Laboratory Series

Nine notebooks tracking the Module 3 → Module 4 sequence. Every notebook ships in two
versions:

- `labs/student/LabNN_*.ipynb` — `# TODO` blanks with `assert` self-checks
- `labs/instructor/LabNN_*_SOLUTION.ipynb` — identical, blanks filled

Both are generated from the same source, so they cannot drift apart.

---

## The sequence

| # | Notebook | Maps to | Student time | Compute |
|---|---|---|---|---|
| 1 | Python and Qubits | M3 L1–2 | 60 min | seconds |
| 2 | Pauli Operators, Tensor Products, R_ZZ | M3 L3 | 60 min | seconds |
| 3 | R_XX, R_YY, and the Configuration Mixer | M3 L3–4 | 60 min | seconds |
| 4 | Hamiltonians and Energy Estimation from Shots | M3 L5–6 | 60 min | ~30 s |
| 5 | **CAPSTONE — Ground-State Energy of H₂** | M3 whole + M4 L4 | 75 min | ~30 s |
| 6 | Spin Orbitals, Encodings, and UCCSD | M4 L1–3 | 60 min | ~10 s |
| 7 | Running H₂ on Real Hardware | M3 L6 applied | 60 min + queue | queue-bound |
| 8 | Materials I — Transverse-Field Ising Chain | M4 applied | 60 min | ~2 min |
| 9 | Materials II — Hubbard Dimer | M4 applied | 75 min | ~2 min |

Labs 1–6, 8, 9 run entirely on the Aer simulator. Lab 7 needs an IBM Quantum account;
Lab 8 has an optional hardware section at the end.

## Verified environment

Every code cell was executed during authoring against:

```
qiskit 2.5.1 · qiskit-aer 0.17.2 · qiskit-nature 0.8.0 · pyscf 2.14.0
qiskit-ibm-runtime 0.48.0 · numpy 2.5 · scipy · matplotlib
```

Colab setup (first cell of each notebook, commented out by default):

```python
%pip install -q qiskit qiskit-aer qiskit-nature pyscf qiskit-ibm-runtime matplotlib scipy
```

Lab 5 degrades gracefully: if Qiskit Nature or PySCF is missing it falls back to a stored
table of Hamiltonian coefficients, so the capstone still runs.

## Numbers your students will reproduce

- **Lab 4** — the lecture's shot table (810/190 etc.) → ⟨Z₀⟩ = 0.62, ⟨Z₁⟩ = −0.62,
  ⟨Z₀Z₁⟩ = −1.0, E = −0.4029 Ha.
- **Lab 5** — PySCF + `ParityMapper` returns *exactly* your slide's coefficients
  (c₀ = −1.0523732458, c₁ = +0.3979374248, c₄ = +0.1809311998); VQE gives
  **R_eq = 0.73 Å, E₀ = −1.13729 Ha = −30.95 eV** against your −0.741 Å / −30.9 eV.
- **Lab 6** — JW vs BK vs parity: same energy, 5 vs 2 vs 2 measurement settings,
  56 vs 4 CNOTs after reduction (14× fewer).
- **Lab 9** — reproduces the analytic (U − √(U²+16t²))/2 to under 1 mHa across U/t = 0–8.

## Pedagogical design

**Python onboarding is folded into Lab 1**, not given its own lab: a C++/Java-to-Python
translation table, `@` for matrix multiplication, and the notebook survival rules
(Shift+Enter, shared namespace, Restart & Run All). That has been enough in practice.

**Complex numbers are never multiplied by hand.** Students only ever need three facts
(magnitude, |z|², and that e^{iφ} rotates without changing length). NumPy does the
arithmetic; students interpret it.

**Endianness is called out explicitly** in Labs 1, 4, and 6 with a warning box each time.
Qiskit prints q_{n-1}…q_0 while the slides write |q₀q₁⟩ left to right — this is the single
most common source of silently wrong answers, and it is worth the repetition.

**Every lab ends with five checkpoint questions** suitable for a quiz or a discussion
opener, and every `# TODO` has an `assert` immediately after it so students know
themselves whether they got it right.

## Three notes on the decks

Offered for your judgement, not as corrections — all three are handled inside the
notebooks in a way that is consistent with your slides either way.

1. **The XX+YY coefficient.** Writing H with c₄(X₀X₁ + Y₀Y₁) and c₄ = 0.18093 doubles the
   |01⟩↔|10⟩ coupling relative to the XX-only form Qiskit Nature produces. Using c₄/2 on
   each makes the two forms give bit-identical ground states (verified numerically in
   Lab 4). Your numerical-example slide would be unaffected in structure — only the
   coefficient in the last term changes.

2. **Nuclear repulsion.** That coefficient set is the electronic part only, so the
   −0.4029 Ha from the shot example is not directly comparable to −30.9 eV, which is a
   *total* energy (−1.1373 Ha × 27.2114). Lab 4 makes E_total = ⟨H_qubit⟩ + E_nuc explicit
   and gives E_nuc(0.735 Å) = 0.7200 Ha.

3. **The U₁ Pauli string.** Qiskit's Jordan–Wigner map of a₂†a₀ − a₀†a₂ gives
   (i/2)(X₂Z₁Y₀ − Y₂Z₁X₀) — the XY−YX pattern, not XX+YY. The distinction is real: the
   XX+YY mixer puts an imaginary amplitude on |10⟩, and for a real molecular Hamiltonian
   that phase cannot lower the energy (I checked — the optimizer stays stuck at the
   Hartree–Fock value). Your Module 4 "Reduced Qubit Mapping" slide already has the
   correct XY−YX form; it is only the intermediate "Final circuit" slide for U₁ that
   shows XX+YY. Lab 3 turns this into a teaching moment: the Module 3 mixer does exactly
   what your slide claims (mixes |10⟩⇆|01⟩, leaves |00⟩,|11⟩ alone), and then students
   discover *why* Module 4 reaches for the Givens cousin.

## Suggested use

- **Take-home:** Labs 1–4 in week 1, Lab 5 as the graded capstone, Labs 6–9 in week 2.
- **In class:** run the instructor copy top to bottom; each is timed for 10–15 minutes at
  a brisk pace, with the plots as the natural pause points.
- **Grading:** Lab 5 has an explicit "Deliverables" section with five written questions.
  Labs 8 and 9 have open-ended extensions (Heisenberg limit, 3-site chain, bring-your-own
  Hamiltonian) suitable for a small project.

## Regenerating

The `build/` directory holds one Python script per lab plus `nbtools.py`. Running
`python3 labNN.py` executes every code cell as a smoke test and then emits both notebook
versions, so edits stay verified. `python3 rebuild.py` regenerates all eighteen without
re-running the tests.
