# Day 3 — Quantum Error Correction: 9-Qubit Shor Code

The **9-Qubit Shor Code** is one of the foundational quantum error-correcting codes. It encodes one logical qubit into nine physical qubits and protects that logical information against any arbitrary single-qubit error. The main idea is not to prevent noise, but to add enough structure and redundancy to the quantum state so that an error can be detected indirectly through a **syndrome** and then corrected without directly measuring the unknown quantum information. The Shor Code combines ideas from the three-qubit bit-flip code and phase-flip protection, making it an important first example of a general quantum error-correcting code.

## Topics Covered

- Physical vs logical qubits
- Why quantum error correction is required
- Quantum noise
- Pauli errors
- Bit-flip error
- Phase-flip error
- Bit + phase-flip error
- No-cloning theorem
- 3-qubit repetition code
- 9-qubit Shor Code structure
- Logical states $|0_L\rangle$ and $|1_L\rangle$
- Arbitrary logical state
- Code parameters $[[9,1,3]]$
- Code distance
- Stabilizer formalism
- Syndrome
- Syndrome extraction
- Conditional correction
- General single-qubit errors
- Knill-Laflamme condition
- Stabilizer group
- Logical operators
- Error spaces
- Degeneracy
- Encoding and decoding
- Measurement
- Qiskit implementation terminology
- Visualization
- Key takeaways

---

# 1. Why Quantum Error Correction?

A quantum computer is never perfectly isolated. Interaction with the environment, imperfect gates, measurement errors, control noise, and decoherence can change the quantum state during a computation. Unlike a classical bit, a qubit contains both amplitude and phase information, so protecting it requires more than simply making multiple copies. Quantum error correction solves this problem by encoding one logical qubit into several physical qubits. The extra physical qubits create a protected **code space** in which errors can be detected through additional information called the syndrome.

For an arbitrary qubit,

$$
|\psi\rangle=\alpha|0\rangle+\beta|1\rangle,
$$

with

$$
|\alpha|^2+|\beta|^2=1.
$$

An error can transform it into

$$
|\psi\rangle\rightarrowE|\psi\rangle.
$$

Quantum error correction aims to perform

$$
\boxed{\text{Encode}\rightarrow\text{Noise}\rightarrow\text{Syndrome}\rightarrow\text{Recovery}\rightarrow\text{Decode}}
$$

without directly measuring $\alpha$ and $\beta$.

---

# 2. Physical Qubit vs Logical Qubit

A **physical qubit** is an actual qubit implemented by quantum hardware, while a **logical qubit** is quantum information encoded across multiple physical qubits. The logical qubit is the information we actually want to preserve during a computation. In the Shor Code, nine physical qubits collectively represent one logical qubit. Therefore, an error on one physical qubit does not necessarily destroy the logical information because the remaining structure contains enough information to identify and correct the error.

The original state is

$$
|\psi\rangle=\alpha|0\rangle+\beta|1\rangle.
$$

After encoding,

$$
|\psi\rangle\longrightarrow|\psi_L\rangle=\alpha|0_L\rangle+\beta|1_L\rangle.
$$

Therefore,

$$
1\text{ logical qubit}\longrightarrow9\text{ physical qubits}.
$$

---

# 3. Quantum Noise

Quantum noise represents unwanted physical processes that modify a quantum state. At the mathematical level, a general single-qubit error can be decomposed into the Pauli basis. This is extremely useful because instead of trying to design a code for every possible physical error, we can study the fundamental Pauli errors $X$, $Y$, and $Z$. If a code can correct these basis errors, linearity allows it to correct their arbitrary linear combinations as well.

The Pauli basis is

$$
\{I,X,Y,Z\}.
$$

A general single-qubit operator can be expressed as

$$
E=aI+bX+cY+dZ.
$$

Therefore,

$$
E|\psi\rangle=a|\psi\rangle+bX|\psi\rangle+cY|\psi\rangle+dZ|\psi\rangle.
$$

