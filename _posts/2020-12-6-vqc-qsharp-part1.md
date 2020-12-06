---
title: 'Variational Quantum Classifier'
date: 2020-12-6
permalink: /posts/2020/11/variational-quantum-classifier/
tags:
  - Quantum Computing
  - Quantum Algorithms
  - Programming
  - Quantum Machine Learning
  - Microsoft Q#
---

# Variational Quantum Classifier

> This blog post is written as part of the Q# Advent Calendar – December 2020.  
> I’d like to thank **Mariia Mykhailova** and the **Microsoft Quantum** Team for giving me this opportunity.


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

A quantum feature map ϕ(⃗x)ϕ(x→) is a map from the classical feature vector ⃗xx→ to the quantum state |Φ(⃗x)⟩⟨Φ(⃗x)||Φ(x→)⟩⟨Φ(x→)|, a vector in Hilbert space. This is faciliated by applying the unitary operation UΦ(⃗x)UΦ(x→) on the initial state |0⟩n|0⟩n where n is the number of qubits being used for encoding. By doing this process we have now blown up the dimension of our feature space and the task of our classifier is to find a separating hyperlane in this new space.  

Constructing feature maps based on quantum circuits that are hard to simulate classically is an important step towards obtaining a quantum advantage over classical approaches. The quantum feature map of depth dd is implemented by the unitary operator  

$$ \mathcal{U}_{\Phi(\mathbf{x})}=\prod_d U_{\Phi(\mathbf{x})}H^{\otimes n},\ U_{\Phi(\mathbf{x})}=\exp\left(i\sum_{S\subseteq[n]}\phi_S(\mathbf{x})\prod_{k\in S} P_k\right) $$ 

which contains layers of Hadamard gates interleaved with entangling blocks encoding the classical data as shown in circuit diagram below for d=2d=2.

There are different types of feature maps available – ZfeatureMap, ZZFeatureMap, PauliFeatureMap, etc. that allow effective data embedding into the quantum system.

![z feature map](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/vqc-part1/zfeaturemap.png)  
*ZFeatureMap as defined in* [[2](#references)].  

![image map – zz](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/vqc-part1/zzfeaturemap.png)  
*ZZFeatureMap as defined in* [[2](#references)].

---
## 2. Variational Circuit - The quantum classification circuit

In this step we append a short depth Variational Circuit to the previously constructed feature map. The parameters of this variational circuit are then trained in the classical optimization loop until it classifies the datapoints correctly. This is the learning stage of the algorithm and accuracy of the model can vary based on the variational circuit one chooses. It is essential to choose a variational circuit of shorter depth (making it especially viable for implementations on real hardware), lesser number of parameters to train (faster training process) while making sure its Expressability and Entangling capacity are enough to classify our dataset to the degree we want. Building variational circuits with such conflicting properties, similar to the study on quantum feature maps, is an active field of research.

There are multiple types of Variational Circuits available that can also be customized. Here is an example of a parameterized variational circuit:  

![Variational Quantum Circuit](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/vqc-part1/realamplitudes.png)  
*[A variational quantum circuit.]*

---
## 3. Measurement and Assigning a Binary Label

After creating and measuring the circuit we are left with an n bit classical string from which we must derive a binary output which will be our classification result. This is done with the help of a boolean function f:{0,1}n−>{0,1}f:{0,1}n−>{0,1}. The way this boolean function is written out may not be as significant as the concepts earlier as modifying the boolean function will change the values of the parameters learned to accommodate for the change. Common examples of boolean functions are:  
- **Parity function** : modulus 2 sum of all the digits of the n bit classical string.
- **Choosing the "k"th digit** : Choose the digit number "k" in the n bit classical string as the output, etc.
This functionality is already inbuilt in the VQC method in Qiskit Aqua and you won't have to make any changes to it.

---
## 4. Classical optimization loop.  

Once we get our predictions a classical optimization routine changes the values of our variational circuit and repeats the whole process again. This is the classical loop that trains our parameters until the cost function value decreases. The details of the working have been ommitted as they are immutable and are not relevant going forward. However, you can look at the code and the optimization step to understand exactly how this step takes place.

Though there's isn't an available option to change the cost function during the optimization of VQC, Aqua provides you with a plethora of Optimization methods you could use during the training stage of the process. Here are a few for your reference:  

![Loss Landscape](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/vqc-part1/loss_landscape.png)  
*[Loss Landscape of a model.]*  

Commonly used optimization methods are as follows:  
- **COBYLA** - Constrained Optimization By Linear Approximation optimizer.
- **SPSA** - Simultaneous Perturbation Stochastic Approximation (SPSA) optimizer.
- **SLSQP** - Sequential Least SQuares Programming optimizer, etc.


:sparkles: Congratulations!  
You have just completed a short tutorial on how to build your own Hybrid Quantum-Classical Architecture for Machine Learning.   
> Stay tuned for a follow-up post on implementing a Variational Quantum Classifier with Microsoft Q#. 

---
## References

[1] :   https://www.researchgate.net/publication/338063278_Separation_of_Multi-mode_Surface_Waves_by_Supervised_Machine_Learning_Methods.  
[2] :  Vojtech Havlicek, Antonio D. Córcoles, Kristan Temme, Aram W. Harrow, Abhinav Kandala, Jerry M. Chow, Jay M. Gambetta, "Supervised learning with quantum enhanced feature spaces", https://arxiv.org/abs/1804.11326.

