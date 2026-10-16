CSS [[4,2,2]] Quantum Error Correction
A Mathematical and Theoretical Guide: Beginner to Advanced

Subject: Quantum Information Theory
Topic: Calderbank–Shor–Steane (CSS) Quantum Error-Detecting Code
Code Parameters: [[4,2,2]]
Scope: Theory, mathematical derivations, stabilizer formalism, error syndromes, and logical operators
Audience: Beginner to Advanced

Table of Contents
Introduction and Learning Objectives
Mathematical Prerequisites
Quantum Information and Error Models
Fundamentals of Quantum Error Correction
Classical Coding Theory and CSS Construction
Mathematical Construction of the [[4,2,2]] Code
Stabilizer Formalism
Logical Qubits and Logical Operators
Error Detection and Syndrome Analysis
Mathematical Proof of Code Distance
Quantum Error-Correction Conditions
Limitations and Theoretical Significance
Worked Mathematical Examples
Exercises and Solutions
Summary and Further Reading
1. Introduction and Learning Objectives
1.1 What Is Quantum Error Correction?

Quantum error correction (QEC) is the mathematical framework used to protect quantum information from noise, decoherence, and imperfect quantum operations.

A classical bit stores either $0$ or $1$. Quantum information, however, is represented by a qubit that may occupy a superposition of computational-basis states:

∣ψ⟩=α∣0⟩+β∣1⟩,

where α,β∈C and

∣α∣2+∣β∣2=1.

The amplitudes contain information about measurement probabilities and relative phase. Consequently, protecting a quantum state requires more than simply copying a bit.

Quantum error correction encodes information into a larger physical system and uses carefully chosen measurements to detect errors without directly measuring the encoded logical information.

1.2 What Is the CSS [[4,2,2]] Code?

The CSS [[4,2,2]] code is a quantum error-detecting code belonging to the Calderbank–Shor–Steane family.

Its parameters are

[[n,k,d]]=[[4,2,2]].

Here:

n=4: number of physical qubits.
k=2: number of encoded logical qubits.
d=2: minimum weight of a nontrivial logical Pauli operator.

The code uses four physical qubits to encode two logical qubits. It detects any single-qubit Pauli error, but does not guarantee correction of every arbitrary single-qubit error.

1.3 Learning Objectives

After studying this document, the reader should be able to:

Understand the mathematical representation of quantum states.
Explain bit-flip, phase-flip, and combined Pauli errors.
Understand the relationship between classical binary codes and CSS codes.
Derive the stabilizer generators of the [[4,2,2]] code.
Construct the code space and its encoded basis states.
Identify logical operators and establish their commutation relations.
Calculate error syndromes using stabilizer measurements.
Prove that the code detects single-qubit errors.
Derive the code distance and understand its implications.
Explain the distinction between error detection and error correction.
2. Mathematical Prerequisites
2.1 Complex Amplitudes and Normalization

A quantum state uses complex-valued probability amplitudes.

For a complex number z=a+ib, its magnitude squared is

∣z∣2=z∗z=a2+b2,

where z∗ denotes complex conjugation.

A single-qubit state is

∣ψ⟩=α∣0⟩+β∣1⟩.

The normalization condition is

∣α∣2+∣β∣2=1.

Upon measurement in the computational basis,

P(0)=∣α∣2,P(1)=∣β∣2.

For example,

∣+⟩=∣0⟩+∣1⟩2

has equal probabilities of measuring $0$ and $1$.

The relative phase between amplitudes is also physically meaningful because it affects interference in other measurement bases.

2.2 Dirac Notation

Quantum mechanics commonly uses Dirac notation.

Ket: ∣ψ⟩, representing a state vector.
Bra: ⟨ψ∣, the conjugate transpose of a ket.
Inner product: ⟨ϕ∣ψ⟩.
Tensor product: ∣ψ⟩⊗∣ϕ⟩.

The computational basis vectors are

∣0⟩=(1
0),∣1⟩=(0
1).

A four-qubit system has a Hilbert-space dimension of

dim⁡(H)=24=16.

Its state can therefore be expressed as a linear combination of sixteen computational-basis states.