This decomposition is one of the fundamental mathematical ideas behind quantum error correction.

---

# 4. Pauli Operators

The Pauli operators provide the basic language for describing single-qubit errors. The identity $I$ represents no error, $X$ represents a bit flip, $Z$ represents a phase flip, and $Y$ represents a combination of bit and phase flip. These operators are also important in the stabilizer formalism used to describe the Shor Code.

$$
I=\begin{pmatrix}1&0\\0&1\end{pmatrix},\qquadX=\begin{pmatrix}0&1\\1&0\end{pmatrix}
$$

$$
Y=\begin{pmatrix}0&-i\\i&0\end{pmatrix},\qquadZ=\begin{pmatrix}1&0\\0&-1\end{pmatrix}.
$$

They satisfy

$$
X^2=Y^2=Z^2=I.
$$

Their multiplication relations include

$$
XY=iZ,
$$

$$
YZ=iX,
$$

$$
ZX=iY.
$$

---

# 5. Bit-Flip Error

A bit-flip error changes the computational basis state from $|0\rangle$ to $|1\rangle$ or from $|1\rangle$ to $|0\rangle$. It is represented by the Pauli-$X$ operator. This is the simplest quantum error to visualize because it directly changes the computational value of the qubit.

$$
X|0\rangle=|1\rangle
$$

and

$$
X|1\rangle=|0\rangle.
$$

For

$$
|\psi\rangle=\alpha|0\rangle+\beta|1\rangle,
$$

we obtain

$$
X|\psi\rangle=\alpha|1\rangle+\beta|0\rangle.
$$

Therefore,

$$
\boxed{X=\text{bit flip}}
$$

---

# 6. Phase-Flip Error

A phase-flip error does not change the computational basis value directly. Instead, it changes the relative phase between $|0\rangle$ and $|1\rangle$. This makes phase errors especially important because a computational-basis measurement may not reveal them immediately. The $Z$ operator performs a phase flip.

$$
Z|0\rangle=|0\rangle
$$

$$
Z|1\rangle=-|1\rangle
$$

Therefore,

$$
Z\left(\alpha|0\rangle+\beta|1\rangle\right)=\alpha|0\rangle-\beta|1\rangle.
$$

Thus,

$$
\boxed{Z=\text{phase flip}}
$$

The measurement probabilities remain

$$
P(0)=|\alpha|^2,\qquadP(1)=|\beta|^2,
$$

but the relative phase has changed.

---

# 7. Bit + Phase Flip

The Pauli-$Y$ error can be understood as a combination of bit and phase flips. Mathematically,

$$
Y=iXZ.
$$

Ignoring the physically irrelevant global phase,

$$
Y\sim XZ.
$$

Its action is

$$
Y|0\rangle=i|1\rangle
$$

and

$$
Y|1\rangle=-i|0\rangle.
$$

Therefore, the three fundamental nontrivial Pauli errors are

$$
\boxed{X,\quad Y,\quad Z}
$$

and the Shor Code is designed to correct any one of them occurring on any single physical qubit.

---

# 8. No-Cloning Theorem

In classical error correction, information can be copied and stored redundantly. Quantum information is different because an arbitrary unknown quantum state cannot be perfectly cloned. Therefore, quantum error correction cannot simply perform

$$
|\psi\rangle\rightarrow|\psi\rangle|\psi\rangle|\psi\rangle.
$$

The No-Cloning Theorem states that no universal physical operation can create

$$
|\psi\rangle|\psi\rangle
$$

from

$$
|\psi\rangle|0\rangle
$$

for every unknown state $|\psi\rangle$.

Quantum error correction instead distributes information through **entanglement and encoding**:

$$
|\psi\rangle\rightarrow|\psi_L\rangle.
$$

---

# 9. 3-Qubit Repetition Code

The three-qubit repetition code is the simplest starting point for understanding the Shor Code. It encodes one qubit using three physical qubits so that a single bit-flip error can be detected and corrected. The logical states are represented by three identical computational-basis values, but this is an encoding operation rather than ordinary copying of an unknown quantum state.

