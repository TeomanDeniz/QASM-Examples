# QASM Examples

* Note: I'll update this readme during my research and tests.

Some exercises for Programing Quantum Computer with QASM Language from **[Quantum Inspire](https://www.quantum-inspire.com/)**.

First of all:

* There are qubits. They are bits of Quantum Computer and work same as original bits. (If we want of course)
* You gonna see this $`∣\psi⟩`$ fromula often. Means our qubit's value. Which also `ψ` can be `0` or `1`. Or **both**... Scary right? Also this formula called **Quantum State Notation**.

## Version Declaration

```qasm
VERSION 1.0
```

This line indicates the version of QASM being used. If you're aiming for more recent features like custom subroutines or control structures, you'd specify a later version, like `3.0`.


## Comment Line

You can create comment lines with `#` character like in original ASM.

```qasm
# This is a comment line
```

## Qubit Declaration

```qasm
QUBITS 3
```

This declares the number of qubits (three) that you'll use in your program.

Like classical bits, a single qubit is the smallest unit of information in a quantum computer, representing either a 0 or a 1.

However, due to superposition, a qubit can represent 0, 1, or both at once in a probabilistic combination. Mathematically, it’s represented as:

$$
∣\psi⟩=\alpha∣0⟩+\beta∣1⟩
$$

where α and β are probability amplitudes, and $`\left|\alpha\right|^{2}+\left|\beta\right|^{2}=1`$

* In QASM, qubits are usually declared as a register, e.g., `qreg q[3];` in QASM 2.0 or 3.0. This line would look like:

```qasm
QREG Q[3];
```

* If syntax doesn't recognize `qubit 3`, we may need to change it to this form to make it compatible with standard QASM (version 1.0).

## Sections

Sections are like comment lines or functions in QASM. They may not work like original ASM but syntax is same.

```qasm
.section_name
	...
```

Also, QASM doesn’t natively support labeled sections.

## Preparation Phase

```qasm
PREP_Z Q[0:2]
```

`PREP_Z` is preparing qubits in the `|0⟩` state using the **Z** operation or as a shorthand for **preparing in the standard state `∣0⟩`** (which is the ground state of a qubit).

And `Q[0:2]` is represents as: `qubit 0`, `qubit 1`, and `qubit 2`.

## Hadamard Gate

```qasm
H Q[0];
```

This command puts your qubit(s) into a superposition state $`\frac{|0> + |1>}{\sqrt{2}}`$.

Without that, our qubit will act like normal bits. (Like 1 or 0).

But with that, our qubit act like it's both 1 or 0.

Now, you may ask: ***WHY???***

Setting a qubit into a **superposition state** is fundamental to how quantum computers work and why they have the potential to outperform classical computers in certain tasks.

For example, we have a `int data[4000000]` and we need to find a single byte from this large data. In a normal code, we actually need to search this byte in a loop one by one. But with this setup, our Quantum Computer finds the data we are looking for **MUTCH mutch faster** than normally (At least tries). Because that qubit is act like it's everywhere.

Also, `22` qubits are enugh for searching a value within **4.000.000** items. ($`2^{22} = `$`4,194,304`)

For another example, let's call the size of our item **`N`** (`for (int index = 0; index < N; index++)`). When we are looking for a data, it's going to be look for this data from an item for **$`O\left(N\right)`$** times. But for qubics with `H` command, it's going to be **$`O\left(\sqrt{N}\right)`$** times which is faster but not instantaneous. ($`\sqrt{4000000}=2000`$)

For more info:

* **Parallelism** (Quantum Parallelism)
  * When a qubit is in a superposition of states (e.g., $`\frac{1}{\sqrt{2}}(∣0⟩ + ∣1⟩)`$), it can represet both the `∣0⟩` and `∣1⟩` states simultaneously.
  * This allows quantum computers to process multiple possibilities at the same time. In classical computing, a bit can only be in one of two states, either 0 or 1, at any given moment. A quantum bit (qubit), on the other hand, can hold an infinite number of potential outcomes, thanks to superposition.
  * For example, a quantum computer with n qubits can represent all $`2^{n}`$ possible states at once, exponentially increasing the amount of information that can be processed simultaneously.
* **Interference**
  * Superposition enables **quantum interference**, where different superpositions of qubits can combine (interfere) with each other.
  * **Constructive interference** can amplify the probability of correct answers, and **destructive interference** can cancel out incorrect ones. Quantum algorithms like **Grover's search algorithm** or **Shor’s factoring algorithm** rely on this interference to find solutions more efficiently than classical methods.
* **Quantum Entanglement**
  * Superposition is often combined with **entanglement**, where qubits are correlated with each other in ways that classical bits cannot be.
  * Entangled qubits are in a superposition of states that are <INS>dependent</INS> on each other, even if the qubits are far apart. This is the foundation of algorithms like **quantum teleportation** and **quantum cryptography**.
  * In many quantum algorithms, superposition and entanglement work together to solve problems that would otherwise be intractable for classical computers.

## **X Gate** (also called Pauli-X Gate or NOT Gate)

X gate is simple flips the value of a qubit.

Like: `|0>` to `|1>` or `|1>` to `|0>`.

For example:

```qasm
# Q[1] IS |0>
X Q[1]
# Q[1] IS |1>
X Q[1]
# Q[1] IS |0>
```

Matrix representation:

$$
X=\left[
\begin{array}{cccc}
0 & 1 \\
1 & 0
\end{array}
\right]
$$

## **Controlled-NOT**

Let's just select `Q[0]` and  `Q[2]` qubits for this command.

```qasm
CNOT Q[0], Q[2]
```

`CNOT` is flips `Q[2]` qubit if `Q[0]` **NOT** `|0>`.

"*Flip*" by I mean, `|0>` to `|1>` or `|1>` to `|0>`.

| CONTROL (Q0) | TARGET (Q2) | RESULT OF (Q2) AFTER `CNOT` |
| -----------: | :---------: | :-------------------------- |
| 0            | 0           | 0                           |
| 0            | 1           | 1                           |
| 1            | 0           | 1                           |
| 1            | 1           | 0                           |
