+++
title = "Codes with Locality for Distributed Storage"
speaker = "Lalitha Vadlamani"
speakerhomepage = "https://faculty.iiit.ac.in/~lalitha.v/"
speakerinstitute = "SPCRC, IIIT-H"
speakerinstitutehomepage = "http://spcrc.iiit.ac.in"
talkdate = 2019-05-07T16:00:00+09:00
venue = "A3 117 Conf. Hall, CSTAR Corridor"
meta = true
math = true
toc = true
categories = ["Communication", "Information Theory", "Distributed Storage System"]
abstract = """
In a distributed storage system, due to increase of storage capacity of a node, efficient repair of failed nodes is becoming increasingly important in addition to ensuring a given level of reliability and low storage overhead. Codes with locality are a class of codes designed for storage systems which have the characteristic that they trade off repair locality (number of nodes accessed to repair a failed node) for storage overhead. In this talk, we will survey the following results related to codes with locality:

1) Tradeoff between repair locality and storage overhead using a Singleton-like bound and a class of optimal codes known as Pyramid codes.

2) Pyramid codes provide repair locality for the message symbols. A Reed-Solomon code like construction has been proposed in the recent literature, which provides locality to all the codeword symbols and is optimal with respect to the Singleton-like bound. 

3) A variant of codes with locality known an maximally recoverable codes. These codes can recover from all failure patterns which are information theoretically recoverable given the repair locality constraints.
"""
scribe_name="Girish Varma"
scribe_link="http://girishvarma.in"
+++

## Error Correcting Codes
Error correcting codes is a fundamental tool used in various fields. 
They were primarily proposed for solving the problem of communication through a noisy channel.

Alice has a message $ m \in \\{0,1\\}^k $ and she needs to sent it to Bob
through a channel. The channel is noisy because of which the message recieved by Bob,
might have some bits changed. The communication problem is to design a system such that
Bob can recover Alice's message. 