The encoding is

$$
|0\rangle\rightarrow|000\rangle
$$

$$
|1\rangle\rightarrow|111\rangle.
$$

Therefore,

$$
|\psi\rangle=\alpha|0\rangle+\beta|1\rangle
$$

becomes

$$
|\psi_L\rangle=\alpha|000\rangle+\beta|111\rangle.
$$

If the second qubit experiences an $X$ error,

$$
|\psi_L\rangle\rightarrow\alpha|010\rangle+\beta|101\rangle.
$$

The redundancy allows the corrupted position to be identified.

However, the repetition code mainly protects against bit flips and does not provide full protection against arbitrary single-qubit errors.

---

# 10. Structure of the 9-Qubit Shor Code

The Shor Code extends the repetition-code idea by arranging nine physical qubits into three groups of three:

$$
(1,2,3),\qquad(4,5,6),\qquad(7,8,9).
$$

Each block provides bit-flip protection, while the relative phase structure across the blocks provides phase-flip protection.

Conceptually:

$$
\boxed{\text{Bit-flip protection}+\text{Phase-flip protection}}
$$

gives

$$
\boxed{[[9,1,3]]}
$$

The code therefore encodes one logical qubit using nine physical qubits.

---

# 11. Logical $|0_L\rangle$

The logical zero state is a superposition of eight computational-basis states. The three blocks each contain the state $(|000\rangle+|111\rangle)$, so the logical information is distributed across the complete nine-qubit system rather than stored in one physical qubit.

$$
|0_L\rangle=\frac{1}{2\sqrt2}(|000\rangle+|111\rangle)(|000\rangle+|111\rangle)(|000\rangle+|111\rangle).
$$

Expanded:

$$
|0_L\rangle=\frac{1}{2\sqrt2}\Big(|000000000\rangle+|000000111\rangle+|000111000\rangle+|000111111\rangle
$$

$$
+|111000000\rangle+|111000111\rangle+|111111000\rangle+|111111111\rangle\Big).
$$

---

# 12. Logical $|1_L\rangle$

The logical one state has the same basic block structure, but the relative signs are different. These signs carry phase information, which is why the Shor Code can also protect against phase-flip errors.

$$
|1_L\rangle=\frac{1}{2\sqrt2}(|000\rangle-|111\rangle)(|000\rangle-|111\rangle)(|000\rangle-|111\rangle).
$$

Expanded:

$$
|1_L\rangle=\frac{1}{2\sqrt2}\Big(|000000000\rangle-|000000111\rangle-|000111000\rangle+|000111111\rangle
$$

$$
-|111000000\rangle+|111000111\rangle+|111111000\rangle-|111111111\rangle\Big).
$$

---

# 13. Arbitrary Logical State

The two logical basis states define the two-dimensional logical code space. Any arbitrary single-qubit state can therefore be encoded by replacing $|0\rangle$ with $|0_L\rangle$ and $|1\rangle$ with $|1_L\rangle$. The logical amplitudes remain $\alpha$ and $\beta$, meaning the quantum information itself is preserved while its physical representation becomes redundant.

$$
|\psi\rangle=\alpha|0\rangle+\beta|1\rangle
$$

becomes

$$
|\psi_L\rangle=\alpha|0_L\rangle+\beta|1_L\rangle.
$$

The logical code space is

$$
\mathcal C=\operatorname{span}\{|0_L\rangle,|1_L\rangle\}.
$$

Thus,

$$
\dim(\mathcal C)=2.
$$

---

# 14. Code Parameters: $[[9,1,3]]$

Quantum stabilizer codes are commonly described using the notation $[[n,k,d]]$. The first number gives the number of physical qubits, the second gives the number of encoded logical qubits, and the third gives the code distance. For the Shor Code, nine physical qubits encode one logical qubit and the distance is three.

$$
[[n,k,d]]
$$

For the Shor Code:

$$
\boxed{[[9,1,3]]}
$$

Therefore,

