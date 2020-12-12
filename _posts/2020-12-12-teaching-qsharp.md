---
title: 'Teaching Quantum Computing with Microsoft Q# at Mini-Workshops'
date: 2020-12-12
permalink: /posts/2020/11/teach-qsharp-at-workshops/
tags:
  - Microsoft QSharp
  - Quantum Computing
---
# Teaching Quantum Computing with Microsoft Q# at Mini-Workshops

> This blog post is written as part of the [**Q# Advent Calendar – December 2020**](https://devblogs.microsoft.com/qsharp/q-advent-calendar-2020/). Check out the calendar for more great posts!

This summer, I had the opportunity to organize an international workshop on Quantum Computing with Microsoft Q# as a part of the [QPower Quantum Community](https://qpower-research.tech/). QPower is a Quantum Computing organization that was formed under the [Microsoft Student Ambassador Program](https://studentambassadors.microsoft.com/) with the guidance of Pablo Veramendi in 2019.  

![Workshop Poster updated](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/teaching-qsharp/workshop_poster.png)  

## Goal of the Workshop

Quantum Computing has been considered as a very new and challenging field by many engineering students in the several countries. Our aim was to introduce the concepts of Quantum Computing to students and professionals in an interactive manner. To achieve this, we had to carefully draft our curriculum so that it included an abstracted amalgamation of quantum mechanics and quantum hardware along with the programming and computing aspects.  

We also had to choose the implementation mechanism for the algorithms that we teach. We chose Microsoft Quantum Development Kit because of its rich integrations with Visual Studio, Visual Studio Code and Jupyter Notebooks. The best part of using the QDK was having a Quantum focused programming language, Q# and the Quantum projects had the interoperability with Python and .NET environments.

To ensure that the audience clearly understood the concepts of Quantum Computing and the language constructs of Q#, we planned on organizing a Kahoot quiz as well.

## The Workshop on "Introduction to Quantum Computing and Microsoft Q#"

The final workshop on Quantum Computing had the following sessions:
- **Basics of Quantum Computing** - In this section, we introduced the students to the paradigm of Quantum Computing and how it is different from that of Classical Computing. We also introduced students to the Dual Nature of Matter as it would help them in understanding qubits better. We concluded the session by talking about some qubit operations and a few applications of Quantum Computing in the real-world.  

- **Quantum Gates and Quantum Information** - This session took a deep dive into qubit operations and the different types of Quantum Gates. We introduced the students to Quantum oracles and some basic algorithms in quantum computing.
- **Introduction to QDK and Microsoft Q#** - A few days prior to the workshop, the attendees were optionally asked to install Microsoft QDK on their local systems by following this [Installation Guide](https://docs.microsoft.com/en-us/quantum/quickstarts/#install-the-qdk-locally) from Microsoft Docs. This session introduced the attendees to the special features of Microsoft QDK and the Q# programming language.

- **Programming with Q#** - The language constructs and syntax of the Q# language were explained in greater depths during this session. Data types, operations, control flow and qubit management were covered as given [here](https://github.com/QPower-Research/Workshops/blob/master/workshop%2001%20-%20Introduction%20to%20Quantum%20Computing/intro-to-qsharp.ipynb). To faciliate a hands-on sessions for those who hadn't installed QDK on their local machines, we setup a [binder](https://mybinder.org/) link with support for Q# kernel in jupyter notebooks, that converts any Git repo into a collection of interactive notebooks and can be executed live simultaneously by multiple attendees. 

The Quantum Katas came in very handy while explaining code snippets about several topics like [Basic Gates](https://github.com/microsoft/QuantumKatas/tree/main/BasicGates), [Linear Algebra](https://github.com/microsoft/QuantumKatas/tree/main/tutorials/LinearAlgebra) and even complete algorithms like the [Grover's Algorithm](https://github.com/microsoft/QuantumKatas/tree/main/GroversAlgorithm).  

![Binder screenshot](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/teaching-qsharp/binder.png)  
[*A screenshot from the online workshop - Hadamard gate with Microsoft Q#*]

## Quantum Computing Quiz

To ensure that the attendees had understood the content of the entire workshop, we organized a [Kahoot](https://kahoot.it/) quiz at the end of the session. This quiz consisted of 10 questions on the concepts of Quantum Computing, language constructs of Microsoft Q# and a few Quantum Algorithms.  

![Kahoot quiz](https://raw.githubusercontent.com/born-2learn/born-2learn.github.io/master/_posts/images/teaching-qsharp/kahoot.png)   
[*One of the 10 questions asked in the kahoot quiz during the workshop*]

## Outcome

The workshop had successfully introduced **500+** students to the concepts of Quantum Computing and to Microsoft Q# as a Quantum-focused programming language. The survey conducted at the end of the session showed great responses. Many students wanted a follow-up workshop for more complex Quantum Computing Algorithms and perhaps for Quantum Machine Learning with Q# as well.  Many students reverted back to us for more resurces that they could use in their next project. 

Overall, it was an awesome experience for the organizing team from [QPower Quantum Community](https://qpower-research.tech/). We learned several ways to conduct a workshop on a cutting-edge technology, Quantum Computing, and we hope to organize several more in the future! 

> If you have any queries or would like to know more about the Microsoft Q# workshop content, feel free to reach out to me on LinkedIn or Twitter.


