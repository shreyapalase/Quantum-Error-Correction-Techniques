 # Day 5 Notes — Quantum Error Correction Technique: Steane Code

 ## Overview

 The **Steane Code** is a seven-qubit quantum error-correcting code that encodes **one logical qubit into seven physical qubits**. It is one of the most important examples of a **CSS (Calderbank–Shor–Steane) stabilizer code**.

 The Steane Code is particularly useful because it can detect and correct **any single-qubit Pauli error**:

- $X$ — bit-flip error
- $Z$ — phase-flip error
- $Y$ — combined bit-flip and phase-flip error

 The code is commonly represented as:

$$
[[7,1,3]]
$$

 where:

 - $7$ = number of physical qubits
- $1$ = number of logical qubits encoded
- $3$ = code distance

 Because the distance is $3$, the Steane Code can **correct any single-qubit error**.

---

 ## 1\. Why Do We Need Seven Physical Qubits?

 In classical computing, copying information is relatively straightforward. If one copy is corrupted, another copy can be used to recover the original information.

 Quantum information is different because an unknown quantum state **cannot simply be copied** due to the **no-cloning theorem**.

 Instead of directly copying a quantum state, quantum error correction distributes the information across an **entangled multi-qubit state**.

 For the Steane Code, one logical qubit is encoded into seven physical qubits:

 $$
 1\text{ logical qubit}\longrightarrow7\text{ physical qubits}
$$

 The seven physical qubits do not represent seven independent copies of the logical state. Instead, they collectively store the logical information in an encoded subspace.

 This redundancy allows us to obtain information about errors without directly measuring and destroying the logical quantum state.

---

 # 2\. Physical Qubits vs Logical Qubits

 A **physical qubit** is an actual qubit used by the quantum computer.

 A **logical qubit** is quantum information encoded across multiple physical qubits so that errors can be detected and corrected.

 For the Steane Code:

 $$
|0\rangle_L,\ |1\rangle_L
$$

 represent the logical computational basis states.

 A general logical state can be written as:

 $$
|\psi\rangle_L=\alpha |0\rangle_L+\beta |1\rangle_L
$$

 where:

 $$
|\alpha|^2 + |\beta|^2 = 1.
$$

 The logical state is distributed across seven physical qubits.

 The important idea is:

 > We are not making seven copies of the quantum state. We are encoding one logical state into a larger protected Hilbert space.

---

 # 3\. The $\[\[7,1,3\]\]$ Steane Code

 The Steane Code is a:

 $$
[[7,1,3]]
$$

 quantum error-correcting code.

 The notation tells us three important properties:

 ### $7$ Physical Qubits

 Seven physical qubits are used to encode the information.

 ### $1$ Logical Qubit

 The seven physical qubits collectively store one logical qubit.

 ### Distance $3$

 The code has distance:

 $$
d=3.
$$

 For a quantum code, the number of correctable errors is:

 $$
t = \left\lfloor\frac{d-1}{2}\right\rfloor.
$$

 For the Steane Code:

 $$
t =\left\lfloor\frac{3-1}{2}\right\rfloor=1.
$$

 Therefore, it can correct **any single-qubit error**.

---

 # 4\. What Errors Can the Steane Code Correct?

 A general single-qubit error can be expressed using the Pauli operators:

 $$
I,\ X,\ Y,\ Z.
$$

 Here:

 ### Bit-Flip Error

 The $X$ operator is:

 $$
X =\begin{pmatrix}0 & 1\\1 & 0\end{pmatrix}.
$$

 It transforms:

 $$
X|0\rangle = |1\rangle
$$

 and

 $$
X|1\rangle = |0\rangle.
$$

 So $X$ represents a **bit-flip error**.

---

 ### Phase-Flip Error

 The $Z$ operator is:

 $$
Z =\begin{pmatrix}1 & 0\\0 & -1\end{pmatrix}.
$$

 It leaves $|0\\rangle$ unchanged but changes the phase of $|1\\rangle$:

 $$
Z|0\rangle = |0\rangle
$$

 $$
Z|1\rangle = -|1\rangle.
$$

 So $Z$ represents a **phase-flip error**.

---

 ### $Y$ Error

 The $Y$ operator is:

 $$
Y =\begin{pmatrix}0 & -i\\i & 0\end{pmatrix}.
$$

 It can be written as:

 $$
Y = iXZ.
$$

 Therefore, a $Y$ error combines both bit-flip and phase-flip behavior.

 The Steane Code can correct:

 $$
X,\quad Y,\quad Z
$$

 errors occurring on any one of the seven physical qubits.

---

 # 5\. Stabilizer Formalism

 The Steane Code is a **stabilizer code**.

 Instead of describing the entire encoded quantum state directly, we describe a set of operators called **stabilizers**.

 A stabilizer $S$ satisfies:

 $$