$$
n=9,\qquadk=1,\qquadd=3.
$$

The number of correctable arbitrary errors is

$$
t=\left\lfloor\frac{d-1}{2}\right\rfloor.
$$

Hence,

$$
t=\left\lfloor\frac{3-1}{2}\right\rfloor=1.
$$

Therefore, the Shor Code corrects any arbitrary **single-qubit error**.

---

# 15. Code Distance

The distance of a quantum error-correcting code measures the minimum weight of a nontrivial logical operation. Intuitively, it tells us how many physical-qubit errors are required before the error can become indistinguishable from a logical operation. A larger distance generally provides stronger protection.

For a stabilizer code,

$$
d=\min_{L\in N(\mathcal S)\setminus\mathcal S}\operatorname{wt}(L).
$$

For Shor:

$$
d=3.
$$

Therefore,

$$
t=\left\lfloor\frac{3-1}{2}\right\rfloor=1.
$$

The code can detect errors of weight up to

$$
d-1=2.
$$

---

# 16. Stabilizer Formalism

The stabilizer formalism provides a compact mathematical description of the code space. A stabilizer is an operator that leaves every valid encoded state unchanged. Instead of explicitly writing all possible encoded states, we can describe the code using a set of operators whose simultaneous $+1$ eigenspace is the logical code space.

For a stabilizer $S$,

$$
S|\psi_L\rangle=|\psi_L\rangle.
$$

A convenient set of eight independent stabilizer generators for the Shor Code is

$$
S_1=Z_1Z_2,\qquadS_2=Z_2Z_3,
$$

$$
S_3=Z_4Z_5,\qquadS_4=Z_5Z_6,
$$

$$
S_5=Z_7Z_8,\qquadS_6=Z_8Z_9,
$$

$$
S_7=X_1X_2X_3X_4X_5X_6,
$$

$$
S_8=X_4X_5X_6X_7X_8X_9.
$$

For every logical code state,

$$
S_i|\psi_L\rangle=|\psi_L\rangle.
$$

---

# 17. Why Stabilizers Detect Errors

The key mathematical mechanism is the commutation relationship between an error and a stabilizer. If an error commutes with a stabilizer, its eigenvalue remains unchanged. If the error anticommutes with the stabilizer, the eigenvalue changes from $+1$ to $-1$. The pattern of changed eigenvalues becomes the error syndrome.

If

$$
SE=ES,
$$

then

$$
SE|\psi_L\rangle=E|\psi_L\rangle.
$$

Therefore,

$$
\lambda=+1.
$$

If

$$
SE=-ES,
$$

then

$$
SE|\psi_L\rangle=-E|\psi_L\rangle.
$$

Therefore,

$$
\lambda=-1.
$$

Hence,

$$
\boxed{\text{commute}\rightarrow +1}
$$

$$
\boxed{\text{anticommute}\rightarrow -1}
$$

---

# 18. Syndrome

A **syndrome** is the classical information obtained from measuring the stabilizers. It does not tell us the values of $\alpha$ and $\beta$; instead, it tells us which error pattern is consistent with the measured stabilizer eigenvalues. This separation between error information and logical information is one of the most important ideas in quantum error correction.

For a stabilizer measurement:

$$
+1\rightarrow0
$$

and

$$
-1\rightarrow1.
$$

For example,

$$
(+1,+1)\rightarrow00
$$

$$
(+1,-1)\rightarrow01
$$

$$
(-1,+1)\rightarrow10
$$

$$
(-1,-1)\rightarrow11.
$$

Therefore,

$$
\boxed{\text{syndrome}=\text{error fingerprint}}
$$

---

# 19. Bit-Flip Syndrome Example

For the first three-qubit block, consider

$$
S_1=Z_1Z_2
$$

and

$$
S_2=Z_2Z_3.
$$

The single-qubit bit-flip errors produce different syndrome patterns:

| Error | Syndrome |
|---|---|
| $I$ | $00$ |
| $X_1$ | $10$ |
| $X_2$ | $11$ |
| $X_3$ | $01$ |

