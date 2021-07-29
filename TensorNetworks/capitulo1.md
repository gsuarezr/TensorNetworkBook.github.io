Capítulo 1
============================




```{epigraph}
**"Being ignorant is not so much a shame, as being unwilling to learn."**<br />

 -- Benjamin Franklin

```



## Introduction

  

The concept of tensor networks is a relatively young one that has its origin in the graph notation proposed by Penrose, which began to be applied in practical problems in 1971 {cite}`penrose_1971`. However, its stayed otside of the popular topics of conversation in science until Deutsch used a diagrammatic notation derived from it to formulate what he called "Quantum Computational Networks" that are now known as quantum circuits {cite}`D1989`. That's right! The circuits you build in [Qiskit](qiskit.org) are but a special case of tensor networks. A fun fact is that while the most popular model was made by Deutsch, Feynman had also formulated his own diagrammatic model related to the former just a few years earlier {cite}`Feynman1986`. But even after all this, tensor networks were rarely a topic of conversation. 

The interest resurfaced thanks to the use of the theory of tensor networks in numerical computations, with the rise of algorithms like MPS, TT, TTN, MERA and PEPS. Some of them have their origin in condensed matter and were meant to be used problems in quantum mechanics, somewhat inspired in the density matrix renormalization group. However, these numerical techniques also found use in machine learning, due to the fact that just like in condensed matter, the numerical problems are high-dimensional. This even drove companies like Google into investing in research in the area, as well as the development of libraries used for numerical computations of these techniques like [TensorNetwork](https://github.com/google/TensorNetwork).

A tensor network is a countable array of tensors connected by contractions (sums over indices) between them and not only has it helped in the theorical comprehension of wave functions in quantum mechanics, but they also form the basis for many powerful approaches to numerical simulation. So... ¿What is this book about? The following texts is mainly concerned in what is commonly called "tensor network methods" whiche is the set of tools associated with the computation and simplification of tensor networks. These methods, on top of being used in the aforementioned areas of physics of condensed matter {cite}`1308.3318` and machine learning {cite}`NIPS2015_6855456e`, they are also present in quantum information theory {cite}`PhysRevLett.125.190402`, open quantum systems {cite}`PhysRevLett.121.227401`, quantum gravity {cite}`Chirco_2018` and many other areas of science.

In this chapter, we will introduce some of the basic concepts that are necessary to understand what is a tensor network and how the techniques used in this theory can be implemented in numerical computations. We begin with the most basic thing we need to understand, what is a tensor.



## ¿Que es un Tensor?



A tensor is a multidimensional array of complex numbers; This mathematical concept generalizes the idea of multilinear maps, i.e. functions of many parameters that are linear with respect to each other. Now, if this somewhat formal definition doesn't give you a clear idea of what tensors are, let's try with a more intuitive explanation.

A tensor is a series of numbers denoted with $N$ indices, where $N$ is what we call the "order" of the tensor. We've all dealt with tensors in our lives, for example:

* $5$ is a tensor of order $0$, it is a series of numbers for which we do not need indices to refer to an element. This is the same for any other scalar.

* The vector $\vec{T}=\begin{pmatrix} 0  \\ 1 \end{pmatrix}$, is a tensor of order $1$, we need a single index to refer to one of its elements (i.e. $i=1,2$)

```{image} images/vector.png
:alt: vector
:width: 200px
:align: center
```


* A matrix $\vec{T}=\begin{pmatrix} 0 & 0 \\1 & 1 \end{pmatrix}$ is a tensor of range $2$, we need two indices to identify unequivocally every single one of its elements


```{image} images/matriz.png
:alt: matrix
:width: 200px
:align: center
```


A tensor of order $3$ is a mathematical object that most people are not very familiar with, but let's imagine a set of numbers ordered in a Rubik's cube. To identify each of the numbers, we would need $3$ indices.


```{image} images/tensor.png
:alt: Tensor of order 3
:width: 200px
:align: center
```


|      Name       |             Example             | Order |              Notation             |
|:---------------:|:-------------------------------:|:-----:|:---------------------------------:|
|     Escalar     |             Constant            |   0   |             $\lambda$             |
|      Vector     |          Wave function          |   1   |             $\psi_{i}$            |
|      Matrix     |             Operator            |   2   |             $A_{i.j}$             |
|       : :       |               : :               |  : :  |                : :                |
|  N-order Tensor |       N-body Wave function      |   N   | $\psi_{\alpha_{1},...\alpha_{N}}$ |










The way that tensors are commonly used in physics and mathematics, is through what is called the index notation and the Einstein sum convention, which we will introduce in the next section.

<<<<<<< HEAD
### Graphic representation of tensor networks  
=======
## Graphic representation of tensor networks  
>>>>>>> ab3240fc3d5a0e570b63d074e8d67108b12d434e

We use a diagram notation for tensors, where every tensor is drawn as a solid figure with connections according to its order. Here are some examples:

**The Vector** $A_{i}$ is represented as:

```{image} images/vector_tensor.png
:alt: A vector in its graphic representation
:width: 140px
:align: center
```

**The matrix** $B_{ij}$ is represented as:

```{image} images/matrix_tensor.png
:alt: A matrix in its graphic representation
:width: 140px
:align: center
```

**A tensor of order 3** $C_{ijk}$ is represented as:

```{image} images/tensor_orden_3.png
:alt: A tensor of order 3 in its graphic representation
:width: 140px
:align: center
```

We can form networks of tensors where an index shared by two different tensors means a sum or contraction over that index.

```{image} images/contraccion.png
:alt: Graphic representation of a contraction between two tensors
:width: 270px
:align: center
```

The multiplication of two matrices $A$ and $B$ can be writen in sum notation as:
$C_{ik} = \sum_{j}A_{ij}B_{jk} $. The equivalent to this in tensor diagrams is;

```{image} images/ejemplo1.png
:alt: Graphic representation between two tensors, denoting a matrix multiplication 
:width: 270px
:align: center
```

Similarly, we can think of how to represent other common operations with matrices, like the inner product. If we now consider vectors $A$ y $B$, their inner product $\sum_{i}A_{i}\cdot B_{i}$ is going to have an equivalent tensor diagram in the form of:

```{image} images/prod_interno1.png
:alt: Graphic representation of an inner product with vectors.
:width: 270px
:align: center
```

With two tensors $D$ and $E$ of order 4, their inner product $\sum_{ijkl}D_{ijkl}\cdot E_{ijkl}$ is equivalent to the diagram:

```{image} images/prod_interno2.png
:alt: Graphic representation of an inner prodcut with order 4 tensors.
:width: 270px
:align: center
```
Finally, we can also represent the trace of an array. Considering the matrix $A$, its trace $\sum_{i}A_{ii}$ has an equivalent diagram:

```{image} images/traza.png
:alt: Graphic representation of the trace of a matrix
:width: 270px
:align: center
```
We can now see the power of using graphic notation for tensors in the contraction of three tensors. A sum notation would be expressed as:
$D_{ijk} = \sum_{lmn}A_{ljm}B_{iln}C_{nmk} $. Instead, graphic notation is much easier to interpret:

```{image} images/ejemplo2.png
:alt: Graphic representation of a contraction between three tensors
:width: 270px
:align: center
```

In many applications, the goal is to approximate a high order tensor, like this one:

```{image} images/tensor_N.png
:alt: N-order tensor
:width: 270px
:align: center
```
To a tensor network of composed of several low order tensors:

```{image} images/tensor_N_2.png
:alt: Red tensorial de N tensores de bajo orden
:width: 270px
:align: center
```

The graphic representation of tensor networks is useful for many reasons:

1. It can offer a more compressed representation of structures with lots of data.
2. It makes characterization of the data structure easier, as well as bringing a more visual and intuitive notation.
3. Due to the data's robust decomposition, they can be used to work with noise or missing data.
4. It allows a unified framework to handle big data. Such as evaluating statistical information using a small set of tensor networks without the need to have specific knowledge about the structure of the whole system or whatever it may represent.

**Tensor network theory** is focused on trying to understand how this representation works and in which situations it is optimal to use it. On the other hand, **tensor network algorithms** are focused on finding methods to obtain, manipulate, and extract information from these representations. 

## Tensor networks for many body quantum  systems

### MPS Tensor Networks

Also known as Tensor Train (TT), **Matrix Product State** tensor networks are a way to represent a tensor of order $N$ as a chain of tensors of order $3$. This representation is the objective of many algorithms due to it providing a good approximation of the tensor networks and their higher computational efficiency.

```{image} images/MPS.png
:alt: Execution of MPS as a "cut" of the target tensor into smaller tensors.
:width: 470px
:align: center
```

Just by observing this image it may be difficult to imagine where such efficency comes from, but lets dive in to find out. Recall that the quantum state $|\psi\rangle=\sum_{ij\dots k}\psi_{ij\dots k}|u_{ij\dots k}\rangle$ requires $2^n$ independent coeficients to be correctly described, i.e. it grows exponentially with the size of n. With MPS we can describe such state as:

$|\psi\rangle = \sum_{ij\dots k}Tr(A_{i}^{[1]}A_{j}^{[2]}\dots A_{k}^{[n]})|u_{ijkl}\rangle$

In this way, the resources required to describe it only grow linearly with a scaling factor $nd\chi^{2}$ where $d$ is the dimension of the subsystem and $\chi$ times $\chi$ is the upper limit of the matrices. We can observe in this equation that obtaining the $\psi$ components is just a matter of calculating the product between the matrices. This is where the name of the method comes from.

The capabilities of MPS to lower the resources needed comes from a tool called **Singular Value Decomposition** that is applied recursively to the initial tensor $T$ to obtain the matrices that satisfy the condition $T=U\Sigma V$, where $U$ and $V$ are unitaries and $\Sigma$ is real, non-diagonal and non-negative.

If we go back to the example in the image, we can tell that the tensors in the extremes only have 2 connections instead of 3. This means that the product will result in a scalar, thus the trace in the equation is no longer required, leaving us with:
$|\psi\rangle = \sum_{ijkl}A_{i}^{[1]}A_{j}^{[2]}A_{k}^{[3]}A_{l}^{[4]}|u_{ij\dots k}\rangle$

```{bibliography}
```
<!--- 



## Notacion indicial y convenio de la suma de Einstein 


Esta introduccion a la notacion indicial y la suma de einstein sera un poco distinta de la usual ya que estaremos revisando tres formas de ver estas manipulaciones, una que llamaremos algebraica (usada en relatividad), la diagramatica que aun no tiene estandar pero que llamaremos de Penrose siguiendo [articulo de biamonte] y la de dirac (usualmente usada en cuantica). Esto nos dara una perspectiva mas amplia al poder apreciar las ventajas y desventajas de cada una de ellas, y resaltar lo intuitiva que es la notacion grafica.