S|\psi_L\rangle = |\psi_L\rangle
$$

 for every valid encoded state $|\\psi\_L\\rangle$.

 The Steane Code has **six independent stabilizer generators**.

 A commonly used set is:

 $$
S_1 = X_1X_2X_3X_4I_5I_6I_7
$$

 $$
S_2 = X_1X_2I_3I_4X_5X_6I_7
$$

 $$
S_3 = X_1IX_3I_4X_5IX_7
$$

 and

 $$
S_4 = Z_1Z_2Z_3Z_4I_5I_6I_7
$$

 $$
S_5 = Z_1Z_2I_3I_4Z_5Z_6I_7
$$

 $$
S_6 = Z_1IZ_3I_4Z_5IZ_7.
$$

 The exact ordering of the stabilizer generators can vary depending on the convention used.

 The important point is that there are:

 $$
6
$$

 independent stabilizer generators for seven physical qubits.

---

 # 6\. Why Are There Six Stabilizers?

 For a stabilizer code with $n$ physical qubits and $k$ logical qubits, the number of independent stabilizer generators is:

 $$
n-k.
$$

 For the Steane Code:

 $$
n=7,\qquad k=1.
$$

 Therefore:

 $$
n-k=7-1=6.
$$

 So six independent stabilizers define the protected code space.

 The stabilizers constrain the seven-qubit system while leaving one logical qubit of quantum information.

---

 # 7\. CSS Structure

 The Steane Code is a **CSS code**.

 CSS stands for:

 **Calderbank–Shor–Steane.**

 A major advantage of CSS codes is that they separate bit-flip and phase-flip error correction.

 The stabilizers can be divided into two groups:

 ### $X$-type Stabilizers

 These contain only $X$ and $I$ operators.

 They are primarily associated with detecting **$Z$-type errors**.

 ### $Z$-type Stabilizers

 These contain only $Z$ and $I$ operators.

 They are primarily associated with detecting **$X$-type errors**.

 This separation makes the structure of the code easier to understand.

---

 # 8\. Parity Checks and the Classical Hamming Code

 One of the most interesting properties of the Steane Code is its connection to the classical **$\[7,4,3\]$ Hamming code**.

 The classical Hamming code has a parity-check matrix that can be written as:

 $$
H =\begin{pmatrix}1&0&0&1&0&1&1\\0&1&0&1&1&0&1\\0&0&1&0&1&1&1\end{pmatrix}.
$$

 Each column corresponds to one of the seven physical qubits.

 The three-bit syndrome generated by these parity checks can uniquely identify which single position is associated with an error.

 For the Steane Code, this classical parity-check structure is used to construct the quantum stabilizers.

 This is one reason the Steane Code is such an important example of a CSS code:

 > Classical error-correction ideas can be incorporated into a quantum error-correcting code.

---

 # 9\. Syndrome Measurement

 The key idea behind quantum error correction is that we do **not directly measure the logical qubit**.

 Instead, we measure the stabilizers.

 Suppose the encoded state is:

 $$
|\psi_L\rangle.
$$

 For a valid code state:

 $$
S_i|\psi_L\rangle = |\psi_L\rangle.
$$

 Therefore, measuring a stabilizer normally produces the eigenvalue:

 $$
+1.
$$

 Now suppose an error $E$ occurs.

 The state becomes:

 $$
E|\psi_L\rangle.
$$

 Depending on the relationship between $E$ and the stabilizers, some stabilizer measurements can produce:

 $$
-1.
$$

 The collection of stabilizer measurement results is called the **syndrome**.

---

 # 10\. Syndrome as an Error Fingerprint

 For the Steane Code there are six independent stabilizers, so the syndrome can be represented using six binary measurement results:

 $$
(s_1,s_2,s_3,s_4,s_5,s_6).
$$

 We can think of the syndrome as an **error fingerprint**.

 For example:

 $$
(+1,+1,+1,+1,+1,+1)
$$

 indicates the expected stabilizer eigenvalues and corresponds to **no detected error**.

 If one or more stabilizers return $-1$, the pattern indicates that an error has occurred.

 The syndrome does not tell us the quantum amplitudes $\\alpha$ and $\\beta$ of the logical state.

 Instead, it provides information about the **error location and type**.

 This is crucial because measuring the syndrome can reveal error information without directly measuring the logical quantum information.

---

 # 11\. Syndrome Extraction

 Conceptually, syndrome extraction follows this process:

```
Encoded logical state
        |
        v
Seven physical qubits
        |
        v
Possible physical error
        |
        v
Measure stabilizers
        |
        v
Generate syndrome
        |
        v
Identify error
        |
        v
Apply correction
        |
        v
Recovered logical state
```

 The syndrome is therefore the bridge between:

 $$
 \text{error detection}\longrightarrow\text{error correction}.