For example, because $X_1$ anticommutes with $Z_1$,

$$
(Z_1Z_2)X_1=-X_1(Z_1Z_2).
$$

Therefore,

$$
S_1\rightarrow-1.
$$

But $X_1$ commutes with $S_2$, so

$$
S_2\rightarrow+1.
$$

Thus,

$$
X_1\rightarrow10.
$$

---

# 20. Phase-Flip Protection

Phase errors cannot always be identified by directly observing computational-basis values. The Hadamard gate changes the basis so that phase-flip behavior becomes related to bit-flip behavior. This basis transformation is central to understanding how the Shor Code protects phase information.

The Hadamard gate is

$$
H=\frac{1}{\sqrt2}\begin{pmatrix}1&1\\1&-1\end{pmatrix}.
$$

It gives

$$
H|0\rangle=|+\rangle,\qquadH|1\rangle=|-\rangle,
$$

where

$$
|+\rangle=\frac{|0\rangle+|1\rangle}{\sqrt2}
$$

and

$$
|-\rangle=\frac{|0\rangle-|1\rangle}{\sqrt2}.
$$

Most importantly,

$$
HXH=Z
$$

and

$$
HZH=X.
$$

Thus,

$$
\boxed{X\leftrightarrow Z}
$$

under a Hadamard basis transformation.

---

# 21. Syndrome Extraction

Directly measuring the data qubits would generally destroy the quantum information we are trying to protect. Instead, auxiliary qubits called **ancillas** interact with the data qubits and become correlated with the stabilizer eigenvalues. Measuring the ancillas then gives the syndrome while leaving the logical amplitudes $\alpha$ and $\beta$ unmeasured.

Conceptually:

$$
|\psi_L\rangle|0\rangle_a\rightarrow\text{entanglement}\rightarrow\text{syndrome information}\rightarrow\text{ancilla measurement}.
$$

The stabilizer eigenvalue is mapped as

$$
+1\rightarrow0
$$

and

$$
-1\rightarrow1.
$$

Therefore,

$$
\boxed{\text{Data qubits}\rightarrow\text{Ancilla}\rightarrow\text{Classical syndrome}}
$$

without directly measuring the logical state.

---

# 22. Conditional Correction

After syndrome extraction, a classical decoder determines which recovery operation should be applied. The recovery operation is chosen based on the syndrome rather than on direct knowledge of the unknown quantum amplitudes. For a single-qubit Pauli error, applying the same Pauli operation again cancels the error because the Pauli operators square to identity.

Let the syndrome be

$$
s.
$$

The decoder produces

$$
s\rightarrow R(s).
$$

For example,

$$
10\rightarrow X_1.
$$

If the actual error was $X_1$,

$$
R(s)E=X_1X_1=I.
$$

Therefore,

$$
R(s)E|\psi_L\rangle=|\psi_L\rangle.
$$

---

# 23. General Single-Qubit Error

A physical error is not necessarily exactly $X$, $Y$, or $Z$. Any single-qubit operator can be decomposed into these basis operators. This is why a code that corrects the Pauli basis errors can also correct an arbitrary single-qubit error. The syndrome measurement separates the different error components into corresponding error subspaces.

Let

$$
E=aI+bX+cY+dZ.
$$

Then

$$
E|\psi_L\rangle=a|\psi_L\rangle+bX|\psi_L\rangle+cY|\psi_L\rangle+dZ|\psi_L\rangle.
$$

If the code corrects

$$
I,\quad X,\quad Y,\quad Z,
$$

then by linearity it corrects

$$
\boxed{aI+bX+cY+dZ}
$$

for an arbitrary single-qubit error.

---

# 24. Knill-Laflamme Condition

The Knill-Laflamme condition gives the general mathematical criterion for whether a set of errors can be corrected by a quantum error-correcting code. The condition says that different correctable errors must not reveal information about which logical state was encoded. Error information can be extracted, but the logical information must remain indistinguishable from the perspective of the error-detection process.

