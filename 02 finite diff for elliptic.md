# Finite difference methods for elliptic PDEs

$$
\newcommand{\x}{\mathbf x}
\newcommand{\b}{\mathbf b}
\newcommand{\j}{\mathbf j}
\newcommand{\n}{\mathbf n}
\newcommand{\v}{\mathbf v}
\newcommand{\U}{\mathbf U}
\newcommand{\abs}[1]{\left\lvert #1 \right\rvert}
\newcommand{\parens}[1]{\left( #1 \right)}
\newcommand{\brackets}[1]{\left[ #1 \right]}
\newcommand{\angles}[1]{\left\langle #1 \right\rangle}
\newcommand{\inv}[1]{#1^{-1}}
\newcommand{\d}{\, \text{d}}
\newcommand{\dbyd}[2]{\frac{\d #1}{\d #2}}
\newcommand{\partials}[2]{\frac{\partial #1}{\partial #2}}
\newcommand{\BigO}{\mathcal O}
$$

##  Finite difference methods

### Meshing

First step of numerically solving PDEs is to discretize domain

If we discretize into a 1d mesh with resolution $M$, then $h = 1/M$ and $x_j = jh$, and we represent our mesh by $\{x_j : 0 \leq j \leq M\}\}$

### Approximations to $u'(x_j)$

#### Forward difference

Using Taylor expansion
$$
\begin{align*}
u(x_{j+1}) &= u(x_j + h) \\
&= u(x_j) + hu'(x_j) + \BigO(h^2) \\
\text{so } u'(x_j) &\approx \frac{u(x_{j+1}) - u(x_j)}{h}
\end{align*}
$$

Let $U_j = u(x_j) + hu'(x_j)$, then we can define our forward difference operator $\partial$ as

$$
\partial U_j = \displaystyle u'(x_j) \approx \frac{U_{j+1} - U_j}{h}
$$

#### Backward difference

Similarly, 

$$
\begin{align*}
u(x_{j-1}) &= u(x_j) - hu'(x_j) + \BigO(h^2) \\
\text{so } u'(x_j) &\approx \frac{u(x_j) - u(x_{j-1})}{h}
\end{align*}
$$

We define our backward difference operator $\overline \partial$ as

$$
\overline \partial U_j = \frac{U_j - U_{j-1}}{h}
$$

#### Central difference

$$
\begin{align*}
u(x_{j+1}) &= u(x_j) + hu'(x_j) + \BigO(h^2) \\
u(x_{j-1}) &= u(x_j) - hu'(x_j) + \BigO(h^2) \\
u(x_{j+1}) - u(x_{j-1}) &= 2hu'(x_j) + \BigO(h^2) \\
u'(x_j) &\approx \frac{u(x_{j+1}) - u(x_{j-1})}{2h}
\end{align*}
$$

The central difference operator $\hat U_j$ is

$$
\hat U_j = \frac{U_{j+1} - U_{j-1}}{2h}
$$

### Approximating higher order derivatives

We can combine our first order operators to calculate higher order derivatives

E.g.

$$
\begin{align*}
u''(x_j)  = (u')'(x_j) &\approx \partial \overline \partial U_j \\
&= \partial \parens{ \frac{U_j - U_{j-1}}{h} } \\
&= \frac{U_{j+1} - 2U_j + U_{j-1}}{h^2}
\end{align*}
$$

### 1d example

Consider a 1d problem about $u(x)$

$$
\begin{align*}
-u'' &= f \text{ on } \Omega = (0, 1) \\
u(0) &= u_0 \\
u(1) &= u_1
\end{align*}
$$
Discretizing into $M$ steps, we have

- $U_0 = u_0$
- $U_M = u_1$
- $-\partial \overline \partial U_j = f(x_j)$ for $j = 1, ..., M-1$

We can eliminate $U_0$ and $U_M$ from our linear system by realizing

- $-\partial \overline \partial U_1 = f_1$ tells us that $-(U_2 - 2U_1)/h^2 = f_1 + u_0 / h^2$
- $-\partial \overline \partial U_{M-1} = f_{M-1}$ tells us that $-(2U_{M-1} + U_{M-2})/h^2 = f_{M-1} + u_1/h^2$

(this is not necessary, but in practice it helps)

This is a fully determined linear system in $U_1, ..., U_{M-1}$:
$$
\underbrace{\begin{bmatrix}
2 & -1 & 0 & 0 & \cdots & 0 & 0 & 0 \\
-1 & 2 & -1 & 0 & \cdots & 0 & 0 & 0 \\
0 & -1 & 2 & -1 & \cdots & 0 & 0 & 0 \\
\vdots & \vdots & \vdots & \vdots & \ddots & \vdots & \vdots & \vdots \\
0 & 0 & 0 & 0 & \cdots & -1 & 2 & -1\\
0 & 0 & 0 & 0 & \cdots & 0 & -1 & 2
\end{bmatrix}}_A
\underbrace{\begin{bmatrix}
U_1 \\ U_2 \\ U_3 \\ \vdots \\ U_{M-2} \\ U_{M-1}
\end{bmatrix}}_\U
=
\underbrace{\begin{bmatrix}
f_1 + u_0/h^2 \\
f_2 \\
f_3 \\
\vdots \\
f_{M-2} \\
f_{M-1} + u_1/h^2
\end{bmatrix}}_\b
$$
$A$ is symmetric and sparse, so we can easily solve $A \x = \b$ to calculate all of our $U_j$ values and therefore solve for $u(x_0), ..., u(x_M)$. We can interpolate these to calculate other values of $u(x)$ on our domain

### Proving usefulness

#### Stability

##### Stability lemma

*First, a lemma:*

If $-\partial \overline \partial U_j \leq 0$ then $\displaystyle \max_j U_j = \max\{U_0, U_M\}$ (i.e. if $U$ is convex then its max is at one of its endpoints)

*Proof.*

Since $\U$ satisfies $-\partial \overline \partial U_j \leq 0$, we can expand this to find that $U_j \leq \frac{1}{2} U_{j-1} + \frac{1}{2} U_{j+1}$

Suppose $\U$ has a maximum at an interior point $U_j$ with $j \in \{1, ..., M-1\}$

If $U_{j+1} < U_j$, then

$$
U_j \leq \frac{1}{2} U_{j-1} + \frac{1}{2} U_{j+1} < \frac{1}{2} U_j + \frac{1}{2} U_{j+1} \\
U_j < U_{j-1}
$$

But this contradicts our assumption that $U_j$ is a maximum point. We arrive at a similar result if we assume that $U_{j-1} < U_j$. Thus, if $U_j$ is an interior max then $U_{j-1} = U_j = U_{j+1}$. Extrapolating this logic, $U$ must be constant.

##### Stability

We denote the max norm as $\displaystyle \abs{\U}_S = \max_{x_j \in S} \abs{U_j}$ 

Suppose $\U$ solves $-\partial \overline \partial U_j = f_j$ with $U_0 = u_0$ and $U_1 = u_1$. Then

$$
\begin{align*}
\abs{\U}_{\overline \Omega} \leq \max \{\abs{U_0}, \abs{U_M}\} + c\abs{\partial \overline \partial \U}_\Omega \\
\abs{\U}_{\overline \Omega} \leq \max \{\abs{U_0}, \abs{U_M}\} + c\abs{\mathbf f}_\Omega
\end{align*}
$$

*Proof.*

Let $\displaystyle w(x) = x - x^2 = \frac{1}{4} - \parens{x - \frac{1}{2}}^2$ and let $W_j = w(x_j)$

Then
$$
\begin{align*}
-\partial \overline \partial W_j &= -\frac{1}{h^2} (W_{j+1} - 2W_j + W_{j-1}) \\
&= \frac{1}{h^2} (x_{j+1} - x_{j+1}^2 - 2x_j + 2x_j^2 + x_{j-1} - x_{j-1}^2) \\
&= 2 \tag{$x_{j+1} = x_j + h$ and $x_{j-1} = x_j - h$}
\end{align*}
$$

Now set $U_j^\pm = \pm U_j - \frac{1}{2} \abs{\partial \overline \partial \U}_\Omega W_j$. Then
$$
\begin{align*}
-\partial \overline \partial U_j^\pm
&= \pm (-\partial \overline \partial U_j) + \frac{1}{2} \abs{\partial \overline \partial \U}_\Omega \partial \overline \partial W_j \\
&= \pm (-\partial \overline \partial U_j) + \abs{\partial \overline \partial \U}_\Omega \partial \overline \partial \\
&\leq 0
\end{align*}
$$
Since $-\partial \overline \partial U_j^\pm \leq 0$ we can apply the previous lemma to find
$$
\begin{align*}
U_j^\pm &\leq \max \{U_0^\pm , U_M^\pm\} \\
&= \max\{\pm U_0, \pm U_M\} \tag{$W_0, W_M = 0$} \\
\abs{\U}_{\overline \Omega} &\leq \max\{\abs{U_0}, \abs{U_M}\} + \frac{1}{2} [...]
\end{align*}
$$

Now we can show existence and uniqueness - if $A$ is invertible then $\U$ exists and is unique. $A$ is nonsingular if $A\U = \mathbf 0$ implies that $\U = \mathbf 0$

#### Consistency

Define the truncation error $\tau_j$ as the error when substituting the exact solution of a PDE into the discretization of the PDE. (in the case of our example, $\tau_j = \partial \overline \partial u(x_j) + f_j$)

A method is **consistent** if $h \to 0$ implies that $\abs{\tau_j} \to 0$ for any PDE

#### Convergence/error estimate