2.3 Pauli Operators

The single-qubit Pauli operators are

X=(0	1
1	0),Z=(1	0
0	−1),

and

Y=(0	−i
i	0).

The identity operator is

I=(1	0
0	1).
Bit-Flip Operator
X∣0⟩=∣1⟩,X∣1⟩=∣0⟩.

The operator X exchanges the computational-basis states.

Phase-Flip Operator
Z∣0⟩=∣0⟩,Z∣1⟩=−∣1⟩.

The operator Z changes the relative phase between computational-basis components.

Combined Error
Y∣0⟩=i∣1⟩,Y∣1⟩=−i∣0⟩.

Up to an overall phase,

Y∼XZ.

More precisely,

XZ=−iY,ZX=iY.

The Pauli operators form a convenient basis for describing quantum errors.

2.4 Multi-Qubit Operators

For four physical qubits,

XXXX=X⊗X⊗X⊗X,

and

ZZZZ=Z⊗Z⊗Z⊗Z.

An operator such as

X1=X⊗I⊗I⊗I

acts only on the first physical qubit.

The weight of a Pauli operator is the number of non-identity factors. For example,

wt⁡(XIXZ)=2.
2.5 Commutation and Anticommutation

Two operators commute if

AB=BA,

and anticommute if

AB=−BA.

The Pauli operators satisfy

XZ=−ZX.

However, operators acting on different qubits commute. For example,

(X⊗I)(I⊗Z)=(I⊗Z)(X⊗I).

This property is fundamental to constructing compatible stabilizer measurements.

3. Quantum Information and Error Models
3.1 Why Quantum States Require Protection

A quantum system can interact unintentionally with its environment. Such interactions may alter amplitudes, relative phases, or populations.

Common error mechanisms include:

Bit-flip errors.
Phase-flip errors.
Combined bit-and-phase errors.
Amplitude damping.
Dephasing.
Imperfect gates and measurements.

The Pauli operators X, Y, and Z provide a useful framework for analyzing the effects of many quantum noise processes.

3.2 Bit-Flip Errors

A bit-flip error exchanges ∣0⟩ and ∣1⟩.

For a multi-qubit system, an error on physical qubit i is represented by Xi.

For example,

X1∣ψ⟩

represents a bit flip on the first physical qubit.

3.3 Phase-Flip Errors

A phase-flip error changes the relative phase:

α∣0⟩+β∣1⟩⟶α∣0⟩−β∣1⟩.

This error may not change measurement probabilities in the computational basis, but it can change interference and subsequent quantum computations.

A phase-flip error on qubit i is denoted Zi.

3.4 Combined Pauli Errors

The operator Yi represents a combined bit-and-phase error on qubit i.

The single-qubit Pauli error set is

E1={I,X,Y,Z}.

For four physical qubits, the full Pauli group contains all tensor products of these operators, together with their overall phases.

The CSS [[4,2,2]] code detects every non-identity single-qubit Pauli error.

4. Fundamentals of Quantum Error Correction
4.1 Encoding Quantum Information

Encoding maps a logical state into a larger physical system:

∣ψL⟩⟼∣ψencoded⟩.

For the [[4,2,2]] code, two logical qubits are represented using four physical qubits.

The encoded state belongs to a four-dimensional subspace of the sixteen-dimensional physical Hilbert space.

The purpose of the additional physical degrees of freedom is to make certain errors detectable.

4.2 Error Syndromes

A syndrome is a collection of measurement outcomes that indicates how an error interacts with the stabilizer constraints.

For this code, two independent stabilizer generators produce two syndrome bits.

A syndrome identifies an error's commutation pattern, rather than necessarily identifying the precise physical error.

4.3 Code Distance

The code distance d is the minimum weight of a nontrivial logical Pauli operator.

For a code of distance d:

The maximum number of arbitrary errors detectable is

d−1,

and the maximum number of arbitrary errors correctable is

t=⌊d−12⌋.

For d=2, the code detects one arbitrary-qubit error but does not guarantee correction of one arbitrary-qubit error.

5. Classical Coding Theory and CSS Construction
5.1 Classical Binary Codes