For correctable errors $E_a$ and $E_b$,

$$
\langle i_L|E_a^\dagger E_b|j_L\rangle=C_{ab}\delta_{ij}.
$$

The Kronecker delta is

$$
\delta_{ij}=\begin{cases}1,&i=j,\\0,&i\neq j.\end{cases}
$$

Therefore, for different logical states,

$$
i\neq j\Rightarrow\langle i_L|E_a^\dagger E_b|j_L\rangle=0.
$$

This is the general mathematical foundation behind quantum error correction.

---

# 25. Code Space and Hilbert Space

Nine physical qubits have a Hilbert-space dimension of

$$
2^9=512.
$$

However, the Shor Code does not use the entire physical Hilbert space to store logical information. Only a two-dimensional subspace is used:

$$
\mathcal C=\operatorname{span}\{|0_L\rangle,|1_L\rangle\}.
$$

Therefore,

$$
\dim(\mathcal C)=2.
$$

The encoding can be viewed as

$$
\mathbb C^2\hookrightarrow(\mathbb C^2)^{\otimes9}.
$$

So the physical system has dimension $512$, while the logical information still has only two dimensions.

---

# 26. Stabilizer Group

The stabilizer generators form a group

$$
\mathcal S=\langleS_1,S_2,\ldots,S_8\rangle.
$$

With eight independent generators,

$$
|\mathcal S|=2^8=256.
$$

The physical Hilbert space has dimension

$$
2^9=512.
$$

Therefore,

$$
\dim(\mathcal C)=\frac{512}{256}=2.
$$

Equivalently, for $n$ physical qubits and $r$ independent stabilizers,

$$
k=n-r.
$$

For Shor:

$$
k=9-8=1.
$$

Hence,

$$
\boxed{[[9,1,3]]}
$$

---

# 27. Logical Operators

A logical operator acts on the encoded information in the same way that an ordinary Pauli operator acts on a physical qubit. One possible choice for the Shor Code is a logical $X$ acting across all nine qubits and a logical $Z$ acting on one qubit from each block.

One possible set is

$$
\bar X=X_1X_2X_3X_4X_5X_6X_7X_8X_9
$$

and

$$
\bar Z=Z_1Z_4Z_7.
$$

Their action is

$$
\bar X|0_L\rangle=|1_L\rangle,
$$

$$
\bar X|1_L\rangle=|0_L\rangle,
$$

$$
\bar Z|0_L\rangle=|0_L\rangle,
$$

$$
\bar Z|1_L\rangle=-|1_L\rangle.
$$

They satisfy

$$
\bar X\bar Z=-\bar Z\bar X.
$$

Thus, the logical qubit obeys the same Pauli algebra:

$$
\boxed{\bar X,\bar Z\simX,Z}
$$

---

# 28. Error Space

When no error occurs, the state belongs to the code space $\mathcal C$. When an error $E$ occurs, the state moves into an error space $E\mathcal C$. Syndrome measurement identifies which error space the state occupies, after which recovery maps the state back to the original code space.

No error:

$$
|\psi_L\rangle\in\mathcal C.
$$

After error $E$:

$$
E|\psi_L\rangle\inE\mathcal C.
$$

Recovery:

$$
R(E\mathcal C)\rightarrow\mathcal C.
$$

Conceptually:

$$
\boxed{\mathcal C\rightarrowE\mathcal C\rightarrow\mathcal C}
$$

---

# 29. Degeneracy

Quantum codes can contain different physical errors that have the same effect on the logical code space. This occurs when two errors differ only by a stabilizer. Since stabilizers act as identity on the code space, multiplying an error by a stabilizer does not change its logical action.

Suppose

$$
E_a=SE_b,\qquadS\in\mathcal S.
$$

Then

$$
E_a|\psi_L\rangle=SE_b|\psi_L\rangle.
$$

Because

$$
S|\psi_L\rangle=|\psi_L\rangle,
$$

we obtain

