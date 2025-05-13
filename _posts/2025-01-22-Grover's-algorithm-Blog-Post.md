---
layout: post
title: Grover dynamics for speeding up optimization
date: 2025-01-22
description: by Kevin Lee
comments: true
tags: ["quantum", "algorithms", "optimization", "hamiltonian"]
---

# 1. Getting started

Optimization problems are ubiquitous in both classical and quantum computing. In quantum mechanics, optimization arises in the **_search_** for a ground state of a Hamiltonian, the energy operator, which captures the low-temperature physics of the system. This search for minimum-energy states extends far beyond quantum systems: we encounter similar optimization challenges when minimizing cost functions in many different scenarios, from finding the shortest route in the Traveling Salesman Problem (TSP) to optimizing supply chain networks. This raises two compelling questions: What is the relationship between **_Grover's search_** and **_Hamiltonian evolution_**, and can we leverage this connection to achieve quadratic speedup in finding optimal states?

One way a quantum computer can outperform a classical computer is its ability to solve problems at a scale beyond brute-force classical simulations. In particular, Grover's algorithm is a perfect quantum search algorithm with its superiority in searching through an unstructured database compared to classical computations. Having delved deeper into the algorithm under the hood and read this insightful paper by Stephen Fenner (Department of Computer Science and Engineering University of South Carolina), I was able to analogize how Grover's algorithm searched through an unstructured database to how a simple Hamiltonian progressively finds a solution (Fenner, 2000).

# 2. Grover's algorithm

## 2.1 High-level overview of Grover's algorithm

