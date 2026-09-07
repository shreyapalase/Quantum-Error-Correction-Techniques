# Day 1 — Quantum Error Correction: 3-Qubit Bit-Flip Code

## 1. Why Quantum Error Correction?

Quantum computers are sensitive to noise and imperfections. During a computation, a qubit can unintentionally change its state. These unwanted changes are called **quantum errors**.

A general qubit can be written as:

$$
|\psi\rangle = \alpha|0\rangle + \beta|1\rangle
$$

where $\alpha$ and $\beta$ are probability amplitudes and:

$$
|\alpha|^2 + |\beta|^2 = 1
$$

**Quantum Error Correction (QEC)** protects this quantum information from errors without directly measuring the unknown quantum state.

---

## 2. What Is a Bit-Flip Error?

A **bit-flip error** changes:

$$
|0\rangle \rightarrow |1\rangle
$$

and

$$
|1\rangle \rightarrow |0\rangle
$$

It is represented by the **Pauli-X operator**:

$$
X =
\begin{pmatrix}
0 & 1 \\
1 & 0
\end{pmatrix}
$$

Therefore:

$$
X|\psi\rangle=\alpha|1\rangle + \beta|0\rangle
$$

The 3-qubit bit-flip code is designed to correct **one bit-flip error**.

---

## 3. Why Can't We Simply Copy a Qubit?

For classical bits, we can use repetition:

$$
0 \rightarrow 000
$$

$$
1 \rightarrow 111
$$

For an unknown quantum state, however, the **No-Cloning Theorem** says that an arbitrary quantum state cannot simply be copied.

Instead, QEC uses **encoding**. The quantum information is distributed across multiple physical qubits.

---

## 4. 3-Qubit Repetition Code

The logical states are encoded as:

$$ 
|0_L\rangle = |000\rangle 
$$

$$
|1_L\rangle = |111\rangle
$$

Therefore, an arbitrary logical qubit:

$$
|\psi\rangle = \alpha|0\rangle + \beta|1\rangle
$$

is encoded as:

$$
\boxed{
|\psi_L\rangle =
\alpha|000\rangle + \beta|111\rangle
}
$$

Here:

* **Logical qubit** → the quantum information we want to protect.
* **Physical qubit** → an actual qubit used to store the information.
* **Encoded state** → the logical information distributed across physical qubits.

So:

$$
1\text{ logical qubit}
\rightarrow
3\text{ physical qubits}
$$

---

## 5. What Happens When an Error Occurs?

Suppose the encoded state is:

$$
|\psi_L\rangle =
\alpha|000\rangle + \beta|111\rangle
$$

If the first physical qubit flips:

$$
X_1|\psi_L\rangle=\alpha|100\rangle + \beta|011\rangle
$$

If the second qubit flips:

$$
X_2|\psi_L\rangle=\alpha|010\rangle + \beta|101\rangle
$$

If the third qubit flips:

$$
X_3|\psi_L\rangle=\alpha|001\rangle + \beta|110\rangle
$$

The important point is that these error cases can be distinguished without directly measuring $\alpha$ and $\beta$.

---

## 6. What Is a Syndrome?

A **syndrome** is classical information that tells us what error occurred or where it occurred.

We do **not** directly measure the logical qubit because measurement can destroy its quantum state.

Instead, we measure properties of the encoded state using **stabilizers**.

For the 3-qubit bit-flip code:

$$
S_1 = Z_1Z_2
$$

$$
S_2 = Z_2Z_3
$$

These measurements produce two syndrome bits.

| Syndrome | Meaning          |
| -------- | ---------------- |
| `00`     | No error         |
| `10`     | Error on qubit 1 |
| `11`     | Error on qubit 2 |
| `01`     | Error on qubit 3 |

> The exact bit order can depend on the circuit and Qiskit classical-register convention.

The main idea is:

$$
\boxed{
\text{Syndrome} \rightarrow \text{Error location}
}
$$

---

## 7. What Are Stabilizers?

A **stabilizer** is an operator that describes a property of the valid encoded state.

For a correct encoded state:

$$
S_1|\psi_L\rangle = |\psi_L\rangle
$$

and:

$$
S_2|\psi_L\rangle = |\psi_L\rangle
$$

When an error occurs, the stabilizer results change. These changes form the **syndrome**.

A simple way to remember it:

> **Stabilizers check the encoded state; the syndrome tells us what went wrong.**

---

## 8. Recovery

After obtaining the syndrome, we identify the error and apply the corresponding correction.

For example:

```text
syndrome → error on q1 → apply X on q1
```

Since:

$$
X^2 = I
$$

applying $X$ twice gives:

$$
XX|\psi\rangle = |\psi\rangle
$$

Therefore, the error can be reversed.

