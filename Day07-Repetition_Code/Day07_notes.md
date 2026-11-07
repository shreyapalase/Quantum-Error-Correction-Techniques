# Day 07 Notes - Generalized Quantum Repetition Code(five qubit)

---

**Subject** : Quantum Computing | Quantum Error Correction (QEC)

**Level** : Beginner → Intermediate → Advanced

**Focus** : Theory, mathematics, encoding, stabilizers, syndrome decoding, and logical-state measurement

---

## 1. Introduction: Why Do We Need Quantum Error Correction?

Quantum computers store information in quantum bits (qubits). Unlike classical bits, qubits can exist in superposition:

An arbitrary quantum state is:

The state is $|\psi\rangle = \alpha|0\rangle + \beta|1\rangle $.
where:

$ ∣\alpha∣^2+∣\beta∣^2=1 $

However, quantum information is fragile. Interactions with the environment, imperfect gates, and measurement operations can introduce errors.
A quantum error-correction code distributes logical information across multiple physical qubits so that errors can be detected and corrected without directly measuring the encoded quantum information.

**Core idea** : Protect one logical qubit using multiple physical qubits.

## 2. Generalized Quantum Repetition Encoding
### 2.1 What is a repetition code?

A repetition code is one of the simplest error-correction techniques.In classical computing, a bit can be repeated several times:

0→00000
1→11111

If one bit changes during transmission, majority voting can recover the original value.

For example:

00000→00100

The received sequence contains three zeros and two? No: it contains four zeros and one one, so the original logical zero is recovered.
Quantum repetition encoding extends this idea to quantum states.

### 2.2 Generalized n-qubit repetition encoding

For an n-qubit repetition code, define:

$ ∣0_L\rangle=∣0\rangle⊗n $
$ ∣1_L\rangle=∣1\rangle⊗n $

The encoded logical quantum state becomes:

$ ∣\psi_L⟩=\alpha∣0\rangle⊗n+\beta∣1\rangle⊗n $

Here:

n: Number of physical qubits.
$∣0_L\rangle $ : Logical zero.
$∣1_L\rangle $: Logical one.
α,β: Probability amplitudes.
$∣\psi_L\rangle $ : Encoded logical qubit.
Example: Three physical qubits
$∣\psi_L\rangle =\alpha∣000\rangle+\beta∣111\rangle $ 
Example: Five physical qubits
$∣\psi_L\rangle =\alpha∣00000\rangle+β∣11111\rangle $

The five physical qubits collectively represent one logical qubit.

### 2.3 How many errors can it correct?

For a repetition code with odd length n, the maximum number of correctable bit-flip errors is:

t=⌊n−12⌋
Physical qubits	Correctable bit flips
3	1
5	2
7	3
9	4
11	5

Remember: Increasing the number of physical qubits increases protection against bit flips, but does not automatically provide protection against phase flips.

## 3. Relationship with the Three-Qubit Bit-Flip Code

The three-qubit bit-flip code is the simplest quantum repetition code.

It encodes:

$∣0_L⟩$=∣000⟩
$∣1_L⟩$=∣111⟩

An arbitrary logical state is encoded as:

$∣ψ_L⟩$=α∣000⟩+β∣111⟩

### 3.1 Understanding the encoding

Suppose the original qubit is:

∣ψ⟩=α∣0⟩+β∣1⟩

After encoding:

∣ψ⟩→α∣000⟩+β∣111⟩

Notice that the amplitudes are preserved.

The quantum information is distributed across three physical qubits.

### 3.2 What happens when an error occurs?

Consider a bit-flip error on the second qubit:

X2∣ψL⟩=α∣010⟩+β∣101⟩

The encoded state is no longer in its original form.

However, the error can be identified using parity measurements.

### 3.3 Three-qubit versus five-qubit repetition code

|Property|	Three-qubit code	|Five-qubit code|
|----|----|----|
|Physical qubits|	3 |	5|
|Logical qubits	|1|	1 |
|Correctable X errors|	1 |	2|
|Stabilizer generators|	2 |	4 |
|Redundancy	|Lower|	Higher|
|Phase-flip protection	|No|	No|


**Key takeaway** : The five-qubit repetition code extends the three-qubit bit-flip code by increasing its ability to correct multiple bit-flip errors.

## 4. Five-Qubit Quantum Repetition Code Construction

### 4.1 Logical encoding

The five-qubit repetition code uses five physical qubits to represent one logical qubit.

Its logical basis states are:

∣0L⟩=∣00000⟩
∣1L⟩=∣11111⟩

The encoded logical state is:

∣ψL⟩=α∣00000⟩+β∣11111⟩

This encoding preserves the original quantum amplitudes.

### 4.2 Why five qubits?

A majority-voting system can recover the correct logical value when fewer than half the physical bits are corrupted.

For five qubits:

