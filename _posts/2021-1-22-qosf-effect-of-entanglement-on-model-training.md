---
title: 'Effect of Entanglement on Model Training'
date: 2021-01-21
permalink: /posts/2021/01/effect-of-entanglement-on-model-training/
tags:
  - Quantum Machine Learning
  - Quantum Computing
---

> This blog post is written as part of the QOSF(Quanutm Open Source Foundation) Mentorship Program.  
>   
> **Student:** [Syed Farhan Ahmad](https://www.linkedin.com/in/syedfarhanahmad/)  
> **Mentor:** [Amira Abbas](https://www.linkedin.com/in/amira-abbas/) 

## Project Description

Classical neural networks encode higher dimensional vectors(inputs) to lower dimensional vectors(outputs), but the reverse is not possible. Recent research has shown that scrambling of information from a smaller subsystem to a larger one is feasible. In our QOSF project, we analyse the effect of entanglement as a variational circuit trains and we also study the role of various entropies to characterize entanglement.


## Entropy to quantify entanglement

Research has shown that entropy(von-Neumann entropy) can be used to quantify entanglement in a quantum circuit. We extended our work on this by studying various entropies and choosing the most relevant ones that impact entanglement in a circuit:  
- von-Neumann entropy
- Meyer-Wallach entropy

## Entanglement Metrics Used  

### **von-Neumann Entropy measure**  

One of the most studied and frequently used entropy functions is the von Neumann entropy, which is defined as follows:   

For a density operator $\rho\in\mathcal{D}(\mathcal{H})$
von-Neumann entropy is:  

$S(\rho)=-\mathrm{Tr}(\rho \log\rho)$
  
This entropy is a quantum generalization of the classical Shannon entropy. If $$\{p_i\}_i$$  are the eigenvalues of a density operator , then the von Neumann entropy equals the Shannon entropy of a random variable X($$\rho$$) with same probability distribution :  

![von neumann equation](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/qosf/vn_eq.png)

### **Meyer Wallach Entropy measure**
It is a single scalar measure of pure-state entanglement. The Meyer–Wallach (MW) measure written in the Brennen form is:

![meyer wallach](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/qosf/meyerwallach_equation.png)  
where $\rho_k$  is the one-qubit reduced density matrix of the kth qubit after tracing out the rest.

**Implementation**: Meyer Wallach entanglement measure is implemented in `libraries/meyer_wallach_measure.py`.

## Toy Model under study

The Quantum Circuit automatically learns the parameters to generate a Bell State. We have used a very simple parameterized circuit and have manually coded an SGD optimizer with MSE Cost function to analyse the effect of entanglement as the model trains and optimizes its parameter to return to the bell state.   


![bell state](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/qosf/bell_state.png)


## Variational Quantum Classifier Setup

### Specifications

![vqc circuit image](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/qosf/vqc_arch.png)
- **Quantum Embedding:** ZZFeatureMap
- **Quantum Variational Circuit:** RealAmplitudes
- **Optimizer:** ADAM
- **Cost Function:** Sigmoid estimation cost function
### Quantum Circuit

![vqc circuit](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/qosf/vqc_circuit.png)

  
### Datsets used
- Ad Hoc dataset from `qiskit.ml.datasets`.
- Synthetic data generated from `sklearn`'s `make_blobs()` with varying standard deviations in the data.

## Results & Observations

### Toy Model(Parameterized bell state with custom optimizer)  
- Let's begin by plotting the Loss function vs the number of epochs and expect a quadratic graph.  
![bell loss](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/qosf/bell_costvsepoch.png)
- The von-Neumann entropy increases as the circuit trains, reaches it's peak and then decreases again.  
![bell vn vs epoch](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/qosf/bell_vn_vs_epoch.png)  
- Observing from right to left, we see that the von-Neumann entropy increases as the Loss function decreses, which is expected from von-Neumann entropy's equation since we are approaching a pure state from a mixed state.  
![bell vn vs loss](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/qosf/bell_vn_vs_loss.png)   
> *Therefore, we can conclude that the von-entropy measure under consideration behaves as expected(after testing on the Toy Model) and we are now ready to test it on our actual VQC experiment.*  

### Variational Quantum Classifier

- Let's again begin by plotting the Loss function vs the number of epochs and expect a step-like plot(since the Sigmoid estimation loss was used).   
![vqc cost vs epoch](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/qosf/vqc_cost_vs_epoch.png)  
- von-Neumann entropy follows a pattern as the model trains and the peak values in the plot gradually decrease. Further analysis is required to understand the correlation.  
![vqc vn vs epoch](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/qosf/vqc_vn_vs_epoch.png)  
- Meyer Wallach measure decreases exponentially as the model trains for both Ad-Hoc and synthetic datasets.  
![vqc mw vs epoch](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/qosf/vqc_mw_vs_epoch.png)  
- This follws the same trend, i.e, von-Neumann entropy's peaks slighly decrese as the cost values decrease(from right to left).  
![vqc vn vs loss](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/qosf/vqc_vn_vs_loss.png)  
- We see that Meyer Wallach measure decreses as the cost function decreases(from right to left).  
![vqc mw vs loss](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/qosf/vqc_mw_vs_loss.png)  

## Code 

The code has been open sourced and can be accessed on [GitHub](https://github.com/born-2learn/Entanglement_in_QML).


## Conclusion
 - Based on the results obtained, we can safely conclude that Meyer Wallach entanglement measure decreases as the model trains and as the cost function's value decreases. 
 - We can also find a pattern in the von-Neumann entropy measure for every 10 epochs where the entropy value peaks at almost 10% and these peaks decrease as the cost function decreases.
 - We are looking for more insights from the controlled synthetic data that is being generated. Please check back soon for updates!

Congrats👏! You have finally completed reading the blog. Stay tuned for more updates on this project as this isn't the end!

## References
1. Expressibility and entangling capability of parameterized quantum circuits for hybrid quantum-classical algorithms, Sukin Sim, Peter D. Johnson, Alan Aspuru-Guzik [[Link]](https://arxiv.org/abs/1905.10876)
2. Information Scrambling in Quantum Neural Networks, Huitao Shen, Pengfei Zhang, Yi-Zhuang You, Hui Zhai [[Link]](https://arxiv.org/abs/1909.11887)
3. An observable measure of entanglement for pure states of multi-qubit systems, Gavin K. Brennen [[Link]](https://arxiv.org/abs/quant-ph/0305094)
4. A characterization of global entanglement, Peter J. Love, Alec Maassen van den Brink, A. Yu. Smirnov, M. H. S. Amin, M. Grajcar, E. Il'ichev, A. Izmalkov, A. M. Zagoskin [[Link]](https://arxiv.org/abs/quant-ph/0602143)

## Acknowledgements

I would like to thank my mentor **Amira Abbas** for her constant support and guidance without who this project wouldn't have taken shape. I would also like to thank the **QOSF** team for giving me this fantastic opportunity of being a part of an awesome mentorship program.  

> If you’re interested in following me on my journey, connect with me on [Linkedin](https://www.linkedin.com/in/syedfarhanahmad/) or follow me on [Twitter](https://twitter.com/syedfarhanrvce).   
> For any queries, feel free to contact me on Linkedin, on twitter or by email.