Alice can encode her message $m$ as a longer string $x \in \\{0,1\\}^n$
where $n>m$ by adding redundant bits. The encoding function $\text{Enc}:\\{0,1\\}^k \rightarrow \\{0,1\\}^n$,
should be such that even if some $d$ bits of $x :=\text{Enc}(m)$ is flipped, Bob should be able to 
recover $m$ from the string he recieves (say $x'$). A necessary condition for this is that $\text{Enc}$
is a one-one function. For the decoding to happen, we will need more conditions on the range of this 
function. The range of $\text{Enc}$ is called a code, which we will denote by $\mathfrak{C}$.

There are two important parameters for a code called rate and minimum distance.

{{% definition name="Minimum Distance" %}}
The minimum distance of a code $\mathfrak C$, denoted by $d\_{\min}$ is the minimum Hamming distance
between any pair of codewords (elements of $\mathfrak C$). 
$$
    d\_{\min} = \min\_{a,b \in \mathfrak{C}} d\_H(a,b) 
$$
{{% /definition %}}


{{% exercise %}}

If $\mathfrak{C}$ is a code with minimum distance $d$, then  design encoding and decoding functions such 
that even if the channel flips $\lfloor(d-1)/2 \rfloor$ bits, Bob can recover/decode Alice's message.

{{% /exercise %}}



{{% definition name="Rate" %}}
The rate of a code $\mathfrak C \subseteq \\{0,1\\}^n$, denoted by $r$ is a fraction given by: 
$$
    r = \frac{k}{n} \qquad \text{ where } k = \log | \mathfrak{C} |
$$
Note that $|\mathfrak{C}| = 2^{rn}$ (assuming binary alphabet).
{{% /definition %}}

{{% exercise %}}
Show that:

- If the code is $\\{0,1\\}^n$, $r=1$ and $d\_{\min} = 1$.
- If the code is $\\{x \in \\{0,1\\}^n: x \text{ has even no of 1's} \\}$ then $r= 1 - 1/n$ and $d\_{\min} = 2$.
{{% /exercise %}}

{{% exercise %}}

What is the rate and minimum distance of the code: 
$$\\{x \in \\{0,1\\}^{2n}: x \text{ has exactly n 1's} \\}.$$

{{% /exercise %}}



## Linear Codes

Earlier we disscussed codes where the alphabet is binary $\\{0,1\\}$. Codes can be made with larger alphabets as well.
Of particular interest is when the alphabet is a finite field $\mathbb{F}_p$. Note that the elements of $\mathbb{F}_p$
are $\\{0,\ldots, p-1\\}$ and there is a definition of addition and multiplication (modulo $p$). The code is a 
subset of $\mathbb{F}_p^n$ where $\mathbb{F}_p^n$ is the vector space of dimension $n$ over the field $\mathbb{F}_p$.  


{{% definition name="Linear Codes" %}}
Linear codes are codes where $\mathfrak{C}$ is a subspace of $\mathbb{F}_p^n$. That is it is closed under the
operation of scalar linear combinations.
{{% /definition %}}

{{% exercise %}}
If $k$ is the dimension of the subspace $\mathfrak{C}$, then the rate of the code $r= k/n$.
{{% /exercise %}}

An interesting property of the linear code is that the minimum distance has an alternative characterization.


{{% exercise %}}
Let
$$
w\_\min = \min\_{c \in \mathfrak{C} \setminus \\{ 0 \\} } |c| ~~\text{ where } |c| \text{ is the number of 1's in } c
$$
Show that for a linear code $w\_\min = d\_\min$.
{{% /exercise %}}



For linear code, the encoding function is a simple matrix multiplication. 
This is because the linear code $\mathfrak{C}$ forms a subspace of $\mathbb{F}_p^n$ say of dimension $k$.
Hence $\mathfrak{C}$ has $k$ linearly independent basis vectors that span it.


{{% definition name="Generator Matrix" %}}
Generator matrix $M$ of $\mathfrak{C}$ is a $k\times n$ matrix with $k$ linearly independent vectors as the rows. Hence
any message $m \in \mathbb{F}\_p^k$ can be encoded by doing the matrix multiplication:
$$
\begin{bmatrix} m_1, m_2, \cdots, m_k \end{bmatrix} \times \begin{bmatrix} M_1\\\ M_2\\\ \vdots\\\ M_k \end{bmatrix}
$$
where $M_1,\ldots, M_k$ forms a basis of $\mathfrak{C}$.
{{% /definition %}}


{{% definition name="Systematic Generator Matrix" %}}
Systematic Generator Matrix is a Generator Matrix where the first $k\times k$ part is an identity matrix.
$$
G = \begin{bmatrix} ~ I\_{k\times k} ~ | ~ P\_{k\times n-k} ~\end{bmatrix}
$$
If the message is encoded using such a matrix, the first $k$ bits are exactly
the bits of the message and the last $n-k$ bits are called parity bits, which adds redundancy for recovery.
{{% /definition %}}


{{% exercise %}}
Prove that every linear code has a systematic generator matrix.
{{% /exercise %}}

## Rate vs Distance Tradeoffs

The rate $r$ of a code $\mathfrak{C} \subseteq \mathbb{F}\_p^n$, is a measure of the number of codewords. 
With such a code, we can reliably sent a message of length $k=rn$, using $n$ usage of the channel, as
long as the channel introduces noise in atmost $d\_\min$ bits. Hence we would like $k$ (or $r$) as well as
$d_\min$ to be large. However larger $k$ imposes limits on how large $d\_\min$ can be.


{{% lemma %}}
Let $T \subseteq \\{1,\ldots, n\\}$ such that $|T| = n-d\_\min + 1$ then $G|\_T$ is full rank.
{{% /lemma %}}


{{% lemma %}}
Let $T \subseteq \\{1,\ldots, n\\}$ such that $\text{rank}(G|\_T) \leq k-1$ then $d\_\min \leq n - |T|$ with equality iff $T$ is the set of largest cardinality.
{{% /lemma %}}

{{% lemma name="Singleton Bound" %}}
For any code with rate $r$, minimum distance $d\_\min$ and $k:= rn$:
$$d\_\min \leq n-k+1$$
{{% /lemma %}}


## Reed Solomon Codes

{{% definition name="$(n,k)_q$-code" %}}
$\mathfrak{C}$ is an $(n,k)_q$-code if $\mathfrak{C} \subseteq \\{0,\cdots q-1\\}^n$ and $|\mathfrak{C}| = q^k$.
{{% /definition %}}

Reed Solomon code is an $(n,k)_q$-code for $n < q$ that achieves the Singleton bound.

{{% definition name="Reed Solomon Code" %}}
Let $\\{ \alpha_1, \ldots, \alpha_n\\} \subseteq \\{0,\cdots p-1\\}$. Reed Solomon code is the linear code (or subspace of $\mathbb{F}_p^n$) defined by the generator matrix
\begin{bmatrix} 
1& 1& \cdots& 1\\\ 
\alpha_1 & \alpha_2 & \cdots & \alpha_n\\\ 
\vdots & \vdots & \vdots & \vdots\\\ 
\alpha_1^{k-1} & \alpha_2^{k-1} & \cdots & \alpha_n^{k-1} 
\end{bmatrix}
{{% /definition %}}

{{% exercise %}}
Show that the Reed Solomon code has minimum distance
$$
d_\min = n - k + 1
$$
{{% /exercise %}}

## Codes for Distributed Storage

In this section, we will address the problem of distributed data 
storage. Suppose we have $n$ storage servers which are prone to
failures. We would like to store the data such that even if one
server goes down, we can recover the data. A very simple approach
will be to store copies of the data (say of size $k$) in each of the
servers. But this approach is highly inefficient since the total storage
used is $n\times k$. 


{{% exercise %}}
Assume the data is a $k$ bit string. Propose an approach that uses only
$k+2$ bits of total storage using Reed Solomon Codes.
{{% /exercise %}}

Another important requirement of such storage systems is that, we need
to be able to recover every bit of the codeword by looking at only few (say $t$) other positions in the codeword.

{{% definition name="Locally Recoverable Codes" %}}
A code $\mathfrak{C} \subseteq \\{0,1\\}^n$ is $t$-localy recoverable, if for every $i \in [n]$ there exists a set $S\_i = \\{ a\_1, a\_2, \ldots, a\_t \\}  \subseteq [n] \setminus \\{ i \\}$ and $\\{ \alpha\_j \\}\_{ j \in [t] }$ such that for every codeword $c \in \mathfrak{C}$
$$
c\_i = \sum_{j \in S\_i} \alpha\_i c_j 
$$
$t$ is called the information locality parameter of the code.
{{% /definition %}}


{{% lemma name="Singleton Like Bound for Localy Recoverable Codes" %}}
For any $(n,k)\_q$-code with minimum distance $d\_\min$ and information locality $t$
$$
d\_\min = n- k + 1 - \left\lceil \frac{k}{t} - 1 \right\rceil
$$

{{% /lemma %}}