$$

---

 # 12\. How Can a Syndrome Identify a Single-Qubit Error?

 There are seven possible physical qubits where an error can occur.

 For a particular error type, the parity-check structure gives a unique syndrome associated with each position.

 Conceptually:

 $$
\text{Error on qubit }i
\longrightarrow
\text{unique syndrome}
\longrightarrow
\text{identify qubit }i.
$$

 For example, the three-bit binary labels of the columns of the Hamming parity-check matrix can be used to distinguish the seven qubit positions.

 Thus, the syndrome provides enough information to determine **where the error occurred**.

 For the CSS structure, separate syndrome information can distinguish the bit-flip and phase-flip components.

---

 # 13\. Understanding $X$, $Y$, and $Z$ Errors Through Syndromes

 The Steane Code can correct all three single-qubit Pauli errors.

 ### $X$ Error

 An $X$ error changes the computational basis component of a qubit.

 The $Z$-type stabilizers detect its presence through anticommutation.

 ### $Z$ Error

 A $Z$ error changes the phase component.

 The $X$-type stabilizers detect it.

 ### $Y$ Error

 Since:

 $$
Y=iXZ,
$$

 a $Y$ error contains both an $X$ and a $Z$ component.

 Therefore, both parts of the syndrome information are relevant.

 Conceptually:

 $$
Y\simX+Z
$$

 in terms of its error-correction behavior.

 The Steane Code therefore provides a unified way to correct:

 $$
X,\quad Y,\quad Z.
$$

---

 # 14\. Anticommutation and Error Detection

 The mathematical reason stabilizer measurements reveal errors is based on **commutation and anticommutation**.

 Suppose $S$ is a stabilizer and $E$ is an error.

 If:

 $$
SE=ES,
$$

 then $S$ commutes with $E$.

 The stabilizer eigenvalue is not flipped.

 But if:

 $$
SE=-ES,
$$

 then $S$ anticommutes with $E$.

 The stabilizer measurement changes sign:

 $$
+1\rightarrow -1.
$$

 This sign change becomes part of the syndrome.

 Therefore:

 > An error can be detected because it changes the eigenvalue of some stabilizer operators.

 This is the fundamental mechanism behind stabilizer-based quantum error correction.

---

 # 15\. Error Correction Process

 Once the syndrome has been measured, a decoder uses it to determine the most likely error.

 For the ideal single-error case:

```
1. Encode one logical qubit into seven physical qubits.
2. A single-qubit error occurs.
3. Measure the stabilizers.
4. Obtain the syndrome.
5. Map the syndrome to an error.
6. Apply the corresponding Pauli correction.
7. Recover the encoded logical state.
```

 For example:

 $$
X_i|\psi_L\rangle
$$

 means that an $X$ error occurred on physical qubit $i$.

 If the syndrome identifies qubit $i$, the correction operation is another $X\_i$:

 $$
X_iX_i|\psi_L\rangle=|\psi_L\rangle.
$$

 Ignoring an overall global phase where appropriate, the original encoded state is recovered.

 Similarly, a detected $Z\_i$ error is corrected using $Z\_i$.

 A detected $Y\_i$ error is corrected using $Y\_i$.

---

 # 16\. Why the Logical Information Is Preserved

 The goal of quantum error correction is not to determine the logical state by measurement.

 Suppose:

 $$
|\psi_L\rangle=\alpha|0\rangle_L+\beta|1\rangle_L.
$$

 The error-correction procedure should preserve the amplitudes:

 $$
\alpha,\quad\beta.
$$

 Instead of asking:

 > "Is the logical qubit $0$ or $1$?"

 we ask:

 > "What error happened to the encoded state?"

 The syndrome measurement provides information about the error while leaving the logical information protected inside the code space.

 After correction, the state should return to the valid logical subspace.

---

 # 17\. Code Space and Logical Operators

 The stabilizers define the **code space**.

 The encoded logical states satisfy:

 $$
S_i|\psi_L\rangle = |\psi_L\rangle
$$

 for all stabilizer generators $S\_i$.

 Logical operators act on the encoded information without being equivalent to stabilizer operations.

 The Steane Code has logical Pauli operators that can be represented by applying the same Pauli operator to all seven physical qubits:

 $$
\overline{X}=X^{\otimes 7}
$$

 and

 $$
\overline{Z}=Z^{\otimes 7}.
$$

 These operators act on the logical qubit rather than representing ordinary physical errors that are automatically removed by the code.

 This distinction is important:

 $$
\text{physical error}
\neq
\text{logical operation}.
$$

 A correctable physical error should be removed without changing the logical information.

---

 # 18\. What Does the Code Distance Mean?

 The distance of the Steane Code is:

 $$