**_Let's first break down Grover's algorithm_**. Grover's algorithm is a well-known search algorithm with a quadratic speed-up. The algorithm has two main parts: initialization and Grover's operator. While it is typical to compartmentalize the algorithm into three parts (i.e. initialization, Grover's oracle, and diffusion operator), I will combine the last two parts into one for a more intuitive visualization. The initialization step creates an equal superposition of all possible states, effectively preparing the quantum system to explore the entire search space simultaneously. The Grover operator then iteratively applies two key actions: it marks the target state by flipping its phase and amplifies its amplitude through interference, making it more likely to be measured. Section 2.2 handles a mathematical low-level breakdown of Grover's algorithm, while Section 2.3 offers a more conceptual low-level deep dive into Grover's algorithm using operator notation.

## 2.2 Low-level (mathematical) overview of Grover's algorithm

Initialization is an inherent simple step. We initialize a superposition state that uniformly covers all possible values |x\rangle within the search space of $$N=2^n$$ total elements by applying one layer of Hadamard gates $$H^\{\otimes n}$$ to each n number of qubits (Eq. 1). This produces an equal amplitude superposition over computational basis states labeled by bit strings, i.e.
\[
H^{\otimes n}|0\rangle^{\otimes n}=\frac{1}{\sqrt{2^n}} \sum{x=0}^{2^n-1} |x\rangle \tag{1}
\]
where x is a bit string. Initialization aims to prepare a state where all solutions are equally likely, allowing the algorithm to leverage quantum interference to amplify the amplitude of the marked state through Grover's operator, as mentioned in the earlier section. This step aims to prepare the states before applying Grover's operator, which has a circuit depth of one, independent of the number of qubits n.

The primary complexity in Grover's search algorithm originates from Grover's operator G. This operator comprises two structures: Grover's oracle O and the diffusion operator D. Grover's oracle is a unitary operator that recognizes and marks the target state/solutions to the search problem by applying a phase shift of -1 to the solution space. In Equation 2.1, |x\rangle is the state representing one of the possible states of the qubits, |q\rangle is the ancillary qubit (typically initialized to |0\rangle or |1\rangle), and $$f(x)$$ is the oracle function that defines the search problem where we look for bit strings $$x$$ that satisfy $$f(x)=1$$. Grover's oracle then applies a phase shift to this space, which we can implement using the ancilla qubit via
\[
O|x\rangle|q\rangle=|x\rangle|q\oplusf(x)\rangle \tag{2.1}
\]
Equation 2.1, which represents the oracle using an ancilla qubit, comes in handy when designing and coding a circuit that uses Grover's algorithm (Bartschi & Eidenbenz, 2020). The oracle function is very abstract, and it turns out that by leveraging the ancilla while designing the circuit, we can get a better idea of how computationally expensive applying an oracle to the states can be. For instance, Figure 1 depicts my implementation of Grover's algorithm on a 5-qubit system with four iterations of Grover's operator G (which will be covered in the upcoming few paragraphs).

<p align="center">
    <img src="https://imgur.com/0ZHBQbt" width="75%"/>
    Figure 1
</p>

However, we do not have to use an ancilla qubit to execute Grover's search at an abstract level since we simply need to apply the oracle function when doing mathematical computations. Hence, it is easier to mathematically represent the computation of an oracle function by defining the transformation shown in Equation 2.2 instead of trying to describe an ancilla qubit mathematically. By definition, $$f(x)=1$$ for the marked state(s), and $$f(x)=0$$ for all other states.
\[
|x\rangle=(-1)^{f(x)}|x\rangle \tag{2.2}
\]
This way, the oracle computation seems more intuitive as it shows that it is the oracle function f(x) that flips the phase of |x\rangle if x satisfies the search condition. To implement the oracle on a circuit, we can \cf0 achieve this with the ancilla by applying a multi-CNOT gate (i.e. multi-controlled Toffoli gate) where the ancilla qubit is the target qubit and the rest of the qubits are the controlled qubits. In the code snippet in Figure 1, this multi-CNOT gate is implemented using **qc.mct(qr, anc[0], None, mode='noancilla')**. Mathematically, the state after applying the oracle is as such in Equation 3.
\[
|\psi\rangle=O\frac{1}{\sqrt{2^n}}\sum{x=0}^{2^n-1}|x\rangle=\frac{1}{\sqrt{2^n}}\sum{x=0}^{2^n-1}(-1)^{f(x)}|x\rangle \tag{3}
\]
After applying the oracle, we carry out three steps that amount to the diffusion operator. First, we apply the Hadamard transform again to all qubits
\[
|\psi\rangle=\frac{1}{\sqrt{2^n}}\sum{x=0}{2^n-1}(-1)^{f(x)}H^{\otimes n}|x\rangle=\frac{1}{2^n}\sum{y=0}{2^n-1}\left(\sum{y=0}{2^n-1}(-1)^{f(x)+x\cdoty}\right)|y\rangle. \tag{4}
\]
The Hadamard gates are applied to transform the uniform superposition basis back into \cf0 the computational basis. Subsequently, we apply the phase shift defined in Equation (5), which performs a conditional phase shift on the computer with every computational basis state except |0\rangle for receiving a phase shift -1.
\[
P: |x\rangle=-(-1)^{\delta*{x,0}}|x\rangle \tag{5}
\]
$$\delta*{x,0}$$ is the Kronecker delta function, which is defined as
\[
\delta\_{x,0}=
\begin{cases}
1 & \text{if } x=0 \\
0 & \text{if} x \neq 0
\end{cases}
\]
where x is the computational basis state (e.g. |x\rangle). This conditional phase shift will apply a phase shift of -1 to every computational basis state except |0\rangle.

Thus, if we apply P to |\psi\rangle, we get Equation (6).
\[
|\psi\rangle=\frac{1}{2^n}\left(\sum{x=0}{2^n-1} (-1)^{f(x)}\right)|0\rangle+\sum{y=0}{2^n-1}\left(\sum{x=0}{2^n-1}(-1)^{f(x)}(-1)^{x\cdoty}\right)|y\rangle \tag{6}
\]
Lastly, we apply the Hadamard transform again, ending in a final state |\psi\rangle shown in Equation (7).
\[
|\psi\_{final}\rangle=H^{\otimesn}\left(\frac{1}{2^n}\left(\sum{x=0}{2^n-1} (-1)^{f(x)}\right)|0\rangle+\sum{y=0}{2^n-1}\left(\sum{x=0}{2^n-1}(-1)^{f(x)}(-1)^{x\cdoty}\right)|y\rangle\right) \tag{7}
\]

## 2.3 Low-level (operator notation) overview of Grover's algorithm

Now, as you can see, it gets extremely mathy as we go through the Grover's Operator since the oracle and the diffusion operator involve multiple operators. However, we can simplify the math representation using operator notation, which will lead us to how we can conceptually understand Grover's Operator and, by extension, Grover's search algorithm. First, we can represent Equation (5) in a slightly different way by replacing the exponential form with a simpler linear expression as shown in Equation 8.1.
\[
P: |x\rangle=\left(2\delta\_{x,0}-1\right)|x\rangle \tag{8.1}
\]
Then, Equation (8.1) can be expressed as operator form using a projection operator |0\rangle\langle0| onto the state |0\rangle as shown in Equation (8.2).
\[
P: |x\rangle=(2|0\rangle\langle0|-I)|x\rangle \tag{8.2}
\]
2|0\rangle\langle0|x\rangle is defined as
\[
|0\rangle\langle0|x\rangle=
begin{cases}
|0\rangle & \text{if } x=0 \\
0 & \text\{if } x \neq 0
\end{cases}
\]
Subsequently, |x\rangle gets a phase flip (multiplied by -1). This shows that Equation (8.1) and (8.2) have the same effect.

The Hadamard transform that is applied on Equation (8.2) results in Equation (8.3) by conjugating it with $$H^\{\otimesn}$$. Two H operators are required as shown below because the Hadamard transform is applied before and after the operator 2|0\rangle\langle0|-I to change its reference basis also known as "conjugating the operator".
\[
H^{\otimesn}(2|0\rangle\langle0|-I)H^{\otimesn}=2|\psi\rangle\langle\psi|-I\equivD \tag{8.3}
\]
Hence, it turns out that everything that comes after the oracle (i.e. diffusion operator) can be represented as Equation (8.3). Therefore, we can represent the entire Grover's Operator G as shown in Equation (9), including the oracle operator.
\[
G=(2|\psi\rangle\langle\psi|-I)O \tag{9}
\]
Equation (9) enables us to acknowledge how Grover's Operator, which is the algorithm's core, is much simpler than it seems in Equation (7). Moreover, it helps us to visualize what G is doing to the initial state to find the target state.

To show this, I am going to define the target state in terms of |\alpha\rangle and |\beta\rangle, which are basis vectors that represent non-solutions and solutions respectively. Hence, we can represent |\psi\rangle=a|\alpha\rangle+b|\beta\rangle where **a** and **b** are arbitrary constants since |\psi\rangle is a combination of an arbitrary proportion of solutions and non-solutions. To be more specific, I will define |\alpha\rangle\equiv\frac{1}{\sqrt{N-M}}\sum{x}{}|x\rangle and |\beta\rangle\equiv\frac{1}{\sqrt{M}}\sum{x}{}|x\rangle where N is the total number of elements and M is the total number of solutions. Having defined |\psi\rangle, we can also graph this vector on |\alpha\rangle and |\beta\rangle axes as shown in Figure 2 below.

<p align="center">
    <img src="https://imgur.com/G4k7KSz" width="75%"/>
    Figure 2
</p>

Figure 2 also shows us the effect of the oracle and the diffusion operator, essentially visualizing Grover's operator. The oracle performs a reflection about the vector |\alpha\rangle, which portrays the oracle marking the target state by applying a phase shift of -1, also shown in Equation (10) and Figure 2.
\[
O(a|\alpha\rangle+b|\beta\rangle)=a|\alpha\rangle-b|\beta\rangle \tag{10}
\]
Then, the diffusion operator 2|\psi\rangle\langle\psi|-I also performs a reflection but on the vector |\psi\rangle. Hence, Grover's Operator G is the product of those two reflections and is effectively a rotation towards the basis vector |\beta\rangle, representing the solution(s)/target state(s)! If we apply G multiple times, |\psi\rangle would rotate closer towards |\beta\rangle, which we refer to as Grover's Iterations G^k|\psi\rangle as mentioned earlier. G^k\psi\rangle remains in the space spanned by |\alpha\rangle and |\beta\rangle for all $$k\in\mathbb{N}$$. The optimal number of iterations that leads to the target state exists, so the more we iterate doesn't necessarily mean the more likely we are going to get to the target state. The so-called Grover's Iterations resemble the process of amplitude amplification as each iteration increases the chances of reaching the target state by rotating closer to |\beta\rangle. An alternative way to represent |\psi\rangle is also shown in Equation (11), where $$\theta$$ is the rotation angle.
\[
|\psi\rangle=\cos(\frac{\theta}{2})|\alpha\rangle+\sin(\frac{\theta}{2})|\beta\rangle
\]
\[
G|\psi\rangle=\cos(\frac{3\theta}{2})|\alpha\rangle+\sin(3\frac{\theta}{2})|\beta\rangle
\]
\[
G^k|\psi\rangle=\cos(\frac{(2k+1)\theta}{2})|\alpha\rangle+\sin(\frac{(2k+1)\theta}{2})|\beta\rangle \tag{11}
\]
In a nutshell, Grover's algorithm is an operator that finds the target state by a series of discrete rotations (made possible by the two successive reflections on |\beta\rangle and |\psi\rangle) towards |\beta\rangle, thereby increasing the amplitude of the target state.

# 3. Hamiltonian

## 3.1 Fenner\'92s Hamiltonian

**_Having said this, how can we parallel Grover's algorithm to a Hamiltonian?_** In his article called "An Intuitive Hamiltonian for Quantum Search", Stephen Fenner describes a Hamiltonian for Grover's algorithm based on Farhi & Gutmann's Hamiltonian (Fenner, 2000). I didn't find Fenner's paper "intuitive", so I went through the mathematical steps to intuitively compare how similar this Hamiltonian was to the Grover's algorithm.

First, we derived the Hamiltonian from Fenner's Hamiltonian as shown in Equation (12), where |w\rangle is the target state ranging from 0 to N (search space) and |\sigma\rangle is an arbitrary unit vector (initial/start state) in the Hilbert space. We also assume x=\langlew|\sigma\rangle\in\mathbb{C} where x=\langlew|\sigma\rangle and x^{_}=\langle\sigma|w\rangle. Fenner's Hamiltonian is then
\[
H=2iE\left(x\cdot|w\rangle\langle\sigma|-x^{_}\cdot|\sigma\rangle\langlew|\right) \tag{12}
\]
If we let |w\rangle=A|\sigma\rangle+B|\sigma*\perp\rangle and \langlew|=A^{\*}\langle\sigma*\perp|+B^{\*}\langle\sigma*\perp| (and \langle\sigma|\sigma*\perp\rangle=0), we can evaluate the Hamiltonian further to give Equation (13).
\[
H=E[(0)I+I(A^{\*}B-AB^{\*})X+(A^{\*}B+AB^{\*})Y+(0)Z] \tag{13}
\]
This looks complicated, but we can think about it in a more intuitive way by mapping it to a spin in a magnetic field. To do so, let's define another Hamiltonian H constructed using an identity operator I, a scalar factor E, and a Pauli matrix \vec{\sigma} scaled by a parameter \vec{r} vector (the axis of rotation in which the initial state rotates about to progress to the target state) shown in Equation (14), representing some sort of spin-system to resemble the rotational nature of Grover's algorithm. This allows us to define the evolution of the initial state |\psi(t)\rangle over time using the time evolution operator, as shown in Equation (15), where \hat{n} is the axis of rotation where the initial state is continuously rotated to the final state..
\[
H=E(I+\vec{r}\cdot\vector{\sigma}) \tag{14}
\]
\[
|\psi(t)\rangle=e^{-itE|\vec{r}|\hat{n}\cdot\vec{\sigma}}|\sigma\rangle \tag{15}
\]

# 4. Grover's algorithm vs Hamiltonian

So, how is this Hamiltonian similar to Grover's search? H resembles Grover's search in two main ways.

First, both methods involve the initial state evolving to the target state through rotation. Grover's algorithm rotates the initial state to the target state through a set of **_discrete_** rotations referred to as Grover's iterations, while the Hamiltonian rotates the initial state about the axis of rotation \vec{r}, in which its direction depends on |\sigma\rangle and |w\rangle.

Second, the time variable in the time evolution operator plays a role similar to the number of iterations in Grover's algorithm. The initial state as a function of time is represented in Equation (15), but it can also be represented as |\psi(t)\rangle=\alpha(t)|w\rangle+\beta(t)|\sigma*\perp\rangle where \alpha(t)=\langlew|\psi(t)\rangle and \beta(t)=\langle\sigma*\perp|\psi(t)\rangle are periodic functions of t. There is a time t^{\*} where |\alpha(t)|^2 is maximized, analogous to the optimal number of iterations in Grover's search. The period of these oscillations will be proportional to \frac{1}{\sqrt{N}} where N is the size of the search space. If we were to find that t^{\*}, we would have to find the time t when |\alpha(t)=\langlew|\psi(t)\rangle|^2=1. After evaluating this equation, we get Equation (16) below which can be graphed to visualize (Fig. 3) where \theta represents E|\vec{r}|t.
\[
\sin^2(\theta)+2|A||B|\sin(\theta)\cos(\theta)+|A|^2\cos(2\theta)=1 \tag{16}
\]

<p align="center">
    <img src="https://imgur.com/frUbqd6" width="75%"/>
    Figure 3
</p>

Hence, depending on $$|A|$$, we can see the t^{\*} values changing. Regardless, the point is that there exists a t^{\*} value that maximizes $$|\alpha(t)|^2$$, thereby reaching the target state.

This is all to say Grover's algorithm operates in a similar way, where the "landscape" of probabilities (analogous to the amplitude of states) is adjusted with each iteration to amplify the correct solution. The reflections and rotations in Grover's algorithm are effectively "searching" the state space to maximize the probability of finding the target state, just as you are looking for the \theta that maximizes $$f(\theta)$$.

# 5. Final remarks

The connection between Grover's search and Hamiltonian dynamics is a bridge between quantum search and physical evolution. Understanding this relationship provides insights into quantum speedups in areas such as molecular dynamics, quantum machine learning, and even quantum error correction. As we continue to explore this deep connection, we may discover entirely new quantum algorithms that harness the natural evolution of quantum systems to solve computational problems more efficiently than before. Indeed, my next research direction leverages this Hamiltonian-Grover connection to develop a novel approach for solving the Traveling Salesman Problem, potentially offering a new quantum speedup for this fundamental optimization challenge.

[1] Bartschi, A., & Eidenbenz, S. (2020). Grover Mixers for QAOA: Shifting Complexity from Mixer Design to State Preparation. ArXiv (Cornell University).

[2]Fenner, S. A. (2000). An intuitive Hamiltonian for quantum search. ArXiv (Cornell University).
