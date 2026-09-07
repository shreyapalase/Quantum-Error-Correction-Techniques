# Day 1 — Quantum Error Correction: 3-Qubit Bit-Flip Code



Day 1 introduces the **3-Qubit Bit-Flip Code**, one of the simplest quantum error-correction techniques. The goal is to understand how a logical qubit can be protected from a single bit-flip error using multiple physical qubits, syndrome measurements, stabilizers, and recovery operations.

The day combines the **mathematical foundation** with a **practical Qiskit implementation**, while also introducing important terminology and considerations for running QEC circuits on real quantum hardware.

---

##  Learning Objectives

By the end of Day 1, you should understand:

* Why quantum error correction is necessary
* What a quantum bit-flip error is
* The Pauli-X operator and its role in bit flips
* Why an arbitrary quantum state cannot simply be copied
* How the 3-qubit repetition code works
* The difference between logical and physical qubits
* How an arbitrary logical qubit is encoded
* How a bit-flip affects the encoded state
* What a syndrome is and why it is required
* The role of stabilizers in quantum error correction
* How syndrome extraction works
* How the error location is identified
* How the recovery operation corrects the error
* Basic Qiskit terminology used in QEC
* Advantages and limitations of the 3-qubit code
* Important considerations when running QEC on real quantum hardware

---

##  Day 1 Resources

### 01 — Day 1 Notes

A concise conceptual and mathematical guide to the **3-Qubit Bit-Flip Code**. These notes explain the complete QEC workflow from the basic qubit and bit-flip error to encoding, stabilizers, syndrome extraction, error identification, and recovery.

**Covers:** Concepts • Mathematics • Terminology • Hardware Considerations

**File:** `Day01_Notes.md`

 **[Read Day 1 Notes](./Day01_Notes.md)**

---

###  02 — Qiskit Implementation

A practical Qiskit implementation of the **3-Qubit Bit-Flip Code**. The notebook demonstrates the concepts discussed in the notes through an executable quantum circuit, including encoding, error injection, syndrome extraction, and recovery.

**Covers:** Circuit • Encoding • Error Injection • Syndrome • Recovery • Results

**File:** `Day1-3Qubit_Bil_Flip_Code.ipynb`

 **[Open Qiskit Implementation](./Day1-3Qubit_Bit_Flip_Code.ipynb)**

---

##  QEC Workflow

The core idea of Day 1 can be summarized as:

**Logical Qubit**

↓

**Encoding**

↓

**3 Physical Qubits**

↓

**Bit-Flip Error**

↓

**Syndrome Extraction**

↓

**Error Identification**

↓

**Recovery**

↓

**Corrected Logical Information**

---

## Key Concept

The 3-qubit bit-flip code does not prevent a physical error from occurring. Instead, it uses redundancy and syndrome information to **identify and correct a single bit-flip error without directly measuring the protected logical state**.

> **Encode → Detect → Identify → Recover**

---

## Scope of This Code

The 3-qubit bit-flip code is primarily a **foundational QEC technique**.

It can correct **one bit-flip error**, but it does not provide complete protection against arbitrary quantum errors such as phase-flip errors. More advanced QEC codes are required for general fault-tolerant quantum computing.

---

## Real Hardware Perspective

The notebook may work ideally in simulation, but real quantum hardware introduces additional challenges such as gate errors, readout errors, decoherence, crosstalk, connectivity constraints, and calibration variations.

Therefore, practical QEC requires **hardware-aware circuit design**, low-depth circuits, efficient qubit mapping, reliable physical qubits, and careful error analysis.

---

## Day 1 Structure

```text
Day 1/
│
├── README.md
├── Day 1 Notes.md
└── Day1-3Qubit_Bit_Flip_Code.ipynb
```

---

## Next Step

After understanding the 3-Qubit Bit-Flip Code, the natural progression is to study **phase-flip errors**,
followed by more complete QEC codes and eventually the **stabilizer formalism, surface codes, logical qubits, and
fault-tolerant quantum computing**.

---

**Created By** : Shreya Palase(codeQubit)

**Project**: Quantum-Error-Correction-Techniques

**Date** : 02-sepetember-2026