A binary vector of length four is written as

x=(x1,x2,x3,x4),xi∈F2.

Here, F2={0,1}, with arithmetic performed modulo two.

For example,

1+1=0(mod2).

A classical parity check determines whether a binary vector satisfies a specified linear constraint.

The parity of a vector can be calculated using

x1+x2+x3+x4(mod2).

Even parity corresponds to a result of zero; odd parity corresponds to a result of one.

5.2 From Classical Parity Checks to Quantum Stabilizers

A CSS code separates quantum checks into two types:

X-type checks: associated with bit-flip errors.
Z-type checks: associated with phase-flip errors.

Given a binary vector x, define

X(x)=∏i=1nXixi.

Similarly,

Z(z)=∏i=1nZizi.

The binary dot product is

x⋅z=∑i=1nxizi(mod2).

The corresponding Pauli operators satisfy

X(x)Z(z)=(−1)x⋅zZ(z)X(x).

Therefore, the X-type and Z-type operators commute precisely when

x⋅z=0(mod2).
5.3 CSS Compatibility Condition

Let HX and HZ denote the binary parity-check matrices that define the two types of stabilizers.

The CSS compatibility condition is

HXHZT=0(mod2).

This condition ensures that every X-type stabilizer commutes with every Z-type stabilizer.

For the [[4,2,2]] code, choose

HX=HZ=(1	1	1	1).

Then

HXHZT=1+1+1+1=0(mod2).

The two checks therefore define a valid commuting stabilizer set.

5.4 Number of Logical Qubits

For a CSS code,

k=n−rank⁡(HX)−rank⁡(HZ).

Both matrices have rank one, so

k=4−1−1=2.

Thus, the code encodes two logical qubits into four physical qubits.

The remaining task is to establish the distance, which will be derived in Section 10.

6. Mathematical Construction of the [[4,2,2]] Code
6.1 Stabilizer Generators

The code is defined by the stabilizer generators

SX=XXXX,SZ=ZZZZ.

The stabilizer group is

S=⟨XXXX,ZZZZ⟩.

The generators are independent and commute. Their products generate the group

S={IIII,XXXX,ZZZZ,YYYY}.

To see why the final element appears, note that

(XXXX)(ZZZZ)=YYYY.

The four factors of −i arising from XZ=−iY multiply to one, so no additional overall minus sign appears.

6.2 Defining the Code Space

The code space consists of states satisfying both stabilizer equations:

SX∣ψL⟩=∣ψL⟩,
SZ∣ψL⟩=∣ψL⟩.

These equations state that every valid code state is a simultaneous +1 eigenstate of both generators.

The code-space projector is

PC=14(I+XXXX)(I+ZZZZ).

Since the full physical Hilbert space has dimension 24=16, and each independent stabilizer halves the allowed dimension,

dim⁡(C)=1622=4.

A four-dimensional quantum space can encode two qubits because

2k=4⟹k=2.
6.3 Deriving the Computational-Basis Codewords

The Z-type stabilizer acts on a computational-basis state as

ZZZZ∣b1b2b3b4⟩=(−1)b1+b2+b3+b4∣b1b2b3b4⟩.

For the eigenvalue to be +1, the bit string must contain an even number of ones.

The X-type stabilizer complements all four bits:

XXXX∣b1b2b3b4⟩=∣1−b1,1−b2,1−b3,1−b4⟩.

A valid codeword must therefore combine an even-parity bit string with its bitwise complement.

One orthonormal logical basis is

∣00L⟩=∣0000⟩+∣1111⟩2,
∣01L⟩=∣0011⟩+∣1100⟩2,
∣10L⟩=∣0101⟩+∣1010⟩2,
∣11L⟩=∣0110⟩+∣1001⟩2.

Each codeword has unit norm, and distinct codewords are orthogonal.

6.4 Verifying a Codeword

Consider

∣00L⟩=∣0000⟩+∣1111⟩2.

Applying SZ=ZZZZ gives

ZZZZ∣0000⟩=∣0000⟩

and

ZZZZ∣1111⟩=(−1)4∣1111⟩=∣1111⟩.