$$
E_a|\psi_L\rangle=E_b|\psi_L\rangle.
$$

Thus, $E_a$ and $E_b$ are physically different but logically equivalent.

---

# 30. Encoding

Encoding converts the original single-qubit state into the nine-qubit logical state. Mathematically, this can be represented by an encoding isometry $V$ that maps the logical Hilbert space into the larger physical Hilbert space.

$$
V:\mathcal H_L\rightarrow\mathcal H_{\text{physical}}.
$$

The logical basis states satisfy

$$
V|0\rangle=|0_L\rangle
$$

and

$$
V|1\rangle=|1_L\rangle.
$$

Therefore,

$$
V|\psi\rangle=|\psi_L\rangle.
$$

---

# 31. Decoding

Decoding reverses the encoding process after error correction. Once the error has been identified and corrected, the logical state can be transformed back into the original single-qubit representation.

The complete process is

$$
|\psi\rangle\xrightarrow{\text{Encode}}|\psi_L\rangle
$$

$$
\xrightarrow{E}E|\psi_L\rangle
$$

$$
\xrightarrow{\text{Syndrome}}s
$$

$$
\xrightarrow{R(s)}R(s)E|\psi_L\rangle
$$

$$
\xrightarrow{\text{Decode}}|\psi\rangle.
$$

For a correctable error,

$$
R(s)E|\psi_L\rangle=|\psi_L\rangle.
$$

---

# 32. Measurement

Syndrome measurement and logical measurement have different purposes. Syndrome measurement determines information about the error, while final logical measurement determines the result of the protected computation. The syndrome should not reveal the unknown logical amplitudes.

For

$$
|\psi\rangle=\alpha|0\rangle+\beta|1\rangle,
$$

computational-basis measurement gives

$$
P(0)=|\alpha|^2
$$

and

$$
P(1)=|\beta|^2.
$$

The objective of error correction is to preserve these logical probabilities despite a correctable physical error.

---

# 33. Physical Error vs Logical Error

A physical error acts on one or more hardware qubits, such as

$$
X_i,\quad Y_i,\quad Z_i.
$$

A logical error changes the encoded information itself.

For example,

$$
\bar X|0_L\rangle=|1_L\rangle.
$$

Therefore,

$$
\text{physical error}\neq\text{necessarily logical error}.
$$

A correctable physical error can be removed:

$$
E|\psi_L\rangle\xrightarrow{R}|\psi_L\rangle.
$$

But an uncorrectable combination of physical errors can become equivalent to a logical operator:

$$
E_{\text{uncorrectable}}\sim\bar X,\bar Z.
$$

---

# 34. Qiskit Implementation Terminology

The complete Qiskit implementation is already provided in the accompanying Jupyter Notebook, so this section intentionally lists only the important implementation terminology rather than repeating the code.

### Quantum Circuit

- `QuantumCircuit`
- `QuantumRegister`
- `ClassicalRegister`

### Qubits

- Data qubits
- Ancilla qubits
- Syndrome qubits
- Logical qubit
- Physical qubits

### Gates

- `h`
- `x`
- `z`
- `cx`
- `barrier`

### State Preparation

- `initialize`
- Logical state preparation
- Encoded state preparation

### Error Correction

- Encoding circuit
- Error injection
- Syndrome extraction
- Syndrome measurement
- Recovery operation
- Conditional correction
- Decoding circuit

### Simulation

- `AerSimulator`
- `transpile`
- `Statevector`
- `Statevector.from_instruction`

### Measurement and Visualization

- `measure`
- `measure_all`
- `plot_histogram`
- `plot_bloch_multivector`
- Circuit visualization

---

# 35. Visualization

The circuit visualization provides the connection between the mathematical description and the physical implementation. When examining the circuit, the important elements are the data qubits, ancillas, encoding operations, inserted error, syndrome extraction, classical syndrome information, conditional recovery, decoding, and final measurement.

The complete conceptual circuit is