d=3.
$$

 The distance describes the minimum number of physical-qubit errors required to transform one valid logical codeword into another distinguishable logical state without being detected as a correctable error.

 The general relationship is:

 $$
d=2t+1
$$

 for a code that corrects $t$ errors.

 For $d=3$:

 $$
3=2(1)+1.
$$

 Therefore:

 $$
t=1.
$$

 So the Steane Code can correct:

 $$
\boxed{\text{any single-qubit error}}
$$

 but it is not guaranteed to correct arbitrary two-qubit errors.

---

 # 19\. Error Detection vs Error Correction

 It is useful to distinguish these two concepts.

 ### Error Detection

 The syndrome tells us that something is wrong.

 For example:

 $$
\text{syndrome}\neq 000000
$$

 indicates a detected stabilizer violation under a suitable binary representation.

 ### Error Correction

 The syndrome is decoded to determine which correction operation should be applied.

 Therefore:

 $$
\text{Syndrome}\rightarrow\text{Error Identification}\rightarrow\text{Correction}.
$$

 Detection alone is not enough. The correction step is what returns the state to the desired code space.

---

 # 20\. Why the Steane Code Is Important

 The Steane Code is important not only because it corrects a single-qubit error, but also because it demonstrates several fundamental ideas in quantum error correction:

 - It is a **CSS stabilizer code**.
- It encodes one logical qubit into seven physical qubits.
- It has parameters $\[\[7,1,3\]\]$.
- It corrects arbitrary single-qubit Pauli errors.
- It has separate $X$-type and $Z$-type stabilizers.
- Its structure is closely related to the classical $\[7,4,3\]$ Hamming code.
- Its syndrome provides information about the location and type of an error.
- It demonstrates how redundancy can protect quantum information without cloning the quantum state.

---

 # 21\. Conceptual Workflow

 The complete theoretical workflow can be summarized as:

```
                 Logical Qubit
                       |
                       v
              ┌─────────────────┐
              │  Steane Encoding │
              └─────────────────┘
                       |
                       v
              7 Physical Qubits
                       |
                       v
                 Error occurs
                       |
             ┌─────────┴─────────┐
             |                   |
             v                   v
            X                 Y / Z
             |                   |
             └─────────┬─────────┘
                       |
                       v
              Syndrome Extraction
                       |
                       v
                Error Syndrome
                       |
                       v
                 Error Decoder
                       |
                       v
              Apply Correction
                       |
                       v
              Protected Logical
                   Information
```

 The essential idea is:

 $$
\boxed{\text{Encode}\rightarrow\text{Error}\rightarrow\text{Syndrome}\rightarrow\text{Correction}\rightarrow\text{Recover}}
$$

---

 # 22\. Qiskit Implementation — Brief Overview

 The practical implementation can be handled separately.

 In a Qiskit simulation, the main conceptual components are:

 1. Prepare a logical state.
2. Encode it using the Steane-code structure.
3. Introduce a single-qubit $X$, $Y$, or $Z$ error.
4. Extract the stabilizer syndrome.
5. Decode the syndrome.
6. Apply the corresponding correction.
7. Compare the logical result before and after correction.

 For simulation, **Qiskit Aer** can be used to model the circuit and observe the effect of the error-correction process.

 The detailed circuit construction, Qiskit code, syndrome-extraction circuit, and simulation results are covered separately.

---

 # 23\. Key Takeaways

 The main concepts from Day 5 are:

 - The **Steane Code** is a seven-qubit quantum error-correcting code.
- It encodes:

 $$
1\text{ logical qubit}\rightarrow7\text{ physical qubits}.
$$

 - Its parameters are:

 $$
\boxed{[[7,1,3]]}
$$

 - The code can correct **any single-qubit Pauli error**:

 $$
X,\quad Y,\quad Z.
$$

 - It is a **CSS stabilizer code**.
- It uses six independent stabilizer generators.
- Stabilizers define the protected code space.
- Errors can be detected through changes in stabilizer eigenvalues.
- The collection of measurement results is called the **syndrome**.
- The syndrome acts as an error fingerprint.
- Syndrome decoding identifies the location and type of a correctable error.
- A suitable Pauli operation is then applied to correct the error.
- The goal is to preserve the logical quantum information while removing the physical error.

 The central idea of the Steane Code can be summarized as:

 $$
\boxed{\text{Seven physical qubits}\rightarrow\text{redundancy}\rightarrow\text{syndrome information}\rightarrow\text{error correction}}
$$

 Quantum error correction therefore allows us to protect fragile quantum information against physical errors without directly measuring the logical quantum state.

---

**Written By** : Shreya Palase(codeQubit)

**Date** : 20-September-2026

Thank You and Enjoy Learning!
