---
title: 'CDR: A friendly review of "Error mitigation with Clifford quantum-circuit data"'
date: 2020-12-04
permalink: /posts/2020/12/Clifford-Data-Regression/
tags:
  - Quantum Computing
  - Quantum Machine Learning
  - Quantum Algorithms
  - Research Paper Review
---

> **Review written by** : [Syed Farhan](https://www.linkedin.com/in/syedfarhanahmad/) and [Nilesh Goel](https://www.linkedin.com/in/nilesh-goel/)  

# A friendly review of "Error mitigation with Clifford quantum-circuit data"

**Title of the Research Paper** : Error mitigation with Clifford quantum-circuit data  
**Authors** : Piotr Czarnik, Andrew Arrasmith, Patrick J. Coles, Lukasz Cincio  
**arXiv** : https://arxiv.org/abs/2005.10189  


## Method Implemented

![Method implemented](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/cdr/method-implementation.png)  

1. Generate states with Clifford Gates which are close to the required state and are easily classically simulable.
   
2. Find the exact expectation value of the observable with classical simulator, and noisy value with the quantum computer.
   
3. Train the noisy and exact values using a linear model and use it to correct the expectation value.

## Generation of Training Data States

![circuit](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/cdr/circuit.png)  

1. Ansatz of Transverse Ising Model is created using QAOA,
which is decomposed into a variational circuit.

2. A quantum circuit with N non-Clifford states is created which
is close to the required variational circuit.

3. More training data circuits are created by changing a
non-Clifford gate to a Clifford Gate and a new Clifford Gate
with the original non-Clifford Gate.  

## Results

![results](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/cdr/results.png)  

- CDR method seems to have decreased the
error by an order.

- Error decreases with increase in N, which is
understandable as increase in N makes the
training circuits more similar to original ones.

- Error increases with depth(p) and number of
qubits(q) in circuit.

---
> Thank you for your time. If you have any questions or would like to report any errors in this post, feel free to reach out to the review authors.  
> Have a great day!

