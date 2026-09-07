# Day 2 — Quantum Error Correction Technique: Phase-Flip Code

## 1. Introduction

In Day 1, we studied the **3-Qubit Bit-Flip Code**, which protects a logical qubit against a single bit-flip error represented by the Pauli-X operator.

In Day 2, we extend the same idea to **phase-flip errors**. A phase flip does not change the computational value from $|0\rangle$ to $|1\rangle$ or vice versa. Instead, it changes the **relative phase** between the components of a quantum superposition.

The main idea is to transform the phase-flip problem into a bit-flip problem, use the same repetition-code idea, and then transform back.

---

## 2. Key Idea

A general qubit is:

$|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$

A phase-flip error is represented by the Pauli-Z operator:

$Z = \begin{pmatrix} 1 & 0 \ 0 & -1 \end{pmatrix}$

Its action is:

$Z|0\rangle = |0\rangle$

$Z|1\rangle = -|1\rangle$

Therefore:

$Z|\psi\rangle = \alpha|0\rangle - \beta|1\rangle$

The important point is that the amplitudes are not exchanged; the relative sign between them changes.

---

## 3. What Is a Phase-Flip Error?

A phase flip changes the relative phase of the quantum state.

For example:

$|+\rangle = \frac{|0\rangle+|1\rangle}{\sqrt{2}}$

After a phase flip:

$Z|+\rangle = \frac{|0\rangle-|1\rangle}{\sqrt{2}}$

The resulting state is:

$|-\rangle = \frac{|0\rangle-|1\rangle}{\sqrt{2}}$

So:

$|+\rangle \xrightarrow{Z} |-\rangle$

This is different from a bit flip, where the basis states themselves are exchanged.

---

## 4. Why Is Phase Flip More Difficult to See?

If we look only at the computational basis, $|0\rangle$ and $|1\rangle$, a phase difference may not be directly visible in a simple measurement.

For example:

$|0\rangle \rightarrow |0\rangle$

under $Z$.

Therefore, the error is not necessarily detected by checking whether the qubit changed from 0 to 1.

The error exists in the **relative phase**, which becomes observable when the state is expressed or measured in another basis.

This is why the Hadamard gate is important in the phase-flip code.

---

## 5. Hadamard Gate — Changing the Basis

The Hadamard gate is:

$
H = \frac{1}{\sqrt{2}}\begin{pmatrix}1 & 1 \1 & -1\end{pmatrix}
$

It transforms the computational basis into the Hadamard basis:

$H|0\rangle = |+\rangle$

$H|1\rangle = |-\rangle$

and:

$H|+\rangle = |0\rangle$

$H|-\rangle = |1\rangle$

The important property is:

$H Z H = X$

This means that a phase-flip error can be converted into a bit-flip error by changing the basis.

---

## 6. Convert Phase Error into Bit Error

This is the central idea of the phase-flip code.

Suppose a phase error $Z$ occurs.

By applying Hadamard gates before and after the error:

$H Z H = X$

Therefore:

$\boxed{\text{Phase flip } Z \rightarrow \text{ Bit flip } X}$

Once the phase error has been converted into a bit error, we can use the same basic repetition-code strategy that we learned in Day 1.

This is a powerful general idea in quantum computing:

> **Change the basis so that an error becomes easier to detect and correct.**

---

## 7. Encoding Against a Phase-Flip Error

For the phase-flip code, the logical states are encoded in the Hadamard basis.

Starting with:

$|0_L\rangle = |+++\rangle$

and:

$|1_L\rangle = |---\rangle$

An arbitrary logical qubit can therefore be represented as:

# $|\psi_L\rangle\alpha|+++\rangle+\beta|---\rangle$

where:

$|+\rangle = \frac{|0\rangle+|1\rangle}{\sqrt{2}}$

and:

$|-\rangle = \frac{|0\rangle-|1\rangle}{\sqrt{2}}$