Therefore,

ZZZZ∣00L⟩=∣00L⟩.

Applying SX=XXXX exchanges the two basis states:

XXXX∣0000⟩=∣1111⟩,
XXXX∣1111⟩=∣0000⟩.

Hence,

XXXX∣00L⟩=∣00L⟩.

The other three codewords satisfy the same stabilizer conditions.

6.5 General Encoded State

A general two-logical-qubit state is

∣ψL⟩=a∣00L⟩+b∣01L⟩+c∣10L⟩+d∣11L⟩,

where

∣a∣2+∣b∣2+∣c∣2+∣d∣2=1.

Both stabilizers leave this state unchanged because they leave every basis state unchanged.

7. Stabilizer Formalism
7.1 Stabilizer Definition

A stabilizer is an operator that acts as the identity on the code space.

For the CSS [[4,2,2]] code,

SX∣ψL⟩=∣ψL⟩,SZ∣ψL⟩=∣ψL⟩.

The corresponding measurement outcomes are +1 for ideal code states.

A measurement outcome of −1 indicates a violation of the corresponding stabilizer constraint.

7.2 Stabilizer Measurements

Let the measured eigenvalues be

sX,sZ∈{+1,−1}.

Convert these eigenvalues to binary syndrome bits:

mX={0,	sX=+1,
1,	sX=−1,

and

mZ={0,	sZ=+1,
1,	sZ=−1.

The syndrome is

m=(mX,mZ).

Its possible values are $00$, $10$, $01$, and $11$.

7.3 Why Commutation Determines the Syndrome

Suppose the initial code state satisfies

S∣ψL⟩=∣ψL⟩.

After an error E, the state becomes

∣ψ′⟩=E∣ψL⟩.

If E commutes with S, then

S∣ψ′⟩	=SE∣ψL⟩
	=ES∣ψL⟩
	=E∣ψL⟩
	=∣ψ′⟩.

The stabilizer eigenvalue remains +1.

If E anticommutes with S, then

S∣ψ′⟩	=SE∣ψL⟩
	=−ES∣ψL⟩
	=−∣ψ′⟩.

The measured eigenvalue becomes −1.

Thus, syndrome extraction is fundamentally a measurement of error commutation relations.

8. Logical Qubits and Logical Operators
8.1 Definition

A logical Pauli operator must preserve the code space by commuting with every stabilizer, while acting nontrivially on the encoded information.

One valid choice is

X‾1=XXII,Z‾1=IZIZ,
X‾2=XIXI,Z‾2=IIZZ.

Here, the subscript identifies the logical qubit.

Different choices of logical representatives are possible because multiplying a logical operator by a stabilizer produces an equivalent action on the code space, up to phase.

8.2 Commutation with the Stabilizers

Consider

X‾1=XXII.

It commutes with XXXX because all non-identity overlapping factors are X.

It also commutes with ZZZZ: there are two X-versus-Z anticommutations, producing a net positive sign.

Therefore,

[X‾1,SX]=[X‾1,SZ]=0.

The same method verifies that all four selected logical operators commute with both stabilizers.

8.3 Logical Pauli Algebra

For each logical qubit,

X‾iZ‾i=−Z‾iX‾i.

For different logical qubits, the corresponding operators commute.

For example,

[X‾1,Z‾2]=0,[X‾2,Z‾1]=0.

These are exactly the commutation relations expected of two independent logical qubits.

8.4 Stabilizer Equivalence

If two Pauli operators differ by multiplication by an element of S, they have the same action on the code space up to an overall phase.

For example, L and L(XXXX) are logically equivalent whenever L is a logical operator.

This equivalence is important because the distance is determined by the minimum-weight representative of a nontrivial logical operation.

9. Error Detection and Syndrome Analysis
9.1 Syndrome Table for Single-Qubit Errors

The stabilizers are

SX=XXXX,SZ=ZZZZ.

A single-qubit Xi error commutes with SX but anticommutes with SZ.

A single-qubit Zi error anticommutes with SX but commutes with SZ.

A single-qubit Yi error anticommutes with both.

Using syndrome ordering (mX,mZ):

Error	Relation with SX	Relation with SZ	Syndrome
Xi	Commutes	Anticommutes	$01$
Zi	Anticommutes	Commutes	$10$
Yi	Anticommutes	Anticommutes	$11$

These relations are identical for all four physical qubits.

9.2 Error Detection

Every single-qubit non-identity Pauli error produces a nonzero syndrome.

Therefore, the code detects every single-qubit Pauli error.

This is the principal error-detection property of the [[4,2,2]] code.

9.3 Why the Error Location Is Not Identified

The errors X1 and X2 both produce syndrome $01$ in the ordering used above.

The syndrome distinguishes the error type but not the physical location.

More importantly, their product is

X1X2=XXII=X‾1.

Since X‾1 is a nontrivial logical operator, the two errors differ by a logical operation.

Choosing a recovery operation based only on this syndrome cannot guarantee restoration of every possible original logical state.

9.4 Zero Syndrome Does Not Guarantee an Unchanged Logical State

The logical operator

X‾1=XXII

commutes with both stabilizers.

Consequently, it produces the zero syndrome:

m=(0,0).

Nevertheless, it changes the encoded logical information.

A zero syndrome therefore means that the stabilizer checks detect no violation. It does not prove that the logical state has remained unchanged.

9.5 Syndrome Extraction and Quantum Measurement

In a physical implementation, stabilizers are normally measured indirectly using ancilla qubits and controlled operations.

This approach extracts stabilizer eigenvalues without measuring the individual data qubits in the computational basis.

The distinction matters because direct measurement of the physical qubits would generally destroy the encoded superposition.

The theoretical syndrome is defined by the stabilizer eigenvalues, independently of the particular circuit used to measure them.

10. Mathematical Proof of Code Distance
10.1 Definition of Distance

The distance of a stabilizer code is

d=min⁡L∈N(S)∖Swt⁡(L),

where:

S is the stabilizer group.
N(S) is the Pauli normalizer of the stabilizer group.
wt⁡(L) is the number of non-identity factors in L.

The normalizer contains Pauli operators that commute with every stabilizer.

Operators in the stabilizer group act trivially on the code space. Operators in the normalizer but outside the stabilizer group represent nontrivial logical operations.

10.2 Lower Bound: No Weight-One Logical Operator

Consider an arbitrary single-qubit Pauli error.

If it is Xi, it anticommutes with ZZZZ.
If it is Zi, it anticommutes with XXXX.
If it is Yi, it anticommutes with both stabilizers.

Therefore, every non-identity weight-one Pauli operator fails to commute with at least one stabilizer.

No weight-one Pauli operator belongs to N(S)∖S.

Hence,

d≥2.
10.3 Upper Bound: A Weight-Two Logical Operator Exists

Consider

L=XXII.

It commutes with XXXX, because the overlapping factors are identical Pauli operators.

It also commutes with ZZZZ, because it anticommutes with Z on two positions. The two minus signs cancel.

Thus,

L∈N(S).

However,

XXII∉S.

It acts nontrivially on the encoded information and has weight two.

Therefore,

d≤2.

Combining the two bounds gives

d≥2,d≤2.

Consequently,

d=2.
10.4 Consequences of Distance Two

A distance-two code can detect any error affecting one physical qubit, provided the relevant error lies within the assumed error model.

Its guaranteed correction capacity for arbitrary errors is

t=⌊d−12⌋=⌊12⌋=0.

Thus, the code has single-qubit error-detection capability but no guaranteed correction of an arbitrary single-qubit error.

11. Quantum Error-Correction Conditions
11.1 General Error Operators

A general single-qubit error operator can be expanded in the Pauli basis:

E=aI+bX+cY+dZ,

where the coefficients may be complex.

Quantum noise may therefore produce coherent combinations of Pauli errors, rather than only classical mixtures of individual errors.

A complete quantum error-correction analysis must consider how the code responds to products of possible error operators.

11.2 Knill–Laflamme Conditions

A quantum code with projector P can correct a set of errors {Ea} if and only if

PEa†EbP=CabP

for all pairs of errors Ea,Eb, where Cab are constants independent of the encoded state.

The condition means that the environment or syndrome information must not reveal the encoded logical state in a way that prevents recovery.

For a stabilizer code, the condition can be understood in terms of the products Ea†Eb:

If a product anticommutes with a stabilizer, it maps the code space to an orthogonal syndrome subspace.
If a product belongs to the stabilizer group, it acts trivially on the code space, up to phase.
If a product is a nontrivial logical operator, it acts nontrivially within the code space and can obstruct correction of the proposed error set.
11.3 Applying the Condition to the [[4,2,2]] Code

Suppose the error set includes every single-qubit Pauli error.

The pair

Ea=X1,Eb=X2

gives

Ea†Eb=X1X2=XXII.

Because XXII is a nontrivial logical operator, its restriction to the code space is not proportional to the identity on that space.

Consequently, the Knill–Laflamme condition fails for the full set of arbitrary single-qubit Pauli errors.

This establishes mathematically why the code does not guarantee universal single-qubit correction.

11.4 Error Detection Versus Correction

The distinction can be summarized as follows.

Detection: The syndrome reveals that the state violates one or more stabilizer constraints.
Correction: The available information is sufficient to choose a recovery operation that restores the encoded state for every error in the specified correctable set.

Detection does not require each error to have a unique syndrome. Guaranteed correction generally requires the errors to be distinguishable by their syndrome structure or to be equivalent in their action on the logical space.

12. Limitations and Theoretical Significance
12.1 Limitations

The CSS [[4,2,2]] code:

Detects all single-qubit Pauli errors.
Does not identify every error's physical location.
Does not guarantee correction of arbitrary single-qubit errors.
Can be affected by undetected weight-two logical operators.
Does not, by itself, provide a complete fault-tolerant computation architecture.

Its value lies in its compact mathematical structure and its ability to demonstrate the central concepts of stabilizer-based error detection.

12.2 Why the Code Is Theoretically Useful

Despite its small size, the code illustrates several fundamental principles:

Quantum information can be encoded into a subspace rather than stored in individual physical qubits.
Classical parity checks can be translated into commuting quantum stabilizers.
Stabilizer measurements detect errors through commutation relations.
Logical operators preserve the code space but can change encoded information.
Code distance determines the guaranteed error-detection and error-correction capabilities.
12.3 Connection to Larger CSS Codes

The same mathematical framework extends to larger quantum codes.

For larger codes, the matrices HX and HZ contain multiple parity-check rows, and their ranks determine the number of logical qubits.

The CSS compatibility condition remains

HXHZT=0(mod2).

The challenge is to choose checks that provide a larger minimum distance while allowing practical encoding, syndrome measurement, and decoding.

12.4 Fault Tolerance

Fault tolerance addresses errors that occur during the protection process itself, including faults in:

Ancilla preparation.
Syndrome measurements.
Quantum gates.
State preparation.
Recovery operations.

A fault-tolerant design must prevent a small number of physical faults from spreading into damaging logical errors.

The [[4,2,2]] code is an introductory example of error detection, not a standalone solution to the full fault-tolerance problem.

13. Worked Mathematical Examples
Example 1: Verify a Codeword

Consider

∣00L⟩=∣0000⟩+∣1111⟩2.

Applying ZZZZ gives eigenvalue +1 on both components. Applying XXXX exchanges the two components without changing their sum.

Therefore,

SX∣00L⟩=∣00L⟩,
SZ∣00L⟩=∣00L⟩.

The state belongs to the code space.

Example 2: Syndrome of an X Error

Consider E=X1.

Since X1 commutes with XXXX and anticommutes with ZZZZ,

sX=+1,sZ=−1.

The syndrome is

(mX,mZ)=(0,1).

The bit-flip error is detected.

Example 3: Syndrome of a Z Error

Consider E=Z3.

It anticommutes with XXXX and commutes with ZZZZ, giving

sX=−1,sZ=+1.

Thus,

(mX,mZ)=(1,0).

The phase-flip error is detected.

Example 4: Syndrome of a Y Error

Consider E=Y2.

The operator anticommutes with both stabilizers:

sX=−1,sZ=−1.

Therefore,

(mX,mZ)=(1,1).

Both stabilizer constraints are violated.

Example 5: An Undetected Logical Operation

Consider

L=XXII.

It commutes with both stabilizers, so

(mX,mZ)=(0,0).

Nevertheless, L=X‾1 acts nontrivially on the first logical qubit.

This demonstrates that a zero syndrome does not guarantee an unchanged logical state.

Example 6: Dimension of the Code Space

Four physical qubits have dimension

24=16.

Each independent stabilizer constraint halves the allowed dimension.

With two independent generators,

dim⁡(C)=1622=4.

Since 4=22, the code encodes two logical qubits.

14. Exercises and Solutions
Exercise 1: Pauli Algebra

Show that

XZ=−ZX.
Solution

Direct multiplication gives

XZ=(0	−1
1	0),ZX=(0	1
−1	0).

Therefore,

XZ=−ZX.
Exercise 2: CSS Compatibility

Verify the CSS condition for

HX=HZ=(1	1	1	1).
Solution
HXHZT=1+1+1+1=0(mod2).

The corresponding stabilizers commute.

Exercise 3: Code-Space Dimension

Calculate the dimension of the code space.

Solution

For n=4 physical qubits and r=2 independent stabilizer generators,

dim⁡(C)=2n−r=24−2=4.
Exercise 4: Error Syndrome

Determine the syndrome of Z2 using the ordering (mX,mZ).

Solution

Z2 anticommutes with XXXX and commutes with ZZZZ.

Thus,

(mX,mZ)=(1,0).
Exercise 5: Logical Operator Verification

Show that XXII commutes with both stabilizers.

Solution

It commutes with XXXX because the overlapping operators are all X. It commutes with ZZZZ because two anticommutations produce a positive sign.

Thus, it preserves the code space.

Exercise 6: Code Distance

Prove that the code distance is two.

Solution

Every weight-one non-identity Pauli operator anticommutes with at least one stabilizer. However, XXII is a weight-two nontrivial logical operator that commutes with both stabilizers.

Therefore,

d=2.
Exercise 7: Error-Correction Conditions

Explain why X1 and X2 prevent guaranteed correction of all single-qubit Pauli errors.

Solution

Their product is

X1†X2=XXII=X‾1.

Because X‾1 is nontrivial on the code space, the Knill–Laflamme conditions fail for the complete set of single-qubit Pauli errors.

15. Summary and Further Reading
15.1 Key Mathematical Results
Property	Result
Code parameters	[[4,2,2]]
Physical qubits	$4$
Logical qubits	$2$
Stabilizer generators	XXXX, ZZZZ
Independent generators	$2$
Code-space dimension	$4$
Code distance	$2$
Single-qubit Pauli error detection	Yes
Guaranteed arbitrary single-qubit correction	No
Minimum logical operator weight	$2$
15.2 Final Conceptual Understanding

The CSS [[4,2,2]] code encodes two logical qubits into four physical qubits by restricting the system to a subspace defined by two commuting stabilizer generators.

The stabilizers detect single-qubit Pauli errors through changes in their measurement eigenvalues. Logical operators commute with the stabilizers while acting nontrivially on the encoded information.

The code's distance is two because no weight-one Pauli operator acts as an undetected logical operation, while weight-two logical operators exist.

The essential conclusion is:

The CSS [[4,2,2]] code demonstrates quantum error detection, but its distance is insufficient to guarantee correction of every arbitrary single-qubit error.
15.3 Further Reading
Calderbank and Shor, Good Quantum Error-Correcting Codes Exist (1996).
Steane, Error Correcting Codes in Quantum Theory (1996).
Gottesman, Stabilizer Codes and Quantum Error Correction (1997).
Nielsen and Chuang, Quantum Computation and Quantum Information.
IBM Quantum Learning: stabilizer codes and quantum error correction.
Qiskit documentation: quantum error correction and quantum circuits.

Scope note: This document intentionally excludes programming implementations, software dependencies, and execution instructions. Those belong in the separate implementation file.
