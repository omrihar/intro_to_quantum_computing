---
title: Introduction to Quantum Computing
subtitle: How I Learned to Stop Worrying and Love the Bomb
author: Dr. Omri Har-Shemesh
institute: 'Schmiede.ONE GmbH & Co. KG'
date: 13. February 2019
header-includes: |
   \usepackage{fontspec}
   \usepackage{amsmath}
   \usepackage{amssymb}
   \usepackage{braket}
   \usepackage{graphicx}
   \usepackage{epigraph}
   \usepackage{tikz}
   \usepackage{tikzpeople}
   \usepackage{makecell}
   \usetikzlibrary{matrix}
   \newfontfamily\myfont{VL Gothic}
   \providecommand{\shrug}{{\myfont ツ}}
   \newcommand{\zero}{\begin{pmatrix}1 \\ 0\end{pmatrix}}
   \newcommand{\one}{\begin{pmatrix}0 \\ 1\end{pmatrix}}
   \newcommand{\cnot}{\pmat{1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 1 \\ 
   0 & 0 & 1 & 0}}
   \newcommand{\zerozero}{\pmat{1 \\ 0 \\ 0 \\ 0}}
   \newcommand{\zeroone}{\pmat{0 \\ 1 \\ 0 \\ 0}}
   \newcommand{\onezero}{\pmat{0 \\ 0 \\ 1 \\ 0}}
   \newcommand{\oneone}{\pmat{0 \\ 0 \\ 0 \\ 1}}
   \newcommand{\phase}[2]{\begin{pmatrix}#1 \\ #2 \end{pmatrix}}
   \newcommand{\pmat}[1]{\begin{pmatrix}#1\end{pmatrix}}
   \newcommand{\sqrtt}{\frac{1}{\sqrt{2}}}
   \newcommand{\half}{\frac{1}{2}}
   \newcommand{\Had}{\pmat{\sqrtt & \sqrtt \\ \sqrtt & -\sqrtt}}
   \newcommand{\NotGate}{\pmat{0 & 1 \\ 1 & 0}}
   \setlength{\tabcolsep}{15pt}
   \newcommand{\Mod}[1]{\ (\mathrm{mod}\ #1)}
   \newcommand{\rawalert}[1]{{\usebeamercolor[fg]{alerted text} #1}}
themeoptions:
   - titleformat=regular
   - numbering=fraction
   - progressbar=frametitle
   - background=light
---

# Introduction

## _Ce n'est pas un lustre_ (This is not a Chandelier)

![IBM Q](images/ibm_q.jpg)

## Big big picture

::: incremental

- New computing paradigm

- Uses the rules of quantum mechanics

- \alert{Might} be able to solve \alert{some} problems exponentially faster

- \alert{Definitely} could simulate quantum systems better

- Realizable in large scale?

- $\text{¯\reflectbox{/}\string_(\text \shrug)\string_/¯}$

:::

## Disclaimer

::: incremental

- Quantum Mechanics is technical

- Quantum Mechanics is weird

- Quantum Computing is technical

- Quantum Algorithms are hard

:::

##

\begin{center}
\Huge Examples?
\end{center}

## Example: the RSA algorithm

\begin{center}
\only<1>{\Huge Breaking the RSA algorithm}
\end{center}
\begin{center}
\begin{tikzpicture}[ampersand replacement=\&, font=\small]
\uncover<2->{\node[alice,female,minimum size=1.5cm] (A) {Alice};}
\uncover<3->{\node[bob,right=3cm of A,minimum size=1.5cm,mirrored] (B) {Bob};}
\uncover<4-5>{\draw (A.east) edge[->]  node[above] {"Hello Bob"} (B.west);}
\uncover<5>{\node[devil, right of=A,minimum size=1.5cm, below=1cm of A] 
(C){Devil};}
\uncover<6-7>{\draw (B.270) ++ (0, -1.0) node {\makecell{Generate 
Public/Private keys\\$e$, $d$, $N=pq$}};}
\uncover<7>{\draw (A.east) edge[<-]  node[above] {"Public Key: $e$, $N$"} 
(B.west);}
\uncover<8->{\draw (A.270) ++(0, -1.0) node 
{\makecell{$m=\mathrm{encode}(\text{"Hello World"})$\\$c = m^e\Mod{N}$}};}
\uncover<9->{\draw (A.east) edge[->] node[above] {$c$} (B.west);}
\uncover<10->{\draw (B.270) ++(0, -1.0) node {\makecell{$m = 
c^d\Mod{N}$\\$\mathrm{decode}(m)$}} ;}
\end{tikzpicture}
\end{center}

## Example: the RSA algorithm - outline

It's \alert{easy} to find $e, d, N \in \mathbb{N}$, such that:

. . .

\begin{equation*}
\boxed{(m^e)^d \equiv m \Mod{N}}
\end{equation*}

. . .

::: incremental

- $N = pq$ with $p$, $q$ large prime numbers.

- $e$, $N$ - Public Key

- $d$, $N$ - Private Key

:::

. . .

But it's \alert{hard} to find $p$, $q$, such that $pq = N$.

## Example: the RSA algorithm - Factorizing is Hard

### Factorizing on a Classical Computer

\vspace{1em}

\begin{tabular}{clp{5cm}}
\alert{Bits} & \alert{Time} & {\textcolor{mLightBrown}{Notes}}\\ \hline
128 & less than 2 seconds \\
192 & 16 seconds \\
256 & 35 minutes \\
260 & 1 hour \\
512 & 73 days & in 2009\\
\uncover<2->{
\alert{768} & \alert{1500 CPU years} & {\textcolor{mLightBrown}{Largest known 
(2010), took 2 years on hundreds of computers}}}
\end{tabular}

\vspace{1em}
\uncover<3->{Most RSA implementations use between \alert{1024} and \alert{4096} 
bits.}

## Example: the RSA algorithm - Shor's Algorithm

![Peter W. Shor](images/peter_w_shor.jpg)

## Example: the RSA algorithm - Shor's Algorithm

\begin{center}
\includegraphics[height=0.8\textheight]{images/peter_w_shor.jpg}
\end{center}

Would require $\sim 4000$ qubits to break $2048$-bit RSA

# Movie

# The Technical Part

## 

\epigraph{``Shut up and calculate''}{David Mermin}

## Overview

- Representing computation with linear algebra

- Qubits, superposition and quantum logic gates

- Simplest problem where a quantum computer outperforms a classical one

- Bonus: Quantum entanglement and quantum teleportation

## Representing classical bits as vectors

One bit with value $0$, also written as $\ket{0}$ (Dirac vector notation)

\qquad $\zero$

\ 

\ 

One bit with value $1$, also written as $\ket{1}$

\qquad $\one$

## Review: matrix multiplication

:::::: {.columns}
::: {.column width="60%"}

\scalebox{0.8}{
$\pmat{a & b \\ c & d}\pmat{x\\y} = \pmat{ax + by \\ cx + dy}$}

\ 

\ 

\scalebox{0.8}{$\pmat{a & b & c\\ d & e & f \\ g & h & i} \pmat{x\\y\\z} = 
\pmat{ax + by + cz \\ dx + ey + fz \\ gx + hy + iz}$}

\ 

\ 


\scalebox{0.8}{$\pmat{a & b \\ c & d}\pmat{w & x \\ y & z} = \pmat{aw + by & ax 
+ bz \\ cw + dy & cx + dz}$}

:::

::: {.column width="40%"}

\scalebox{0.8}{$\pmat{1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 
& 0 & 1} \pmat{a \\ b \\ c \\ d} = \pmat{a \\ b \\ c \\ d}$}

\ 

\ 

\scalebox{0.8}{$\pmat{1 & 0 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 
& 0 & 1} \pmat{0 \\ 1 \\ 0 \\ 0} = \pmat{0 \\ 0 \\ 1 \\ 0}$}

:::
:::::: 

## Operations on one classical bit (cbit)

\only<1>{\huge What are the operations we can perform on one classical bit?}


\only<2->{
\begin{tabular}{l c c c}
Identity & $f(x) = x$ & \scalebox{0.6}{$\pmat{1 & 0 \\ 0 & 1}\pmat{1 \\ 
0}=\pmat{1 \\ 0}$} & \scalebox{0.6}{$\pmat{1 & 0 \\ 0 & 1}\pmat{0 \\ 1}= 
\pmat{0 \\ 1}$} \\ \\

Negation & $f(x) = \neg x$ & \scalebox{0.6}{$\pmat{0 & 1 \\ 1 & 0}\pmat{1 \\ 
0}=\pmat{0 \\ 1}$} & \scalebox{0.6}{$\pmat{0 & 1 \\ 1 & 0}\pmat{0 \\ 1}= 
\pmat{1 \\ 0}$} \\ \\

Constant-0 & $f(x) = 0$ & \scalebox{0.6}{$\pmat{1 & 1 \\ 0 & 0}\pmat{1 \\ 
0}=\pmat{1 \\ 0}$} & \scalebox{0.6}{$\pmat{1 & 1 \\ 0 & 0}\pmat{0 \\ 1}= 
\pmat{1 \\ 0}$} \\ \\

Constant-1 & $f(x) = 1$ & \scalebox{0.6}{$\pmat{0 & 0 \\ 1 & 1}\pmat{1 \\ 
0}=\pmat{0 \\ 1}$} & \scalebox{0.6}{$\pmat{0 & 0 \\ 1 & 1}\pmat{0 \\ 1}= 
\pmat{0 \\ 1}$} \\
\end{tabular}
}

## Reversible computing

- Given the operation and the input, you can always infer the output.
   - For $Ax = b$, given $b$ and $A$, you can uniquely find $x$.

. . .

- Permutations are reversible; erasing and overwriting are not
   - Identity and negation are reversible.
   - Constant-0 and Constant-1 are not reversible.

. . .

- Quantum computers use \alert{only reversible operations}.
   - In fact, all quantum operations are their own inverse.


## Review: tensor product of vectors

::::: {.columns}
::: {.column width="50%"}

\scalebox{0.7}{$\pmat{x_0 \\ x_1}\otimes\pmat{y_0\\y_1}= 
\pmat{x_0\pmat{y_0\\y_1} \\ x_1\pmat{y_0\\y_1}} = \pmat{x_0y_0\\x_0y_1 \\ 
x_1y_0\\x_1y_1}$}

\ 

\ 

\scalebox{0.7}{$\pmat{x_0\\x_1}\otimes \pmat{y_0\\y_1}\otimes \pmat{z_0\\z_1} = 
\pmat{x_0y_0z_0\\ x_0y_0z_1\\ x_0y_1z_0 \\ x_0y_1z_1 \\ x_1y_0z_0 \\ x_1y_0z_1 
\\ x_1y_1z_0 \\ x_1y_1z_1}$}
:::

::: {.column width="50%"}

\scalebox{0.8}{$\pmat{1\\2}\otimes\pmat{3\\4}= \pmat{3\\4\\6\\8}$}

\ 

\scalebox{0.8}{$\one \otimes \one \otimes \zero = 
\pmat{0\\0\\0\\0\\0\\0\\1\\0}$}
:::
:::::


## Representing multiple cbits

::::: {.columns}

::: {.column width="33%"}
\scalebox{0.7}{$\ket{00} = \zero\otimes\zero = \pmat{1\\0\\0\\0}$}

\ 

\scalebox{0.7}{$\ket{10} = \one\otimes\zero = \pmat{0\\0\\1\\0}$}
:::

::: {.column width="33%"}
\scalebox{0.7}{$\ket{01} = \zero\otimes\one = \pmat{0\\1\\0\\0}$}

\ 

\scalebox{0.7}{$\ket{11} = \one\otimes\one = \pmat{0\\0\\0\\1}$}
:::
::: {.column width="33%"}

\scalebox{0.6}{$\ket{100} = \one\otimes\zero\otimes\zero = 
\pmat{0\\0\\0\\0\\1\\0\\0\\0}$}

:::
:::::

- The tensor representation is called the \alert{product state}.

- It can be \alert{factored} back into the \alert{individual state} 
  representation.

- The product state of $n$ bits is a vector of size $2^n$.

## Operations on multiple cbits: CNOT 

- Takes two bits, one \alert{control} bit and one \alert{target} bit.

- If the control bit is set, flip the target bit, otherwise leave it.

. . .

- If most significant bit is control, and least-significant is target, then:

::::: {.columns}

::: {.column width="50%"}

\hspace{30pt}
\begin{tikzpicture}[ampersand replacement=\&]
  \matrix (m) [matrix of math nodes,row sep=.5em,column sep=4em,minimum 
width=2em]
  {
    \ket{00} \& \ket{00} \\
    \ket{01} \& \ket{01} \\
    \ket{10} \& \ket{10} \\
    \ket{11} \& \ket{11} \\
   };

   \uncover<3->{\draw[->] (m-1-1) -- (m-1-2);}
   \uncover<4->{\draw[->] (m-2-1) -- (m-2-2);}
   \uncover<5->{\draw[mLightBrown,->] (m-3-1) -- (m-4-2);}
   \uncover<6->{\draw[mLightBrown,->] (m-4-1) -- (m-3-2);}
\end{tikzpicture}

:::

::: {.column width="50%"}

\uncover<7->{
\vspace{10pt}
\scalebox{1.0}{$C = \cnot$}}
:::
:::::

## Operations on multiple cbits: CNOT

\scalebox{0.8}{$C\ket{10} = C\left(\one\otimes\zero \right) = \cnot\onezero = 
\oneone = \one\otimes\one = \ket{11}$}

\hspace{30pt}

\scalebox{0.8}{$C\ket{11} = C\left(\one\otimes\one \right) = \cnot\oneone = 
\onezero = \one\otimes\zero = \ket{10}$}

## Operations on multiple cbits: CNOT

\scalebox{0.8}{$C\ket{00} = C\left(\zero\otimes\zero \right) = \cnot\zerozero = 
\zerozero = \zero\otimes\zero = \ket{00}$}

\hspace{30pt}

\scalebox{0.8}{$C\ket{01} = C\left(\zero\otimes\one \right) = \cnot\zeroone = 
\zeroone = \zero\otimes\one = \ket{01}$}


## Qubits and superposition

- Cbits are a special case of Qubits!

- A qubit is represented by \scalebox{0.5}{$\pmat{a \\ b}$} where $a$ and $b$ 
  are \alert{complex numbers} such that $||a||^2 + ||b||^2=1$.

   - The cbit vectors \scalebox{0.5}{$\zero$} and \scalebox{0.5}{$\one$} fit 
     this definition. 

- Example qubit values:

\begin{equation*}
\pmat{\frac{1}{\sqrt{2}}\\{\frac{1}{\sqrt{2}}}} \qquad 
\pmat{\frac{1}{2}\\{\frac{\sqrt{3}}{2}}} \qquad \pmat{-1\\0} \qquad 
\pmat{\frac{1}{\sqrt{2}}\\{\frac{-1}{\sqrt{2}}}}
\end{equation*}

## Qubits and superposition 

### What does that mean?

. . .

\metroset{block=fill}
\begin{block}{Superposition}
The qubit is in a state of both $\ket{0}$ and $\ket{1}$. We can write this as:

\scalebox{0.8}{$\pmat{a\\b} = a\zero + b\one = a\ket{0} + b\ket{1}.$}

\only<3>{
\begin{center}
\usetikzlibrary{arrows.meta}
\begin{tikzpicture}
    \draw [<->] (0,2) -- (0, 0) -- (2, 0);
    \node at (2, -0.3) {$\ket{0}$};
    \node at (-0.3, 2) {$\ket{1}$};
    \draw [-Triangle,very thick] (0, 0) -- (45:1);
    \draw [-Triangle,very thick] (0, 0) -- (0, 1);
    \draw [-Triangle,very thick] (0, 0) -- (1, 0);
    \node[anchor=south west] at (70:0.8) {\scalebox{0.7}{ 
        $\ket{\psi}=\frac{1}{\sqrt{2}}(\ket{0}+\ket{1})$}};
\end{tikzpicture}
\end{center}
}

\end{block}

. . .

. . .

\begin{block}{Amplitudes}
$a$ and $b$ are called amplitudes. $||a||^2$ is the probability of
the qubit being $0$ when \alert{measured}; $||b||^2$ is the probability of
measuring $1$.
\end{block}

. . . 

\begin{block}{Measurement}
The measurement of the qubit \alert{collapses} its state. It will be in the
state $\ket{0}$ if we measured $0$ and $\ket{1}$ if we measured $1^\dagger$.
\end{block}


## Qubits and superposition

\metroset{block=fill}
\begin{exampleblock}{For example}
The qubit \scalebox{0.7}{$\pmat{\sqrtt \\ \sqrtt}$} has a 
$\left|\left|\sqrtt\right|\right|^2 = \frac{1}{2}$ chance of collapsing to 
$\ket{0}$ or $\ket{1}$.

The qubit \scalebox{0.7}{$\zero$} has $100\%$ chance of collapsing to 
$\ket{0}$, and \scalebox{0.7}{$\one$} has a $100\%$ chance of collapsing to 
$\ket{1}$.

\end{exampleblock}


## Qubits and Superposition

- Multiple qubits are represented by the tensor product: 
  \scalebox{0.7}{$\phase{a}{b}\otimes\phase{c}{d}=\pmat{ab\\ad\\bc\\bd}$}
  with $||ac||^2 + ||ad||^2+||bc||^2+||bd||^2=1$.

. . .

- For example:
  \begin{equation*}
  \phase{\sqrtt}{\sqrtt} \otimes \phase{\sqrtt}{\sqrtt} = 
\pmat{\frac{1}{2}\\\frac{1}{2}\\\frac{1}{2}\\\frac{1}{2}} ; \quad  
\left|\left|\frac{1}{2}\right|\right|^2 = \frac{1}{4}; \quad 
\frac{1}{4}+\frac{1}{4}+\frac{1}{4}+\frac{1}{4} = 1
  \end{equation*}

. . .

  $\rightarrow$ There's an equal chance ($25\%$) of measuring $\ket{00}$, 
  $\ket{01}$, $\ket{10}$, $\ket{11}$.

## Operations on qubits

- We operate on qubits in the same way as on cbits: with matrices.

- All the operations we saw so far (bit flip, CNOT, etc...) work on qubits as 
  well.

- In reality, the matrix operations model some device that manipulates the real 
  qubits \alert{without measurement}. 

- Some gates only make sense in the quantum context...

## The Hadamard gate

- The Hadaramd gate puts a $\ket{0}$ or $\ket{1}$ bit into exact
  superposition: $H\ket{0} = \sqrtt\left(\ket{0}+\ket{1}\right)$ and $H\ket{1} 
  = \sqrtt\left(\ket{0}-\ket{1}\right)$.

. . .

\begin{equation*}
H = \pmat{\sqrtt & \sqrtt \\ \sqrtt & -\sqrtt}
\end{equation*}

. . .

- Note that $H^2 = HH = \bf{1}$ so $H^2\ket{0} = \ket{0}$ and $H^2\ket{1} = 
  \ket{1}$.

. . .

- This allows us to get out of superposition without measurement! So we can 
  structure computations deterministically.

##

\Huge The Deutsch Oracle

## The Deutsch oracle

\begin{center}
\begin{tikzpicture}
\draw [color=mLightBrown, ->] (0,0) -- (6, 0);
\draw [color=mLightBrown,fill=white] (2, -.5) rectangle (4,.5) 
node[midway]{Black Box};
\draw (0, 0) node [anchor=east] {Input};
\draw (6, 0) node [anchor=west] {Output};
\end{tikzpicture}
\end{center}

::: incremental

- The \alert{Black Box} is a deterministic function of one bit.

- You can give it whatever input you want, and observe the output.

- How many queries do you need to determine the function on classical computer?

- And on a quantum computer?

:::

## The Deutsch oracle

\begin{center}
\begin{tikzpicture}
\draw [color=mLightBrown, ->] (0,0) -- (6, 0);
\draw [color=mLightBrown,fill=white] (2, -.5) rectangle (4,.5) 
node[midway]{Black Box};
\draw (0, 0) node [anchor=east] {Input};
\draw (6, 0) node [anchor=west] {Output};
\end{tikzpicture}
\end{center}

::: incremental

- What if we want to determine whether the function is constant or variable?

- How many queries on a classical computer?

- And on a quantum computer?

:::

## The Deutsch oracle

\textbf{Problem:} the `constant-0` and `constant-1` functions are 
\alert{non-reversible}. Before we can continue, we have to write them in a 
reversible way:

. . .

::::: {.columns}
::: {.column width=50%}

### Before:

\begin{tikzpicture}
\draw [color=mLightBrown, ->] (0, 0) -- (3, 0);
\draw [color=mLightBrown, fill=white] (1, -1.5) rectangle (2, 1.5) 
node[midway]{BB};
\draw (0, 0) node [anchor=east] {Input};
\draw (3, 0) node [anchor=west] {Output};
\draw (0, 0) node [anchor=south west] {\scalebox{0.8}{$\ket{x}$}};
\draw (2, 0) node [anchor=south west] {\scalebox{0.8}{$f(\ket{x})$}};
\end{tikzpicture}

:::
::: {.column width=50%}

. . .

### After:

\begin{tikzpicture}
\draw [color=mLightBrown, ->] (0, -.5) -- (3, -.5);
\draw [color=mLightBrown, ->] (0, 0.5) -- (3, 0.5);
\draw [color=mLightBrown, fill=white] (1, -1.5) rectangle (2, 1.5) 
node[midway]{BB};

\draw (0, 0.5) node [anchor=east] {Output};
\draw (0, -0.5) node[anchor=east] {Input};
\draw (3, 0.5) node [anchor=west] {Output'};
\draw (3, -0.5) node [anchor=west] {Input'};

\draw (0, .5) node [anchor=south west] {\scalebox{0.8}{$\ket{0}$}};
\draw (0, -.5) node [anchor=south west] {\scalebox{0.8}{$\ket{x}$}};
\draw (2, -.5) node [anchor=south west] {\scalebox{0.8}{$\ket{x}$}};
\draw (2, .5) node [anchor=south west] {\scalebox{0.8}{$f(\ket{x})$}};
\end{tikzpicture}

:::
::::

. . .

The black box leaves the \alert{input qubit} unchanged, writing the output of 
the function to the \alert{output qubit}.

## The Deutsch oracle: `constant-0`

\begin{center}
\begin{tikzpicture}
\draw [color=mLightBrown, ->] (0, -.5) -- (3, -.5);
\draw [color=mLightBrown, ->] (0, 0.5) -- (3, 0.5);
\draw [color=mLightBrown, fill=white] (1, -1.5) rectangle (2, 1.5) 
node[midway]{BB};

\draw (0, 0.5) node [anchor=east] {Output};
\draw (0, -0.5) node[anchor=east] {Input};
\draw (3, 0.5) node [anchor=west] {Output'};
\draw (3, -0.5) node [anchor=west] {Input'};

\draw (0, .5) node [anchor=south west] {\scalebox{0.8}{$\ket{0}$}};
\draw (0, -.5) node [anchor=south west] {\scalebox{0.8}{$\ket{x}$}};
\draw (2, -.5) node [anchor=south west] {\scalebox{0.8}{$\ket{x}$}};
\draw (2, .5) node [anchor=south west] {\scalebox{0.8}{$\ket{0}$}};
\end{tikzpicture}
\end{center}


\begin{center}
\begin{tikzpicture}
\draw [color=mLightBrown] (0, 0) -- (8, 0);
\draw [color=mLightBrown] (0, -1) -- (8, -1);

\draw (0, 0) node [anchor=east] {Output};
\draw (8, 0) node [anchor=west,color=white] {Output};
\draw (0, -1) node [anchor=east] {Input};
\end{tikzpicture}
\end{center}

## The Deutsch oracle: `constant-1`

\begin{center}
\begin{tikzpicture}
\draw [color=mLightBrown, ->] (0, -.5) -- (3, -.5);
\draw [color=mLightBrown, ->] (0, 0.5) -- (3, 0.5);
\draw [color=mLightBrown, fill=white] (1, -1.5) rectangle (2, 1.5) 
node[midway]{BB};

\draw (0, 0.5) node [anchor=east] {Output};
\draw (0, -0.5) node[anchor=east] {Input};
\draw (3, 0.5) node [anchor=west] {Output'};
\draw (3, -0.5) node [anchor=west] {Input'};

\draw (0, .5) node [anchor=south west] {\scalebox{0.8}{$\ket{0}$}};
\draw (0, -.5) node [anchor=south west] {\scalebox{0.8}{$\ket{x}$}};

\draw (2, .5) node [anchor=south west] {\scalebox{0.8}{$\ket{1}$}};
\draw (2, -.5) node [anchor=south west] {\scalebox{0.8}{$\ket{x}$}};
\end{tikzpicture}
\end{center}


\begin{center}
\begin{tikzpicture}
\draw [color=mLightBrown] (0, 0) -- (8, 0);
\draw [color=mLightBrown] (0, -1) -- (8, -1);

\draw [fill=red,draw=mLightBrown] (3.7, -.3) rectangle (4.3, .3) 
node[midway,color=white]{X};

\draw (0, 0) node [anchor=east] {Output};
\draw (8, 0) node [anchor=west,color=white] {Output};
\draw (0, -1) node [anchor=east] {Input};
\end{tikzpicture}
\end{center}

## The Deutsch oracle: identity

\begin{center}
\begin{tikzpicture}
\draw [color=mLightBrown, ->] (0, -.5) -- (3, -.5);
\draw [color=mLightBrown, ->] (0, 0.5) -- (3, 0.5);
\draw [color=mLightBrown, fill=white] (1, -1.5) rectangle (2, 1.5) 
node[midway]{BB};

\draw (0, 0.5) node [anchor=east] {Output};
\draw (0, -0.5) node[anchor=east] {Input};
\draw (3, 0.5) node [anchor=west] {Output'};
\draw (3, -0.5) node [anchor=west] {Input'};

\draw (0, .5) node [anchor=south west] {\scalebox{0.8}{$\ket{0}$}};
\draw (0, -.5) node [anchor=south west] {\scalebox{0.8}{$\ket{x}$}};

\draw (2, .5) node [anchor=south west] {\scalebox{0.8}{$\ket{x}$}};
\draw (2, -.5) node [anchor=south west] {\scalebox{0.8}{$\ket{x}$}};
\end{tikzpicture}
\end{center}


\begin{center}
\begin{tikzpicture}
\draw [color=mLightBrown] (0, 0) -- (8, 0);
\draw [color=mLightBrown] (0, -1) -- (8, -1);

\draw [draw=mLightBrown] (4, 0) circle (.2);
\draw [color=mLightBrown] (4, -1) -- (4, .2);
\filldraw[fill=mLightBrown, draw=mLightBrown] (4, -1) circle (.1);

\draw (0, 0) node [anchor=east] {Output};
\draw (8, 0) node [anchor=west,color=white] {Output};
\draw (0, -1) node [anchor=east] {Input};
\end{tikzpicture}
\end{center}

## The Deutsch oracle: negation

\begin{center}
\begin{tikzpicture}
\draw [color=mLightBrown, ->] (0, -.5) -- (3, -.5);
\draw [color=mLightBrown, ->] (0, 0.5) -- (3, 0.5);
\draw [color=mLightBrown, fill=white] (1, -1.5) rectangle (2, 1.5) 
node[midway]{BB};

\draw (0, 0.5) node [anchor=east] {Output};
\draw (0, -0.5) node[anchor=east] {Input};
\draw (3, 0.5) node [anchor=west] {Output'};
\draw (3, -0.5) node [anchor=west] {Input'};

\draw (0, .5) node [anchor=south west] {\scalebox{0.8}{$\ket{0}$}};
\draw (0, -.5) node [anchor=south west] {\scalebox{0.8}{$\ket{x}$}};

\draw (2, .5) node [anchor=south west] {\scalebox{0.8}{$\ket{\neg x}$}};
\draw (2, -.5) node [anchor=south west] {\scalebox{0.8}{$\ket{x}$}};
\end{tikzpicture}
\end{center}


\begin{center}
\begin{tikzpicture}
\draw [color=mLightBrown] (0, 0) -- (8, 0);
\draw [color=mLightBrown] (0, -1) -- (8, -1);

\draw [draw=mLightBrown] (2.5, 0) circle (.2);
\draw [color=mLightBrown] (2.5, -1) -- (2.5, .2);
\filldraw[fill=mLightBrown, draw=mLightBrown] (2.5, -1) circle (.1);

\draw [fill=red,draw=mLightBrown] (5.2, -.3) rectangle (5.8, .3) 
node[midway,color=white]{X};

\draw (0, 0) node [anchor=east] {Output};
\draw (8, 0) node [anchor=west,color=white] {Output};
\draw (0, -1) node [anchor=east] {Input};
\end{tikzpicture}
\end{center}

## The Deutsch oracle: solution

- So how do we solve the problem in one operation?

. . .

\begin{center}
\begin{tikzpicture}
\draw [color=mLightBrown] (0, 0) -- (6.5, 0);
\draw [color=mLightBrown] (0, -1) -- (6.5, -1);

\draw [fill=red,draw=mLightBrown] (1.5, -.3) rectangle (2.1, .3) 
node[midway,color=white]{X};
\draw [fill=red,draw=mLightBrown] (1.5, -1.3) rectangle (2.1, -.7) 
node[midway,color=white]{X};

\draw [fill=mLightBrown,draw=mLightBrown] (2.5, -.3) rectangle (3.1, .3) 
node[midway,color=white]{H};
\draw [fill=mLightBrown,draw=mLightBrown] (2.5, -1.3) rectangle (3.1, -.7) 
node[midway,color=white]{H};

\draw [fill=white,draw=mLightBrown] (3.5, -1.3) rectangle (4.1, .3) node 
[midway] {BB};

\draw [fill=mLightBrown,draw=mLightBrown] (4.5, -.3) rectangle (5.1, .3) 
node[midway,color=white]{H};
\draw [fill=mLightBrown,draw=mLightBrown] (4.5, -1.3) rectangle (5.1, -.7) 
node[midway,color=white]{H};

\draw [fill=white,draw=mLightBrown] (5.5, -.3) rectangle (6.1, .3) 
node[midway]{M};
\draw [fill=white,draw=mLightBrown] (5.5, -1.3) rectangle (6.1, -.7) 
node[midway]{M};

\draw (0, 0) node [anchor=east] {Output};
\draw (0, -1) node [anchor=east] {Input};
\draw (0, 0) node [anchor=south west] {$\ket{0}$};
\draw (0, -1) node [anchor=south west] {$\ket{0}$};
\end{tikzpicture}
\end{center}

. . .

- If the black-box function is constant, we will measure $\ket{11}$.
- If the black-box function is variable, we will measure $\ket{01}$.

## The Deutsch oracle: check for `constant-0`

\begin{center}
\begin{tikzpicture}
\draw [color=mLightBrown] (0, 0) -- (6.5, 0);
\draw [color=mLightBrown] (0, -1) -- (6.5, -1);

\draw [fill=red,draw=mLightBrown] (1.5, -.3) rectangle (2.1, .3) 
node[midway,color=white]{X};
\draw [fill=red,draw=mLightBrown] (1.5, -1.3) rectangle (2.1, -.7) 
node[midway,color=white]{X};

\draw [fill=mLightBrown,draw=mLightBrown] (2.5, -.3) rectangle (3.1, .3) 
node[midway,color=white]{H};
\draw [fill=mLightBrown,draw=mLightBrown] (2.5, -1.3) rectangle (3.1, -.7) 
node[midway,color=white]{H};


\draw [fill=mLightBrown,draw=mLightBrown] (4.5, -.3) rectangle (5.1, .3) 
node[midway,color=white]{H};
\draw [fill=mLightBrown,draw=mLightBrown] (4.5, -1.3) rectangle (5.1, -.7) 
node[midway,color=white]{H};

\draw [fill=white,draw=mLightBrown] (5.5, -.3) rectangle (6.1, .3) 
node[midway]{M};
\draw [fill=white,draw=mLightBrown] (5.5, -1.3) rectangle (6.1, -.7) 
node[midway]{M};

\draw (0, 0) node [anchor=east] {Output};
\draw (0, -1) node [anchor=east] {Input};
\draw (0, 0) node [anchor=south west] {$\ket{0}$};
\draw (0, -1) node [anchor=south west] {$\ket{0}$};
\end{tikzpicture}
\end{center}

\begin{align*}
\text{Output'} = \Had\Had\NotGate\zero = \one
\end{align*}

\begin{align*}
\text{Input'} = \Had\Had\NotGate\zero = \one
\end{align*}

Output is $\ket{11}$.

## The Deutsch oracle: check for `constant-1`

\begin{center}
\begin{tikzpicture}
\draw [color=mLightBrown] (0, 0) -- (6.5, 0);
\draw [color=mLightBrown] (0, -1) -- (6.5, -1);

\draw [fill=red,draw=mLightBrown] (1.5, -.3) rectangle (2.1, .3) 
node[midway,color=white]{X};
\draw [fill=red,draw=mLightBrown] (1.5, -1.3) rectangle (2.1, -.7) 
node[midway,color=white]{X};

\draw [fill=mLightBrown,draw=mLightBrown] (2.5, -.3) rectangle (3.1, .3) 
node[midway,color=white]{H};
\draw [fill=mLightBrown,draw=mLightBrown] (2.5, -1.3) rectangle (3.1, -.7) 
node[midway,color=white]{H};

\draw [fill=red,draw=mLightBrown] (3.5, -.3) rectangle (4.1, .3) node 
[midway,color=white] {X};

\draw [fill=mLightBrown,draw=mLightBrown] (4.5, -.3) rectangle (5.1, .3) 
node[midway,color=white]{H};
\draw [fill=mLightBrown,draw=mLightBrown] (4.5, -1.3) rectangle (5.1, -.7) 
node[midway,color=white]{H};

\draw [fill=white,draw=mLightBrown] (5.5, -.3) rectangle (6.1, .3) 
node[midway]{M};
\draw [fill=white,draw=mLightBrown] (5.5, -1.3) rectangle (6.1, -.7) 
node[midway]{M};

\draw (0, 0) node [anchor=east] {Output};
\draw (0, -1) node [anchor=east] {Input};
\draw (0, 0) node [anchor=south west] {$\ket{0}$};
\draw (0, -1) node [anchor=south west] {$\ket{0}$};
\end{tikzpicture}
\end{center}

\begin{align*}
\text{Output'} = \Had\NotGate\Had\NotGate\zero = \one
\end{align*}

\begin{align*}
\text{Input'} = \NotGate\Had\Had\zero = \one
\end{align*}

Output is $\ket{11}$.


## The Deutsch oracle: check for the identity

\begin{center}
\begin{tikzpicture}
\draw [color=mLightBrown] (0, 0) -- (6.5, 0);
\draw [color=mLightBrown] (0, -1) -- (6.5, -1);

\draw [fill=red,draw=mLightBrown] (1.5, -.3) rectangle (2.1, .3) 
node[midway,color=white]{X};
\draw [fill=red,draw=mLightBrown] (1.5, -1.3) rectangle (2.1, -.7) 
node[midway,color=white]{X};

\draw [fill=mLightBrown,draw=mLightBrown] (2.5, -.3) rectangle (3.1, .3) 
node[midway,color=white]{H};
\draw [fill=mLightBrown,draw=mLightBrown] (2.5, -1.3) rectangle (3.1, -.7) 
node[midway,color=white]{H};

\draw [draw=mLightBrown] (3.8, 0) circle (.2);
\draw [color=mLightBrown] (3.8, -1) -- (3.8, .2);
\filldraw[fill=mLightBrown, draw=mLightBrown] (3.8, -1) circle (.1);

\draw [fill=mLightBrown,draw=mLightBrown] (4.5, -.3) rectangle (5.1, .3) 
node[midway,color=white]{H};
\draw [fill=mLightBrown,draw=mLightBrown] (4.5, -1.3) rectangle (5.1, -.7) 
node[midway,color=white]{H};

\draw [fill=white,draw=mLightBrown] (5.5, -.3) rectangle (6.1, .3) 
node[midway]{M};
\draw [fill=white,draw=mLightBrown] (5.5, -1.3) rectangle (6.1, -.7) 
node[midway]{M};

\draw (0, 0) node [anchor=east] {Output};
\draw (0, -1) node [anchor=east] {Input};
\draw (0, 0) node [anchor=south west] {$\ket{0}$};
\draw (0, -1) node [anchor=south west] {$\ket{0}$};
\end{tikzpicture}
\end{center}

\scalebox{0.8}{$
C\left(\pmat{\sqrtt \\ -\sqrtt} \otimes \pmat{\sqrtt \\ -\sqrtt}\right) = 
C\pmat{\half \\ -\half \\ -\half \\ \half} = \half \cnot \pmat{1 \\ -1 \\ -1 \\ 
1} = \half \pmat{1 \\ -1 \\ 1 \\ -1}
$}
\scalebox{0.8}{$= \pmat{\sqrtt\\\sqrtt}\otimes\pmat{\sqrtt\\-\sqrtt}$}

Output (after applying Hadamard gate): $\ket{01}$.

## The Deutsch oracle: check for negation

\begin{center}
\begin{tikzpicture}
\draw [color=mLightBrown] (0, 0) -- (7.5, 0);
\draw [color=mLightBrown] (0, -1) -- (7.5, -1);

\draw [fill=red,draw=mLightBrown] (1.5, -.3) rectangle (2.1, .3) 
node[midway,color=white]{X};
\draw [fill=red,draw=mLightBrown] (1.5, -1.3) rectangle (2.1, -.7) 
node[midway,color=white]{X};

\draw [fill=mLightBrown,draw=mLightBrown] (2.5, -.3) rectangle (3.1, .3) 
node[midway,color=white]{H};
\draw [fill=mLightBrown,draw=mLightBrown] (2.5, -1.3) rectangle (3.1, -.7) 
node[midway,color=white]{H};

\draw [draw=mLightBrown] (3.8, 0) circle (.2);
\draw [color=mLightBrown] (3.8, -1) -- (3.8, .2);
\filldraw[fill=mLightBrown, draw=mLightBrown] (3.8, -1) circle (.1);

\draw [fill=red,draw=mLightBrown] (4.5, -.3) rectangle (5.1, .3) 
node[midway,color=white]{X};

\draw [fill=mLightBrown,draw=mLightBrown] (5.5, -.3) rectangle (6.1, .3) 
node[midway,color=white]{H};
\draw [fill=mLightBrown,draw=mLightBrown] (5.5, -1.3) rectangle (6.1, -.7) 
node[midway,color=white]{H};

\draw [fill=white,draw=mLightBrown] (6.5, -.3) rectangle (7.1, .3) 
node[midway]{M};
\draw [fill=white,draw=mLightBrown] (6.5, -1.3) rectangle (7.1, -.7) 
node[midway]{M};

\draw (0, 0) node [anchor=east] {Output};
\draw (0, -1) node [anchor=east] {Input};
\draw (0, 0) node [anchor=south west] {$\ket{0}$};
\draw (0, -1) node [anchor=south west] {$\ket{0}$};
\end{tikzpicture}
\end{center}

\begin{center}
\Huge Left as an exercise :)
\end{center}

Output: $\ket{01}$.

## The Deutsch oracle

- We managed to determine whether the function is constant in one query!

- We magnified the difference between categories (CNOT gate) and neutralized 
  the difference within the categories (NOT gate).

- There is a generalization of this to $n$-bit black boxes (Deutsch-Josza 
  problem).

- It was an inspiration for Shor's algorithm!

# Bonus Topic - Quantum Entanglement

## Quantum entanglement

- If the product state of two qubits cannot be factored, they are said to be 
  \alert{entangled}

:::: {.columns}
::: {.column width=50%}

\begin{equation*}
\pmat{\sqrtt\\0\\0\\\sqrtt} = \pmat{a\\b}\otimes\pmat{c\\d}
\end{equation*}

:::
::: {.column width=50%}
\begin{align*}
ac &= \sqrtt \\
ad &= 0 \\
bc &= 0 \\
bd &= \sqrtt
\end{align*}
:::
::::

- The system of equations has no solution, so we cannot factor the quantum 
  state.

- This state has a $50\%$ chance of collapsing to $\ket{00}$ and $50\%$ chance 
  of collapsing to $\ket{11}$.

## Quantum entanglement

To reach an entangled state is quite simple:

\begin{tikzpicture}
\draw [color=mLightBrown] (0, 0) -- (4, 0);
\draw [color=mLightBrown] (0, -1) -- (4, -1);

\draw [draw=mLightBrown] (3, 0) circle (.2);
\draw [color=mLightBrown] (3, -1) -- (3, .2);
\filldraw[fill=mLightBrown, draw=mLightBrown] (3, -1) circle (.1);

\draw [fill=mLightBrown,draw=mLightBrown] (1.4, -1.3) rectangle (2, -.7) 
node[midway,color=white]{H};

\draw (0, 0) node [anchor=south west] {$\ket{0}$};
\draw (0, -1) node [anchor=south west] {$\ket{0}$};
\end{tikzpicture}

\scalebox{0.8}{$
CH_1\left(\zero\otimes\zero\right) = C\left(\pmat{\sqrtt\\\sqrtt} 
\otimes\zero\right) = \cnot\pmat{\sqrtt\\0\sqrtt\\0} = 
\pmat{\sqrtt\\0\\0\\\sqrtt}
$}

## Quantum entanglement - consequences

- The qubits are forced to be equal (both are either $0$ or $1$)!

- If we measure one of them, the state of the other will collapse to be equal 
  to the one we measured.

- This can happen even if they are very very far away from each other!

- The value is \alert{not predetermined}.

- We cannot use this to transmit information, however.