The code therefore provides protection against a single phase-flip error.

---

## 8. How the Code Works Conceptually

The phase-flip code can be understood as three stages:

**1. Change basis**

Use Hadamard gates so that phase information is represented in a basis where it can be treated like a bit value.

**2. Apply repetition-code protection**

The three physical qubits represent the logical information redundantly in the new basis.

**3. Change basis back**

After error detection and correction, Hadamard gates return the system to the original computational basis.

Conceptually:

$\text{Phase error}
\rightarrow
\text{Basis change}
\rightarrow
\text{Bit error}
\rightarrow
\text{Correction}
\rightarrow
\text{Basis change back}$

---

## 9. Syndrome Measurement

Just like the bit-flip code, we do not directly measure the logical qubit to determine whether an error occurred.

Instead, we extract a **syndrome**.

The syndrome provides information about the error location while preserving the logical quantum information.

For a three-qubit phase-flip code, the syndrome can distinguish:

| Syndrome | Error                  |
| -------- | ---------------------- |
| `00`     | No error               |
| `10`     | Phase error on qubit 1 |
| `11`     | Phase error on qubit 2 |
| `01`     | Phase error on qubit 3 |

> The displayed syndrome-bit order can depend on the circuit and Qiskit classical-register convention.

The important principle is:

$\boxed{\text{Syndrome} \rightarrow \text{Error location}}$

---

## 10. Correction

Once the syndrome identifies the affected physical qubit, the corresponding correction is applied.

For a phase-flip error, the recovery operation is a $Z$ operation on the identified qubit.

For example:

$\text{Syndrome} \rightarrow \text{Error on }q_1$

then:

$\text{Apply }Z_1$

Because:

$Z^2 = I$

we have:

$ZZ|\psi\rangle = |\psi\rangle$

Therefore, the phase error is reversed.

---

## 11. Complete Phase-Flip Circuit

At a conceptual level, the complete circuit contains:

1. Logical-state preparation
2. Basis transformation using Hadamard gates
3. Encoding across three physical qubits
4. Phase-flip error
5. Syndrome extraction
6. Syndrome measurement
7. Error identification
8. Recovery using $Z$
9. Final basis transformation
10. Logical measurement

The complete flow is:

**Prepare → Encode → Phase Error → Syndrome → Identify → Correct → Decode → Measure**

---

## 12. Simulator Testing

In simulation, we can intentionally introduce a phase-flip error using the $Z$ gate.

For example, we can test:

$Z_1$

$Z_2$

$Z_3$

These represent phase errors occurring at each of the three possible physical-qubit locations.

The purpose of the experiment is to verify that the syndrome changes according to the error location and that the recovery operation restores the logical state.

---

## 13. Comparison With No Error Correction

A useful practical experiment is to compare two cases:

### Without QEC

A phase-flip error is introduced directly into the qubit. There is no redundancy or recovery mechanism, so the error can affect the final result.

### With QEC

The logical state is encoded into three physical qubits. The syndrome identifies the error location and a recovery operation is applied.

The comparison helps demonstrate the purpose of QEC:

$\boxed{\text{QEC aims to reduce logical errors caused by physical noise.}}$

However, in real hardware, QEC itself introduces additional gates and measurements, so improvement is not guaranteed unless the underlying hardware is sufficiently reliable.

---

## 14. Visualization

The final results can be visualized by comparing the output probabilities or success rates of the system with and without error correction.

A simple comparison graph can answer an important question:

> **Does the error-corrected circuit produce the expected logical result more reliably than the uncorrected circuit?**

The graph is therefore not just for visualization; it provides an experimental way to evaluate the benefit of the QEC technique.

---

## 15. Test All Three Error Locations

A complete test should inject the phase-flip error into each physical qubit:

$Z_1,\quad Z_2,\quad Z_3$

The expected behavior is:

