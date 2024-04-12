# Implementing finite element method

$$
\newcommand{\x}{\mathbf x}
\newcommand{\y}{\mathbf y}
\newcommand{\f}{\mathbf f}
\newcommand{\j}{\mathbf j}
\newcommand{\n}{\mathbf n}
\newcommand{\v}{\mathbf v}
\newcommand{\U}{\mathbf U}
\newcommand{\abs}[1]{\left\lvert #1 \right\rvert}
\newcommand{\norm}[1]{\big\lVert #1 \big\rVert}
\newcommand{\parens}[1]{\left( #1 \right)}
\newcommand{\brackets}[1]{\left[ #1 \right]}
\newcommand{\angles}[1]{\left\langle #1 \right\rangle}
\newcommand{\curlies}[1]{\left\lbrace #1 \right\rbrace}
\newcommand{\inv}[1]{#1^{-1}}
\newcommand{\d}{\, \text{d}}
\newcommand{\dbyd}[2]{\frac{\d #1}{\d #2}}
\newcommand{\partials}[2]{\frac{\partial #1}{\partial #2}}
\newcommand{\BigO}{\mathcal O}
\newcommand{\disclapl}[1][]{\partial_{#1} \overline \partial_{#1}}
\newcommand{\Domain}{\overline \Omega}
\DeclareMathOperator{\span}{span}
$$

## Model problem

Consider the model problem

$$
\begin{cases}
-\dbyd{}{x} \parens{\kappa(x) \dbyd{u}{x}} = f(x) &\text{in } \Omega = (0, 1) \\
u(0) = \alpha \\
-\kappa(1) \dbyd{u}{x}(1) = \beta
\end{cases}
$$

### Nonzero Dirichlet boundary condition

To deal with this, we assume there exists $G: \R \to \R$ so that $G(0) = \alpha$. Define $w = u - G$, then $w(0) = 0$.

We will derive a weak formulation for $w$ and find $u = w + G$

Let $v$ be smooth enough, so

$$
\begin{align*}
-v (\kappa u')' &= fv \tag{within $\Omega$} \\
-\int_0^1 v \dbyd{}{x} \parens{\kappa \dbyd{u}{x}} \d x &= \int_0^1 fv \d x \\
-\int_0^1 v \dbyd{}{x} \parens{\kappa(x) \parens{\dbyd{w}{x} + \dbyd{G}{x}}} \d x &= \int_0^1 fv \d x \tag{$u = w + G$} \\
\int_0^1 \dbyd{v}{x} \kappa(x) \parens{\dbyd{w}{x} + \dbyd{G}{x}} \d x \quad&\\
- v(x) \kappa(x) \parens{\dbyd{w}{x} + \dbyd{G}{x}} \Bigg\vert_0^1 &= \int_0^1 fv \d x \tag{int. by parts}
\end{align*}
$$

Let $V = \curlies{v \in L^2(\Omega) : v' \in L^2(\Omega) \text{ and } v(0) = 0}$ and $V_\alpha = \curlies{v \in L^2(\Omega) : v' \in L^2(\Omega) \text{ and } v'(0) = \alpha}$.

Let $v \in V$ and $G \in V_{\alpha}$. Then,

$$
\begin{align*}
-v(x) \kappa(x) \parens{\dbyd{w}{x} + \dbyd{G}{x}} \Bigg\vert_0^1 &= -v(1) \kappa(1) \parens{\dbyd{w}{x}(1) + \dbyd{G}{x}(1)} \tag{$v(0) = 0$} \\
&= v(1) \beta \tag{boundary condition}
\end{align*}
$$

Thus, our weak formulation becomes to find $w \in V$ so that

$$
\int_0^1 \kappa(x) \dbyd{w}{x}(x) \dbyd{v}{x}(x) \d x = \int_0^1 f(x) v(x) \d x - \int_0^1 \kappa(x) \dbyd{G}{x}(x)\dbyd{v}{x} \d x - v(1) \beta
$$

## FEM

Let $V_h = \span\curlies{\phi_1, ..., \phi_N}$ where $\curlies{\phi_i}_i$ are the basis hat functions, then $V_h$ is a finite dimensional subspace of $V$.

So any $v_h \in V_h$ can be written as $v_h(x) = \sum_{j=1}^N V_j \phi_j(x)$

To impose the dirichlet boundary conditions, we assume $G \in V_\alpha$ exists, i.e. $G \in L^2(\Omega)$, $G' \in L^2(\Omega)$, and $G'(0) = \alpha$.

For now,

$$
u_h(x) = G(x) + \underbrace{\sum_{j=1}^N U_j \phi_j(x)}_{w_h(x)}
$$

So we want to solve for the unknowns $U_1, ..., U_N$.

The finite dimensional version of our weak formulation is thus

$$
\text{find } u_h = G + w_h, w_h \in V_h \text{ so that } a(w_h, v_h) = F(v_h) \text{ for all } v_h \in V_h
$$

### Linear system

Let $w_h(x) = \sum_{j=1}^N U_j \phi_j (x)$ and $v_h(x) = \sum_{j=1}^N V_j \phi_j(x)$

Then

$$
\begin{align*}
a(w_h, v_h) &= \int_0^1 \kappa(x) \dbyd{}{x} \parens{\sum_{j=1}^N W_j \phi_j(x)} \dbyd{}{x} \parens{\sum_{i=1}^N V_i \phi_i (x)} \d x \\
&= \sum_{i=1}^N \sum_{j=1}^N V_i U_j \int_0^1 \kappa(x) \dbyd{\phi_i}{x}(x) \dbyd{\phi_j}{x}(\dbyd{\phi_j}{x}) \d x
\end{align*}
$$

$$
\begin{align*}
F(v_h) &= ... \\
&= \sum_{i=1}^N V_i \parens{ \int_0^1 f(x) \phi_i(x) \d x - \phi(1) \beta - \int_0^1 \kappa(x) \dbyd{G}{x}(x) \dbyd{\phi_i}{x}(x) \d x}
\end{align*}
$$

[...]

Define

$$
\begin{align*}
a_{ij} &= \int_0^1 \kappa(x) \dbyd{\phi_j}{x} \dbyd{\phi_i}{x} \d x \\
F_i &= \int_0^1 \phi_i f(x) \d x - \phi_i(1) \beta - \int_0^1 \kappa(x) \dbyd{G}{x} \dbyd{\phi_i}{x} \d x \end{align*}
$$
then

$$
\sum_{j=1}^N U_j a_{ij} = F_i \text{ for } i = 1, \cdots, N
$$

(note that we do not have a $j=0$ term because in our example problem we have a Dirichlet boundary condition)

We can rewrite this linear equation as a matrix equation $A \U = \mathbf F$ where

$$
\begin{align*}
A &= [a_{ij}] \in \R^{N \times N} \\
\mathbf F &= [F_i] \in \R^n \\
\U &= [U_i] \in \R^n
\end{align*}
$$

## Solving??

We choose continuous piecewise polynomial approximation for $\phi_i$ from the subspace $V_h \subset V$

$$
V_h = \curlies{v_h \in C^0(\overline \Omega) : v_h \in P^1(k_j), \forall k_j \in \Omega, v_h(0) = 0}
$$

where $k_j$ are our "finite elements", i.e. in our 1D case $k_j = [x_{j-1}, x_j]$

- $\phi_j$ is a global basis function
- $\phi_j$ is nonzero only on two elements (the elements that share node $x_j$)

Since the basis functions are zero almost everywhere, it is convenient to introduce local basis functions that are only defined on each $k_j$:

$$
\begin{align*}
\psi_{j,1}: k_j &\to [0, 1] \\
x &\mapsto \frac{x_j - x}{x_j - x_{j-1}} \\
\psi_{j,2}: k_j &\to [0, 1] \\
x &\mapsto \frac{x - x_{j-1}}{x_j - x_{j-1}}
\end{align*}
$$

So our global basis functions $\phi_j$ can be written piecewise as $\psi_j$ and $\psi_{j+1}$ on their support and $0$ elsewhere.

To simplify implementation, we also introduce a coordinate transformation $F_{k_j}$ from any physical element $k_j$ to a reference element $\hat k = [0, 1]$ with local coordinate $\xi$

$$
\begin{align*}
F_{k_j}: [0, 1] &\to k_j = [x_{j-1}, x_j] \\
\xi &\mapsto x_{j-1} + h \xi
\end{align*}
$$

We can make corresponding basis function on these reference elements

$$
\begin{align*}
\hat \psi_1(\xi) &= 1 - \xi \\
\hat \psi_2(\xi) &= \xi
\end{align*}
$$

i.e., $\psi_{j, -}(x) = \hat \psi_-(\inv F_{k_j}(x))$

We need a $G$ such that $G(0) = \alpha$. We simply take $G(x) = \alpha \phi_0(x) = \alpha \psi_{1, 1}(x)$

The integral over $\Omega = (0, 1)$ can be split into a sum of integrals over our finite elements $k_j$

$$
\sum_{j=1}^N U_j \parens{ \sum_{k=1}^N \int_{k_j} \kappa(x) \dbyd{\phi_j}{x} \dbyd{\phi_i}{x} \d x } = \sum_{k=1}^N \int_{k_k} f \phi_i \d x - \beta\phi_i(1) - \alpha \sum_{k=1}^N \int_{k_k} \kappa(x) \dbyd{\phi_0}{x} \dbyd{\phi_i}{x} \d x
$$

This must hold for $i = 1, \cdots, N$

Thus the $i$th row of our matrix corresponds to test function $\phi_i$. We know that $\phi_i$ is nonzero only on $k_i$ and $k_{i+1}$​

#### First row

Constructing the first row of our left hand side,

$$
\parens{\int_{k_1} \kappa(x) \dbyd{\phi_1}{x} \dbyd{\phi_1}{x}} U_1
+ \parens{\int_{k_2} \kappa(x) \dbyd{\phi_2}{x} \dbyd{\phi_1}{x}} U_1
+ \parens{\int_{k_2} \kappa(x) \dbyd{\phi_2}{x} \dbyd{\phi_1}{x}} U_2
$$

Constructing the first entry of the right hand side,

$$
\int_{k_1} f\phi_1 \d x
+ \int_{k_2} f\phi_1 \d x
-\alpha \int_{k_1} \kappa \dbyd{\phi_0}{x}\dbyd{\phi_1}{x} \d x
$$

#### Last row

Consider $i=N$. Note that $\phi_N$ is only nonzero on $k_N$! So constructing the left hand side,

$$
\parens{ \int_{k_N} \kappa(x) \dbyd{\phi_{N-1}}{x} \dbyd{\phi_N}{x} \d x} U_{N-1}
+ \parens{ \int_{k_N} \kappa(x) \dbyd{\phi_N}{x} \dbyd{\phi_N}{x} \d x} U_N
$$

For the right hand side, note that we are using our Neumann boundary condition, so

$$
\int_{k_N} f \phi_N \d x - \beta
$$

#### Interior rows

Let $i=k$, then we know $\phi_k(x) \neq 0$ when $x \in k_k$ or $x \in k_{k+1}$

So our left hand side will be

$$
\parens{\int_{k_k} \kappa \dbyd{\phi_{k-1}}{x} \dbyd{\phi_k}{x}}U_{k-1}
+ \parens{\int_{k_k} \kappa \dbyd{\phi_k}{x} \dbyd{\phi_k}{x}}U_k
+ \parens{\int_{k_{k+1}} \kappa \dbyd{\phi_k}{x} \dbyd{\phi_k}{x}}U_k
+ \parens{\int_{k_{k+1}} \kappa \dbyd{\phi_{k+1}}{x} \dbyd{\phi_k}{x}} U_{k+1}
$$

And our right hand side will be

$$
\int_{k_k} f \phi_k \d x + \int_{k_{k+1}} f \phi_k \d x
$$

### Local basis functions

What we have above is good, but it's still written in terms of our global basis functions. It'll be easier to deal with if we use local basis functions.

Introduce

$$
\begin{align*}
A_{ij}^{(k)} &= \int_{k_k} \kappa \dbyd{\psi_{k,j}}{x} \dbyd{\psi_{k,i}}{x} \d x \text{ where } A^{(k)} = [A_{ij}^{(k)}] \in \R^{2 \times 2} \tag{i.e. $i, j \in \curlies{1, 2}$} \\
F_i^{(k)} &= \int_{k_k} f \psi_{k,i} \d x \text{ where } F^{(k)} = [F_i^{(k)}] \in \R^2
\end{align*}
$$

Using these simplifications, we can rewrite our row equations:

$$
\begin{align*}
\parens{ A_{22}^{(1)} + A_{11}^{(2)} } U_1 + A_{12}^{2} U_2 &= F_2^{(1)} + F_1^{(2)} - \alpha A_{21}^{(1)} \\
A_{21}^{(k)} U_{k-1} + \parens{A_{22}^{(k)} + A_{11}^{k+1}} U_k + A_{12}^{(k+1)} U_{k+1} &= F_2^{(k)} + F_1^{(k+1)} \\
A_{21}^{(N)} U_{N-1} + A_{12}^{(N)}U_N &= F_2^{(N)} - \beta
\end{align*}
$$

Assembling into one matrix for this linear system, we get a tridiagonal matrix equation:

$$
\begin{pmatrix}
(A_{22}^{(1)} + A_{11}^{(2)}) & A_{12}^{(2)} \\
& A_{21}^{(2)} & (A_{22}^{(2)} + A_{11}^{(3)}) & A_{12}^{(3)}  \\
& & \ddots & \ddots & \ddots \\
& & & A_{21}^{(N-1)} & (A_{22}^{(N-1)} + A_{11}^{(N)}) & A_{12}^{(N)}  \\
& & & & A_{21}^{(N)} & A_{12}^{(N)}
\end{pmatrix}
\begin{pmatrix}
U_1 \\
U_2 \\
\vdots \\
U_{N-1} \\
U_N
\end{pmatrix}
=
\begin{pmatrix}
F_2^{(1)} + F_1^{(2)} - \alpha A_{21}^{(1)} \\
F_2^{(2)} + F_1^{(3)} \\
\vdots \\
F_2^{(N-1)} + F_1^{(N)} \\
F_2^{(N)} - \beta
\end{pmatrix}
$$

Now, to compute the coefficients!

Note that using the chain rule on our coordinate transformation,

$$
\begin{align*}
\psi_{j,m} &= \hat \psi_m \circ \inv F_{k_j} \\
\dbyd{\psi_{j,m}}{x} &= \frac{1}{h_k} \dbyd{\hat \psi_m}{x}
\end{align*}
$$

$$
\begin{align*}
A_{ij}^{(k)} &= \int_{k_k} \kappa \dbyd{\psi_{k,j}}{x}\dbyd{\psi_{k,i}}{x} \d x \\
&= \int_0^1 \kappa(\inv F_{k_k}(\xi)) \parens{\frac{1}{h_k} \dbyd{\hat \psi_j}{\xi}}\parens{\frac{1}{h_k} \dbyd{\hat \psi_i}{\xi}} h_k \d \xi \\
&= \frac{1}{h_k} \int_0^1 \kappa(\inv F_{k_k}(\xi)) \dbyd{\hat \psi_j}{\xi} \dbyd{\hat \psi_i}{\xi} \d \xi \\
\end{align*}
$$


To actually compute these integrals over $[0, 1]$, we must use quadrature to approximate

$$
\int_0^1 g(\xi) \d \xi \approx \sum_{i=1}^n \frac12 b_i g\parens{\frac12 c_i + \frac12}
$$

Smarter people have come up with the $b_i$s and $c_i$s for each $n$