$$
\boxed{\text{Input}\rightarrow\text{Encoding}\rightarrow\text{Error}\rightarrow\text{Syndrome Extraction}\rightarrow\text{Recovery}\rightarrow\text{Decoding}\rightarrow\text{Measurement}}
$$

The circuit should therefore be understood as a physical realization of the mathematical process.

---

# 36. Complete Mathematical Picture

The entire Shor Code can be summarized by considering the state at each stage.

### Initial State

$$
|\psi\rangle=\alpha|0\rangle+\beta|1\rangle.
$$

### Encoding

$$
|\psi\rangle\rightarrow|\psi_L\rangle=\alpha|0_L\rangle+\beta|1_L\rangle.
$$

### Error

$$
|\psi_L\rangle\rightarrowE|\psi_L\rangle.
$$

### Syndrome

$$
E|\psi_L\rangle\rightarrows.
$$

### Recovery

$$
s\rightarrowR(s).
$$

Then

$$
R(s)E|\psi_L\rangle=|\psi_L\rangle.
$$

### Decoding

$$
|\psi_L\rangle\rightarrow|\psi\rangle.
$$

Therefore:

$$
\boxed{|\psi\rangle\rightarrow|\psi_L\rangle\rightarrowE|\psi_L\rangle\rightarrows\rightarrowR(s)E|\psi_L\rangle\rightarrow|\psi_L\rangle\rightarrow|\psi\rangle}
$$

---

# 37. Key Mathematical Results

The most important formulas for the Shor Code are:

### Code Parameters

$$
\boxed{[[9,1,3]]}
$$

### Correctable Errors

$$
t=\left\lfloor\frac{d-1}{2}\right\rfloor=1.
$$

### Logical State

$$
|\psi_L\rangle=\alpha|0_L\rangle+\beta|1_L\rangle.
$$

### Stabilizer Condition

$$
S_i|\psi_L\rangle=|\psi_L\rangle.
$$

### Syndrome Detection

$$
SE=ES\Rightarrow+1
$$

$$
SE=-ES\Rightarrow-1.
$$

### Code Space

$$
\mathcal C=\operatorname{span}\{|0_L\rangle,|1_L\rangle\}.
$$

### Stabilizer Dimension

$$
k=n-r=9-8=1.
$$

### Knill-Laflamme Condition

$$
\boxed{\langle i_L|E_a^\dagger E_b|j_L\rangle=C_{ab}\delta_{ij}}
$$

### Logical Operators

$$
\bar X=X_1X_2X_3X_4X_5X_6X_7X_8X_9
$$

$$
\bar Z=Z_1Z_4Z_7.
$$

---

# 38. Final Mental Model

The easiest professional mental model is:

$$
\boxed{\text{Encode}\rightarrow\text{Protect}\rightarrow\text{Detect}\rightarrow\text{Correct}\rightarrow\text{Decode}}
$$

The Shor Code does not eliminate physical noise. Instead, it creates a protected logical representation in which a single physical error produces a measurable syndrome.

The complete idea is:

$$
\boxed{\text{Physical qubits}\rightarrow\text{Encoded logical state}\rightarrow\text{Physical error}\rightarrow\text{Syndrome}\rightarrow\text{Recovery}\rightarrow\text{Logical state preserved}}
$$

The essential facts to remember are:

$$
\boxed{9\text{ physical qubits}}
$$

$$
\boxed{1\text{ logical qubit}}
$$

$$
\boxed{d=3}
$$

$$
\boxed{[[9,1,3]]}
$$

$$
\boxed{\text{Corrects any single-qubit }X,Y,Z\text{ error}}
$$

$$
\boxed{\text{8 independent stabilizer generators}}
$$

$$
\boxed{\text{Syndrome identifies the error without directly measuring the logical state}}
$$

--

**Written By**: Shreya Palase

**Date**: 07-september-2026

Thank you and Keep Learning!

The Shor Code is therefore a foundational bridge from basic quantum mechanics to **stabilizer codes, fault-tolerant quantum computing, logical qubits, syndrome decoding, and modern codes such as the surface code**.