| Error Location | Expected Result                      |
| -------------- | ------------------------------------ |
| Qubit 1        | Syndrome identifies q1 → apply $Z_1$ |
| Qubit 2        | Syndrome identifies q2 → apply $Z_2$ |
| Qubit 3        | Syndrome identifies q3 → apply $Z_3$ |

Testing all three locations is important because the code should not only work for one specific error position.

---

## 16. Qiskit Terminology

The practical notebook uses several important Qiskit concepts.

**Hadamard gate (`H`)** changes the basis and is essential for converting phase errors into bit errors.

**Pauli-Z gate (`Z`)** represents a phase-flip error and is also used for phase-error recovery.

**Data qubits** are the physical qubits that store the encoded quantum information.

**Ancilla qubits** are auxiliary qubits used during syndrome extraction.

**Syndrome bits** are classical bits that store the measurement results used to identify the error.

**Logical qubit** represents the protected quantum information, while **physical qubits** are the actual hardware qubits used to encode it.

**Encoding** maps the logical state to multiple physical qubits, while **decoding** converts the protected representation back toward the logical state.

---

## 17. Real Quantum Hardware Advantages

The phase-flip code introduces an important practical technique for real quantum hardware: **basis transformation**.

Instead of trying to detect every type of error directly, we can transform the problem into a form that our error-correction circuit already knows how to handle.

This makes the concept of Pauli-frame and basis-aware error handling important in larger QEC systems.

However, real hardware has additional noise sources such as gate errors, decoherence, readout errors, crosstalk, and connectivity limitations.

The Hadamard gates and syndrome-extraction operations also introduce additional opportunities for error. Therefore, a practical implementation should minimize unnecessary gates and circuit depth and choose suitable physical qubits.

The key hardware goal remains:

$\boxed{\text{Logical error rate} < \text{Physical error rate}}$

---

## 18. Advantages

The phase-flip code:

* Protects against a single phase-flip error.
* Demonstrates how basis changes can simplify error correction.
* Uses the same repetition-code idea introduced in Day 1.
* Introduces the important relationship $H Z H = X$.
* Helps build intuition for more general quantum error correction.
* Provides a foundation for understanding codes that handle both bit and phase errors.

---

## 19. Limitations

The phase-flip code is also a specialized code.

It is designed to correct a single phase-flip error, not arbitrary combinations of quantum errors.

It also requires additional gates for basis transformations and syndrome extraction. On real hardware, these operations can themselves introduce errors.

Therefore, just like the 3-qubit bit-flip code, it is mainly a **foundational QEC technique** rather than a complete fault-tolerant solution.

---

## 20. Day 1 → Day 2 Connection

The easiest way to connect both techniques is:

**Day 1 — Bit-Flip Code**

$X$ error → repetition code → syndrome → $X$ recovery

**Day 2 — Phase-Flip Code**

$Z$ error → Hadamard basis change → bit-flip equivalent → syndrome → $Z$ recovery

The key mathematical relationship is:

$H Z H = X$

So the phase-flip code is not an entirely different idea. It is largely the **bit-flip protection strategy viewed in a different basis**.

---

## 21. Final Takeaways

* A phase flip changes the **relative phase** of a quantum state.
* The Pauli-$Z$ operator represents a phase-flip error.
* The Hadamard gate changes between computational and Hadamard bases.
* The important identity is:

$H Z H = X$

* This allows a phase error to be converted into a bit-flip error.
* Three physical qubits are used to protect one logical qubit.
* Syndrome measurement identifies the location of a single phase error.
* The corresponding $Z$ operation performs recovery.
* All three possible physical error locations should be tested.
* Real hardware requires careful optimization because QEC circuits also introduce errors.

### One-Line Mental Model

> **Convert the phase error into a bit error using a basis change, apply the familiar repetition-code error-correction idea,
> and transform back.**

---

**Created By** : Shreya Palase

**Date** : 04-september-2026

Thank you and Keep Learning!