0 errors: Correct.
1 error: Correct.
2 errors: Correct.
3 errors: Majority decoding fails.
4 errors: Majority decoding fails.
5 errors: The encoded logical value is inverted.

Therefore:

t=⌊5−12⌋=2

The code can correct up to two bit-flip errors.

### 4.3 Conceptual encoding diagram
∣ψ⟩
↓
Quantum Encoder
↓
q1q2q3q4q5
↓
α∣00000⟩+β∣11111⟩

All five qubits collectively store one logical quantum state.

## 5. Stabilizer Generators and Parity Checks

This is one of the most important concepts in quantum error correction.

### 5.1 What is a stabilizer?

A stabilizer is an operator that leaves the encoded logical state unchanged.
If S is a stabilizer operator, then:

$S∣ψ_L⟩=∣ψ_L⟩$

The stabilizers describe the mathematical structure of the code space.

For the five-qubit bit-flip repetition code, the stabilizer generators are:

S1=Z1Z2
S2=Z2Z3
S3=Z3Z4
S4=Z4Z5

Here, $Z_i$ represents a Pauli-Z operator acting on physical qubit i.

The remaining qubits are acted on by the identity operator.

### 5.2 Why use Z operators to detect X errors?

Recall:

Z∣0⟩=∣0⟩
Z∣1⟩=−∣1⟩

For a pair of equal computational-basis bits:

$ Z_i Z_i+1∣00⟩=∣00⟩ $
$ Z_i Z_i+1∣11⟩=∣11⟩ $

Therefore, both logical basis states have stabilizer eigenvalue +1.

However, if one of the qubits flips, the corresponding pair has opposite values.

For example:

∣00⟩→∣10⟩

Then:

$ Z_i Z_i+1∣10⟩=−∣10⟩ $

This produces a different measurement outcome.

**Important principle** : Z-type parity checks detect X-type bit-flip errors because Pauli X and Z anticommute.

XZ=−ZX

### 5.3 Stabilizer generator table
|Generator|	Operator|	Checks |
|----|----|----|
|S1|	Z1Z2|	Qubit 1 and 2 parity|
|S2|	Z2Z3|	Qubit 2 and 3 parity|
|S3|	Z3Z4|	Qubit 3 and 4 parity|
|S4|  Z4Z5|	Qubit 4 and 5 parity|

There are four independent stabilizer generators.

For a stabilizer code:

k=n−r

where:

n: Physical qubits.
r: Independent stabilizer generators.
k: Logical qubits.

For this code:

k=5−4=1

Therefore, five physical qubits encode one logical qubit.

## 6. Syndrome Measurement and Error Detection

### 6.1 What is a syndrome?

A syndrome is a collection of measurement outcomes from stabilizer operators.
Each stabilizer measurement returns one of two eigenvalues:

+1 or −1

For easier interpretation, convert them into binary values:

+1 → 0
−1 → 1

The five-qubit repetition code has four syndrome bits:

s=(s1,s2,s3,s4)

where:

$s_i$ ={0	if Si returns +1
1	if $S_i$ returns −1

The syndrome identifies the error pattern without requiring direct measurement of the logical amplitudes.

### 6.2 Syndrome calculation

Suppose an error operator E acts on the encoded state.
The syndrome is determined by:

$ S_iE∣ψ_L⟩=(−1)s_iE∣ψ_L⟩ $

If the stabilizer commutes with the error, the syndrome bit is zero.
If it anticommutes with the error, the syndrome bit is one.

### 6.3 Single-qubit X-error syndrome table
$$
|Error|	S1|	S2	|S3|	S4|	Syndrome|
|----|----|----|----|----|----|
|None|	0	|0	|0	|0|	0000|
|X1|	1|	0|	0|	0|	1000|
|X2	|1	|1	|0	|0	|1100|
|X3|	0|	1|	1|	0|	0110|
|X4|	0	|0	|1	|1	|0011|
|X5|	0	|0	|0	|1	|0001|
$$
The syndrome tells us which physical qubit experienced a single bit-flip error.

### 6.4 Understanding two-qubit errors

One advantage of the five-qubit repetition code is that it can also correct two bit-flip errors.

For example:

E=X1X2

Its syndrome is:

1000⊕1100=0100

Similarly:

E=X2X4

Its syndrome is:

1100⊕0011=1111

The syndrome patterns are unique for all error combinations containing zero, one, or two X errors.

There are:

(50)+(51)+(52)
=1+5+10=16

possible patterns.

Four syndrome bits provide:

24=16

different syndrome values.

This is exactly enough to distinguish every bit-flip error pattern of weight zero, one, or two.

## 7. Syndrome-Controlled Error Recovery

Detecting an error is not sufficient. We must also correct it.
The recovery process follows three stages:

1.Measure stabilizer operators.
2.Determine the error from the syndrome.
3. Apply the appropriate recovery operation.

### 7.1 Mathematical recovery

If the detected error is E, the recovery operator is:

R=E†

For Pauli X errors:

X†=X

Therefore:

R=X

For example, if the error is:

E=X2

The recovery operator is:

R=X2

The combined operation becomes:

RE=X2X2=I

The original encoded state is restored.

### 7.2 Recovery examples
$$
|Detected |error	|Syndrome	Recovery|
|----|----|----|
|None|	0000|	Identity|
|X1|	1000|	X1|
|X2	|1100|	X2|
|X3	|0110|	X3|
|X4|	0011|	X4|
|X5	|0001|	X5|
|X1X2|	0100|	X1X2|
|X2X4	|1111|	X2X4|
$$

The recovery operation depends on the syndrome-to-error lookup.

**Advanced note** : In a real quantum error-correction system, syndrome extraction is usually performed using ancilla qubits and controlled operations. The encoded data is not directly measured in the computational basis.

## 8. Measurement and Logical-State Comparison

Understanding measurement is essential because quantum information cannot be copied or directly inspected like classical information.

## 8.1 Physical measurement versus logical measurement

A physical qubit measurement observes one physical qubit.
A logical measurement extracts information about the encoded logical qubit.
For the five-qubit repetition code:

$∣0_L⟩$ =∣00000⟩
$∣1_L⟩$ =∣11111⟩

To measure the logical Z observable, measure the physical qubits in the computational basis and perform majority voting.

The logical operator is:

Z‾=Z1

Within the code space, the equivalent operators are:

Z‾=Z1=Z2=Z3=Z4=Z5

The equality is understood modulo stabilizer operators.

### 8.2 Logical measurement example

Suppose the physical measurement produces:

00100

There is one bit-flip deviation from the logical zero codeword.

Majority voting gives:

$0_L$

Similarly, if the result is:

11011

Majority voting gives:

$1_L$

### 8.3 Logical-state comparison
$$	
|Physical qubit|	|One physical qubit|	Five physical qubits
|	Logical qubit|Logical zero	$|0\rangle$|Logical one	$|1\rangle$|
|State measurement|	Single-qubit measurement|	Encoded measurement|
|Error protection	|None |by itself	Up to two bit flips|
|Information storage|	Individual qubit|	Collective encoded state|
$$

### 8.4 Important measurement limitation

Measuring all five physical qubits in the computational basis destroys the superposition between logical zero and logical one.

For example:

∣ψL⟩=α∣00000⟩+β∣11111⟩

A full computational-basis measurement gives either:

∣00000⟩

or:

∣11111⟩

with probabilities:

P($0_L$)=∣α∣^2
P($1_L$)=∣β∣^2

This is why stabilizer syndrome extraction is different from directly measuring the data qubits.

Remember: Error detection should preserve the encoded quantum information. Direct logical-state measurement intentionally extracts information and collapses the measured state.

## 9. Logical Operators of the Five-Qubit Repetition Code

Logical operators describe how quantum information is manipulated at the encoded level.

For the five-qubit bit-flip repetition code:

### 9.1 Logical X operator

The logical X operator flips every physical qubit:

X‾=X1X2X3X4X5

Its action is:

X‾∣0L⟩=∣1L⟩
X‾∣1L⟩=∣0L⟩

### 9.2 Logical Z operator

A logical Z can be represented by a Z operation on any one physical qubit:

Z‾=Z1

Equivalently:

Z‾=Z2=Z3=Z4=Z5

within the encoded code space.

Its action is:

Z‾∣0L⟩=∣0L⟩
Z‾∣1L⟩=−∣1L⟩

Therefore:

Z‾∣ψL⟩=α∣0L⟩−β∣1L⟩

This demonstrates an important limitation: a single physical Z error acts as a logical phase flip.

## 10. Bit-Flip Three-Qubit Code vs Five-Qubit Repetition Code
$$
|Feature	|Three-qubit bit-flip code	|Five-qubit repetition code|
|----|----|----|
|Code notation|	[[3,1,1]]	|[[5,1,1]]|
|Physical qubits|	3|	5|
|Logical qubits	|1	|1 |
|Stabilizers|	Z1Z2,Z2Z3	Z1Z2|Z2Z3,Z3Z4,Z4Z5|
|Syndrome bits	|2	|4|
|Correctable X errors|	1|	2|
|Phase error correction|	No|	No|
|Logical X	|X1X2X3|	X1X2X3X4X5|
|Logical Z	|Z1|	Z1|
|Physical overhead	|3:1|	5:1|
|Main advantage|	Simplicity	|Greater bit-flip tolerance|
$$

Notation clarification: Both codes have distance one against arbitrary quantum errors because a single physical Z error acts as a logical Z operation. Their bit-flip distance is three and five, respectively.

The notation [[n,k,d]] describes a general quantum code's ability to correct arbitrary Pauli errors, not just one selected error type.

## 11. Can the Five-Qubit Repetition Code Correct Phase Errors?

This is an important advanced concept.

The five-qubit repetition code detects X errors using Z-type stabilizers.

However, it does not protect against Z errors.

Consider:

Z1∣ψL⟩
=α∣00000⟩−β∣11111⟩

The result is still inside the encoded code space, but the relative phase has changed.

All stabilizer measurements still return +1.

Therefore, the phase error remains undetected.

### 11.1 How can phase errors be handled?
A complementary phase-flip repetition code can be constructed using the X basis.

Define:

∣+⟩=∣0⟩+∣1⟩2
∣−⟩=∣0⟩−∣1⟩2

Then encode:

∣0L⟩=∣+++++⟩
∣1L⟩=∣−−−−−⟩

This construction detects phase-flip errors using X-type parity checks.

### 11.2 General error protection

A general quantum error can be represented as:

E=aI+bX+cY+dZ

The five-qubit bit-flip repetition code only provides protection against the X-error component.

For comprehensive protection against arbitrary single-qubit errors, more advanced codes are required.

Examples include:

-Shor nine-qubit code.
-Steane seven-qubit code.
-Five-qubit perfect code.
- Surface code.

## 12. Where Can Quantum Repetition Codes Be Applied?

Quantum repetition codes are especially useful for understanding and demonstrating the fundamental principles of quantum error correction.

1. Quantum computing education
- Understand logical qubits.
- Learn quantum encoding.
- Study Pauli errors.
- Understand syndrome extraction.
- Learn stabilizer formalism.

2. Quantum communication

Repetition-based encoding illustrates how redundant quantum information can improve resilience against selected noise processes.
However, the code alone does not protect against arbitrary communication noise.

3. Quantum hardware experiments

Useful for studying:

- Physical qubit bit-flip rates.
- Error detection.
- Syndrome measurement.
- Recovery operations.
- Logical versus physical error rates.

4. Quantum error-correction research

The repetition code provides a simple test environment for developing and evaluating decoding methods.

5. Building blocks for advanced QEC

Repetition codes are conceptually important in understanding larger quantum error-correction constructions, including concatenated codes and surface-code architectures.

Practical limitation: A five-qubit repetition code is not a complete fault-tolerant quantum computing solution. It protects only against a restricted error model.



## Test your understanding before moving to implementation or advanced codes.

**Q1. What is the purpose of quantum repetition encoding?**

Answer: To distribute logical quantum information across multiple physical qubits so that selected errors can be detected and corrected.

**Q2. How many logical qubits are encoded in the five-qubit repetition code?**

Answer: One logical qubit.

**Q3. How many independent stabilizer generators are required?**

Answer: Four.

**Q4. Which Pauli errors can this code correct?**

Answer: Up to two physical bit-flip (X) errors.

**Q5. Why are Z-type stabilizers used?**

Answer: Because they anticommute with X errors and therefore reveal bit-flip error syndromes.

**Q6. Does this code correct phase-flip errors?**

Answer: No. A single Z error acts as a logical phase flip.

**Q7. What does a syndrome represent?**

Answer: A collection of stabilizer measurement outcomes that identifies the error pattern.

**Q8. Why should we avoid directly measuring all data qubits during syndrome extraction?**

Answer: Direct measurement can destroy the encoded superposition and collapse the logical state.

## 15. Final Conceptual Summary

The generalized quantum repetition code is a fundamental introduction to quantum error correction.
The progression is:

- Quantum information: Understand superposition and quantum noise.
- Repetition encoding: Distribute one logical qubit across multiple physical qubits.
- Three-qubit code: Learn how one bit-flip error can be corrected.
- Five-qubit code: Increase bit-flip protection to two errors.
- Stabilizers: Define the valid encoded code space.
- Parity checks: Detect deviations from the code space.
- Syndrome decoding: Identify the error using measurement outcomes.
- Recovery: Apply the inverse error operation.
- Logical measurement: Extract encoded information without confusing it with syndrome measurement.
- Advanced QEC: Understand why more comprehensive codes are required to protect against arbitrary quantum errors.
- The most important takeaway
- Encoding→Error→Syndrome→Recovery

The five-qubit repetition code demonstrates how quantum information can be protected against multiple bit-flip errors using redundant encoding and stabilizer measurements.
However, true general-purpose quantum error correction requires protection against both bit-flip and phase-flip errors.
Next learning milestone: Understand the Shor nine-qubit code and the five-qubit perfect code to see how quantum error correction progresses from protection against individual error types to protection against arbitrary single-qubit errors.

---

**Written By** : Shreya Palase(codeQubit)

**Date** : 03-october-2026

Thank you and Keep Learning!