The complete QEC process is:

$$
\boxed{
\text{Encode}
\rightarrow
\text{Error}
\rightarrow
\text{Syndrome}
\rightarrow
\text{Recovery}
}
$$

---

## 9. Important Qiskit Terminology

When reading the Qiskit implementation, focus on the meaning of each component.

### Data Qubits

The physical qubits that contain the encoded quantum information.

### Logical Qubit

The protected quantum information. It is represented using multiple physical qubits rather than a single hardware qubit.

### Physical Qubit

An actual qubit available in the quantum processor.

### Ancilla Qubit

An additional qubit used for tasks such as syndrome extraction. It helps detect errors without directly measuring the logical state.

### Syndrome Bits

Classical bits used to store the result of syndrome measurements.

### Encoding

The process:

$$
\text{Logical qubit}
\rightarrow
\text{Encoded physical qubits}
$$

### Syndrome Extraction

The process of obtaining information about the error using stabilizers and ancilla qubits.

### Recovery

Applying the correction operation according to the syndrome.

---

## 10. How Much Error Can It Correct?

The 3-qubit bit-flip code has distance:

$$
d = 3
$$

The number of correctable errors is:

$$
t =
\left\lfloor
\frac{d-1}{2}
\right\rfloor
$$

Therefore:

$$
t =
\left\lfloor
\frac{3-1}{2}
\right\rfloor
= 1
$$

So the code can correct:

$$
\boxed{\text{one bit-flip error}}
$$

It does **not** correct arbitrary quantum errors.

For example, a phase-flip is represented by the Pauli-Z operator:

$$
Z|0\rangle = |0\rangle
$$

$$
Z|1\rangle = -|1\rangle
$$

Handling both bit-flip and phase-flip errors requires more advanced QEC codes.

---

## 11. Real Quantum Hardware

On a simulator, we can easily create an ideal circuit and intentionally add a bit-flip error. Real quantum hardware is more challenging because the qubits, gates, and measurements are themselves noisy.

Important hardware problems include:

* Gate errors
* Two-qubit gate errors
* Decoherence
* Readout errors
* Crosstalk
* Limited qubit connectivity
* Calibration drift

An important practical point is that **QEC circuits also contain gates that can introduce errors**.

Therefore, simply adding more qubits does not automatically make the computation better.

A useful hardware goal is:

$$
\boxed{
\text{Logical error rate}
<
\text{Physical error rate}
}
$$

Useful hardware techniques include:

* Choosing better physical qubits
* Reducing two-qubit gates
* Minimizing circuit depth
* Hardware-aware qubit mapping
* Measurement-error mitigation
* Dynamical decoupling when appropriate

---

## 12. Advantages

The 3-qubit bit-flip code is useful because it:

* Demonstrates the basic idea of QEC.
* Shows how one logical qubit can use multiple physical qubits.
* Introduces **syndrome measurement**.
* Introduces the **stabilizer concept**.
* Demonstrates error detection and recovery.
* Provides a foundation for advanced codes such as the **surface code**.

---

## 13. Limitations

The code is intentionally simple, so it has important limitations:

* It mainly protects against **one bit-flip error**.
* It does not fully protect against phase-flip errors.
* It requires three physical data qubits for one logical qubit.
* Syndrome extraction requires additional operations.
* Real hardware noise can reduce the benefit of the code.

Therefore, the 3-qubit bit-flip code is best understood as a **fundamental QEC learning model**.

---

## 14. Final Mental Model

The complete idea can be remembered as:

```text
Logical Qubit
     ↓
   Encode
     ↓
3 Physical Qubits
     ↓
   Error
     ↓
Syndrome Extraction
     ↓
Syndrome Bits
     ↓
Identify Error
     ↓
Recovery (X)
     ↓
Corrected Logical Information
```

### Key idea

> **The 3-qubit bit-flip code protects one logical qubit by encoding it into three physical qubits, using stabilizers to identify a single bit-flip error, and applying the corresponding recovery operation.**

---

## 15. One-Minute Revision

| Concept            | Meaning                                  |
| ------------------ | ---------------------------------------- |
| Bit flip           | $X$ error                                |
| Logical qubit      | Information we want to protect           |
| Physical qubit     | Actual hardware qubit                    |
| Encoding           | Logical → physical representation        |
| Stabilizer         | Checks the encoded state                 |
| Syndrome           | Identifies the error                     |
| Syndrome bit       | Classical error information              |
| Recovery           | Corrects the detected error              |
| Code distance      | $d=3$                                    |
| Correctable errors | 1 bit flip                               |
| Main limitation    | Does not handle arbitrary quantum errors |

---

**Created By** : Shreya Palase

**Date** : 02-September-2026

Thank You and Keep Learning!
