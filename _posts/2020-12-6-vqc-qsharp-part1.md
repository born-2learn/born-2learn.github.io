---
title: 'Variational Quantum Classifier'
date: 2020-12-6
permalink: /posts/2020/12/variational-quantum-classifier/
tags:
  - Quantum Computing
  - Quantum Algorithms
  - Programming
  - Quantum Machine Learning
  - Microsoft QSharp
---
VQCs are Hybrid Quantum Classical Machine Learning Architectures meant for Classification tasks using Quantum Computers.  

# Variational Quantum Classifier

> This blog post is written as part of the [**Q# Advent Calendar – December 2020**](https://devblogs.microsoft.com/qsharp/q-advent-calendar-2020/).  
> I’d like to thank **Mariia Mykhailova** and the [**Microsoft Quantum**](https://www.microsoft.com/en-us/quantum) Team for giving me the opportunity to write this blog.


![introduction](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/vqc-part1/title-image.jpeg)

## Introduction ##   

**Machine Learning** (ML) is arguably the most important field of Artificial Intelligence today. It refers to the process of building algorithms that can learn from existing observations (or data sets), and leverage that learning to predict new observations, or determine the output of new input.

**Quantum Machine Learning** is also an evolving field that is gaining a lot of traction.  Quantum machine learning is the integration of *quantum algorithms* within *machine learning programs*.

There are multiple algorithms for classification in Classical machine learning that include Logistic Regression, Decision Tree Learning, K-Nearest Neighbours, Support Vector Machines and Neural Network based classifiers. All of them take in classical data and try to reduce a cost function by optimizing the parameters of the classical model. 

![classical machine learning workflow](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/vqc-part1/classical-ml-workflow.png)

*[Classical Machine Learning workflow]* [[1]](#references).  


There are multiple methods for classifying a dataset using a quantum computer, but we are going to explore an algorithm known as **VQC (Variational Quantum Classifier)**.

Like classical machine learning the VQC algorithm has a training stage (where datapoints with labels are provided and learning takes place) and a testing stage (where new datapoints without labels are provided which are then classified). Each of these stages in VQC are a four-step process:

1. Feature Map - Loading the Data into the Quantum System
2. Variational Circuit - The quantum classification circuit
3. Measurement and Assigning a Binary Label 
4. Classical Optimization Loop

![HQC workflow](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/vqc-part1/qml-workflow.png)  
*[HQC Machine Learning Algorithm Workflow]*.

----
## 1. Feature Map - Loading the Data into the Quantum System  

The idea of quantum feature maps comes from the *theory of kernels* in classical machine learning where a dataset is mapped non-linearly onto a higher dimensional space where a hyperplane can be found that classifies it.  

A quantum feature map $\phi(\vec{x})$ is a map from the classical feature vector $\vec{x}$ to the quantum state $\|\Phi(\vec{x})\rangle\langle\Phi(\vec{x})\|$ , a vector in Hilbert space. By applying the unitary operation on the initial state,  we have now blown up the dimension of our feature space and the task of our classifier is to find a separating hyperlane in this new space.  

The quantum advantage comes into picture when we use non-Classically simulable quantum feature maps over feature maps that can be simulated on classical computers. The quantum feature map of depth d is implemented by the unitary operator : 

$$ \mathcal{U}_{\Phi(\mathbf{x})}=\prod_d U_{\Phi(\mathbf{x})}H^{\otimes n},\ U_{\Phi(\mathbf{x})}=\exp\left(i\sum_{S\subseteq[n]}\phi_S(\mathbf{x})\prod_{k\in S} P_k\right) $$ 

which contains layers of Hadamard gates interleaved with entangling blocks encoding the classical data as shown in circuit diagram below for d = 2.

There are different types of feature maps available – `ZfeatureMap`, `ZZFeatureMap`, `PauliFeatureMap`, etc. that allow effective embedding of classical data into the quantum system.

![z feature map](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/vqc-part1/zfeaturemap.png)  
*`ZFeatureMap` as defined in* [[2](#references)].  

![image map – zz](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/vqc-part1/zzfeaturemap.png)  
*`ZZFeatureMap` as defined in* [[2](#references)].

---
## 2. Variational Circuit - The quantum classification circuit

This is a very crucial step for classification using quantum ciruits. We add a short depth variational circuit to the feature maps that have been defined above. The parameters of this variational circuit are then trained in the classical optimization loop(mentioned [here](#4-classical-optimization-loop)) until it classifies the datapoints correctly. This is the learning stage of the algorithm and accuracy of the model can vary based on the variational circuit one chooses.  

It is essential to choose a variational circuit of *shorter depth* (making it especially viable for implementations on real hardware), *lesser number of parameters* to train (faster training process) while making sure its Expressability and Entangling capacity are enough to classify our dataset to the degree we want. Building variational circuits with such conflicting properties, similar to the study on quantum feature maps, is an active field of research.

There are multiple types of Variational Circuits available that can also be customized. Here is an example of a parameterized variational circuit with interlinking parameterized Ry gates and entangling CNOT units:  

![Variational Quantum Circuit](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/vqc-part1/realamplitudes.png)  
*[A variational quantum circuit.]*

---
## 3. Measurement and Assigning a Binary Label

An n bit classification string is obtained once we measure the variational quantum circuit using the Measurement operator.  

The n bit string is now assigned values of the classification classes. This is done with the help of a boolean function $f: \{0, 1\}^{n} -> \{0, 1\}$. Common examples of boolean functions are:  
- **Parity function** : modulus 2 sum of all the digits of the n bit classical string.
- **Choosing the "k"th digit** : Choose the digit number "k" in the n bit classical string as the output, etc.  


---
## 4. Classical optimization loop.  

The parameters of the quantum variational circuit are updated using a classical optimization routine once the measurements are ready. This is the classical loop that trains our parameters until the cost function's value decreases.   
 

![Loss Landscape](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/vqc-part1/loss_landscape.png)  
*[Loss Landscape of a model.]*  

Commonly used optimization methods are as follows:  
- **COBYLA** - Constrained Optimization By Linear Approximation optimizer.
- **SPSA** - Simultaneous Perturbation Stochastic Approximation (SPSA) optimizer.
- **SLSQP** - Sequential Least SQuares Programming optimizer, etc.
---

## Microsoft Q# and QDK

![Azure Quantum](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/vqc-part1/azure-quantum.jpg)  
*Microsoft [Azure Quantum](https://azure.microsoft.com/en-in/services/quantum/)*

- **Q#** is Microsoft’s open-source programming language for developing and running quantum algorithms. It’s part of the **Quantum Development Kit (QDK)**, which includes Q# libraries, quantum simulators, extensions for other programming environments, and API documentation. In addition to the Standard Q# library, the QDK includes Chemistry, Machine Learning, and Numeric libraries.

- As a programming language, Q# draws familiar elements from Python, C#, and F# and supports a basic procedural model for writing programs with loops, if/then statements, and common data types. It also introduces new quantum-specific data structures and operations.
- You can get started with Q# at [Microsoft Docs - QDK and Q#](https://docs.microsoft.com/en-us/quantum/quickstarts/get-started)  

## Quantum Machine Learning with Microsoft   

You can try out a Quantum Classifier on your local machine by following this [Quantum Kata](https://github.com/microsoft/QuantumKatas/tree/main/tutorials/QuantumClassification). The [Quantum Katas](https://github.com/microsoft/QuantumKatas) are a collection of self-paced tutorials and programming exercises to help you learn quantum computing and Q# programming.  

---
✨✨✨ **Congratulations!**  
You have just completed a short tutorial on how to build your own Hybrid Quantum-Classical Architecture for Machine Learning.   
> Stay tuned for a follow-up post on implementing a Variational Quantum Classifier with [Microsoft Q#](https://www.microsoft.com/en-us/quantum/development-kit).  
> If you have any queries or you have found any bugs, you can reach out to me on my LinkedIn, twitter or via email. 😊

---
## References

[1] :   https://www.researchgate.net/publication/338063278_Separation_of_Multi-mode_Surface_Waves_by_Supervised_Machine_Learning_Methods.  
[2] :  Vojtech Havlicek, Antonio D. Córcoles, Kristan Temme, Aram W. Harrow, Abhinav Kandala, Jerry M. Chow, Jay M. Gambetta, "Supervised learning with quantum enhanced feature spaces", [https://arxiv.org/abs/1804.11326](https://arxiv.org/abs/1804.11326).

