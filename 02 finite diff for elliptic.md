# Finite difference methods for elliptic PDEs

$$
\newcommand{\x}{\mathbf x}
\newcommand{\f}{\mathbf f}
\newcommand{\j}{\mathbf j}
\newcommand{\n}{\mathbf n}
\newcommand{\v}{\mathbf v}
\newcommand{\U}{\mathbf U}
\newcommand{\abs}[1]{\left\lvert #1 \right\rvert}
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
$$

##  Finite difference methods

### Meshing

First step of numerically solving PDEs is to discretize domain

If we discretize into a 1d mesh with resolution $M$, then $h = 1/M$ and $x_j = jh$, and we represent our mesh by $\curlies{ x_j : 0 \leq j \leq M}$

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
u''(x_j)  = (u')'(x_j) &\approx \disclapl U_j \\
&= \partial \parens{ \frac{U_j - U_{j-1}}{h} } \\
&= \frac{U_{j+1} - 2U_j + U_{j-1}}{h^2}
\end{align*}
$$

## 1d Poisson problem

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
- $-\disclapl U_j = f(x_j)$ for $j = 1, ..., M-1$

We can eliminate $U_0$ and $U_M$ from our linear system by realizing

- $-\disclapl U_1 = f_1$ tells us that $-(U_2 - 2U_1)/h^2 = f_1 + u_0 / h^2$
- $-\disclapl U_{M-1} = f_{M-1}$ tells us that $-(2U_{M-1} + U_{M-2})/h^2 = f_{M-1} + u_1/h^2$

(this is not necessary, but in practice it helps)

This is a fully determined linear system in $U_1, ..., U_{M-1}$:

$$
\underbrace{\frac{1}{h^2} \begin{bmatrix}
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
= \underbrace{\begin{bmatrix}
f_1 + u_0/h^2 \\
f_2 \\
f_3 \\
\vdots \\
f_{M-2} \\
f_{M-1} + u_1/h^2
\end{bmatrix}}_\f
$$

$A$ is symmetric and sparse, so we can easily solve $A \U = \f$ to calculate all of our $U_j$ values and therefore solve for $u(x_0), ..., u(x_M)$. We can interpolate these to calculate other values of $u(x)$ on our domain.

Note that $\U$ is only interior points, we will use $\overline \U = (u0, U_1, ..., U_{m-1}, u_1)$

### Proving usefulness

#### Stability

##### Stability lemma

*First, a lemma:*

If $-\disclapl U_j \leq 0$ then $\displaystyle \max_j U_j = \max\curlies{ U_0, U_M}$ (i.e. if $\overline \U$ is convex then its max is at one of its endpoints)

*Proof.*

Since $\overline \U$ satisfies $-\disclapl U_j \leq 0$, we can expand this to find that $\frac{2}{h^2} U_j \leq \frac{1}{h^2} (U_{j-1} + U_{j+1})$, i.e. $U_j \leq \frac{1}{2}U_{j-1} + \frac{1}{2}U_{j+1}$

Suppose $\overline \U$ has a maximum at an interior point $U_j$ with $j \in \curlies{ 1, ..., M-1}$

If $U_{j+1} < U_j$, then

$$
U_j \leq \frac{1}{2} U_{j-1} + \frac{1}{2} U_{j+1} < \frac{1}{2} U_j + \frac{1}{2} U_{j+1} \\
U_j < U_{j-1}
$$

But this contradicts our assumption that $U_j$ is a maximum point. We arrive at a similar result if we assume that $U_{j-1} < U_j$. Thus, if $U_j$ is an interior max then $U_{j-1} = U_j = U_{j+1}$. Extrapolating this logic, $U$ must be constant.

##### Stability

We denote the max norm as $\displaystyle \abs{\U}_S = \max_{x_j \in S} \abs{U_j}$ 

Suppose $\overline \U$ solves $-\disclapl U_j = f_j$ with $U_0 = u_0$ and $U_M = u_1$. Then

$$
\begin{align*}
\abs{U}_{\overline \Omega} \leq \max \curlies{ \abs{U_0}, \abs{U_M}} + c\abs{\disclapl \overline \U}_\Omega \\
\abs{U}_{\overline \Omega} \leq \max \curlies{ \abs{U_0}, \abs{U_M}} + c\abs{\mathbf f}_\Omega
\end{align*}
$$

*Proof.*

Let $\displaystyle w(x) = x - x^2 = \frac{1}{4} - \parens{x - \frac{1}{2}}^2$ and let $W_j = w(x_j)$

Then

$$
\begin{align*}
-\disclapl W_j &= -\frac{1}{h^2} (W_{j+1} - 2W_j + W_{j-1}) \\
&= -\frac{1}{h^2} (x_{j+1} - x_{j+1}^2 - 2x_j + 2x_j^2 + x_{j-1} - x_{j-1}^2) \\
&= 2 \text{ (using definition of $W_i$)}
\end{align*}
$$

Now set $U_j^\pm = \pm U_j - \frac{1}{2} \abs{\disclapl \U}_\Omega W_j$. Then

$$
\begin{align*}
-\disclapl U_j^\pm
&= \pm (-\disclapl U_j)
\mp \frac{1}{2} \abs{\disclapl \U}_\Omega \disclapl W_j \\
&= \pm (-\disclapl U_j)
\pm \abs{\disclapl \U}_\Omega \\
&\leq 0
\end{align*}
$$

Since $-\disclapl U_j^\pm \leq 0$ we can apply the previous lemma to find

$$
\begin{align*}
U_j^\pm &\leq \max \curlies{ U_0^\pm , U_M^\pm} \\
&= \max\curlies{ \pm U_0, \pm U_M} \tag{$W_0, W_M = 0$} \\
\abs{\U}_{\overline \Omega} &\leq \max\curlies{ \abs{U_0}, \abs{U_M}} + \frac{1}{2} [...]
\end{align*}
$$

Now we can show existence and uniqueness - if $A$ is invertible then $\U$ exists and is unique. $A$ is nonsingular if $A\U = \mathbf 0$ implies that $\U = \mathbf 0$

#### Consistency

Define the truncation error $\tau_j$ as the error when substituting the exact solution of a PDE into the discretization of the PDE. (in the case of our example, $\tau_j = \disclapl u(x_j) + f_j$)

A method is **consistent** if $h \to 0$ implies that $\abs{\tau_j} \to 0$ for any PDE

In this case, $\tau_j = \disclapl u(x_j) + f_j$

*Theorem.*

If $u$ is small enough, then

$$
\abs{\tau_j} \leq \frac{1}{12} h^2 \max_{x \in \overline \Omega} \abs{u^{(4)}(x)} \text{ for } j \in \curlies{ 1, ..., M-1}
$$

*Proof.*

By taylor's theorem, there is some $\zeta_1 \in [x_{j-1}, x_j]$ so that

$$
u(x_{j-1}) = u(x_j) - h u'(x_j) + \frac{1}{2} h^2 u''(x_j) - \frac{1}{3!} h^3 u'''(x_j) + \frac{1}{4!} h^4 u^{(4)}(\zeta_1)
$$

And similarly some $\zeta_2 \in [x_j, x_{j+1}]$ so that

$$
u(x_{j+1}) = u(x_j) + h u'(x_j) + \frac{1}{2} h^2 u''(x_j) + \frac{1}{3!} h^3 u'''(x_j) + \frac{1}{4!} h^4 u^{(4)}(\zeta_2)
$$

Adding these sums together, we have

$$
u(x_{j-1}) + u(x_{j+1}) = 2u(x_j) + h^2 u''(x_j) + \frac{1}{4!} h^2 (u^{(4)}(\zeta_1) + u^{(4)}(\zeta_2))
$$

Since $u^{(4)}$ is continuous on $[x_{j-1}, x_{j+1}]$, by the intermediate value thereom we know there is some $\zeta$ in this interval so that

$$
u^{(4)}(\zeta) = \frac{u^{(4)}(\zeta_1) + u^{(4)}(\zeta_2)}{2}
$$

And so

$$
\begin{align*}
u(x_{j-1}) + u(x_{j+1})
&= 2u(x_j) + h^2 u''(x_j) + \frac{1}{12} h^4 u^{(4)}(\zeta) \\
\frac{u(x_{j-1}) - 2u(x_j) + u(x_{j+1})}{h^2}
&= u''(x_j) + \frac{1}{12} h^2 u^{(4)}(\zeta) \\
\disclapl u(x_j) - u''(x_j) &= \frac{1}{12} h^2 u^{(4)}(\zeta) \\
\tau_j &= \frac{1}{12} h^2 u^{(4)}(\zeta) \\
\abs{\tau_j} &\leq \frac{1}{12} h^2 \max_{x \in \overline \Omega} \abs{u^{(4)}(x)}
\end{align*}
$$

*Corollary.*

As a result of this theorem, as $h \to 0$, $\abs{\tau_j} \to 0$, so our method is consistent.

#### Convergence/error estimate

*Theorem.*

Let $u$ be the solution to our poisson problem and $U$ be its finite difference discretization. Then

$$
\max_{1 \leq j \leq M-1} \abs{U_j - u(x_j)} \leq \frac{1}{96} h^2 \max_{x \in [0, 1]} \abs{u^{(4)}(x)}
$$

*Proof.*

Define the error $z_j = U_j - u(x_j)$

On interior points,

$$
\begin{align*}
-\disclapl z_j &= -\disclapl U_j + \disclapl u(x_j) \\
&= f_j + \disclapl u(x_j) \\
&= \tau_j
\end{align*}
$$

At boundary points, we have $z_0 = U_0 - u(x_0) = 0$ and $z_1 = U_m - u(x_m) = 0$

Using our stability estimate (lemma 4.2 [...])

$$
\begin{align*}
\max_{1 \leq j \leq M-1} \abs{z_j}
&\leq \max \curlies{ \abs{z_0}, \abs{z_m}} + \frac{1}{8} \max_{1 \leq j \leq M-1} \abs{\disclapl z_j} \\
&= \max\curlies{ 0, 0} + \frac{1}{8} \max_{1 \leq j \leq M-1} \abs{\tau_j} \\
&\leq \frac{1}{96} h^2 \max_{x \in [0, 1]}\abs{u^{(4)}(x)} \tag{stability}
\end{align*}
$$

## 2d Poisson problem

We define our 2d Poisson problem with Dirichlet boundary conditions on the unit square by

$$
\begin{align*}
-\Delta u &= f(x, y) &\text{ on } \Omega = (0, 1) \times (0, 1) \\
u &= 0 &\text{ on } T = \partial \Omega
\end{align*}
$$

To discretize, we turn it into an $M \times M$ grid from $(0, 0) = (x_0, y_0)$ to $(1, 1) = (x_M, y_M)$

We want an operator $\Delta_h$ that approximates $\displaystyle \Delta = \partials{}{x} + \partials{}{y}$

### 2d finite difference operators

Forward difference:

$$
\begin{align*}
\partial_1 U_{i, j} &= \frac{U_{i + 1, j} - U_{i, j}}{h} \\
\partial_2 U_{i, j} &= \frac{U_{i, j + 1} - U_{i, j}}{h}
\end{align*}
$$

Backward difference

$$
\begin{align*}
\overline \partial_1 U_{i, j} &= \frac{U_{i, j} - U_{i - 1, j}}{h} \\
\overline \partial_2 U_{i, j} &= \frac{U_{i, j} - U_{i, j - 1}}{h} \\
\end{align*}
$$

Combining:

$$
\begin{align*}
\Delta_h U_{i, j} &= \disclapl[1] U_{i, j} + \disclapl[2] U_{i, j} \\
&= \frac{U_{i+1, j} + U_{i, j+1} - 4U_{i, j} + U_{i-1, j} + U_{i, j-1}}{h^2} \\
\end{align*}
$$

So we can discretize our PDE as

$$
\begin{align*}
-\Delta_h U_{i, j} &= f(x_i, y_j) \text{ for } (x_i, y_j) \in \Omega \\
U_{i, j} &= 0 \text{ for } (x_i, y_j) \in T
\end{align*}
$$

And solve it as a linear system $A \U = \f$ where

$$
\U = \begin{bmatrix}
[...]
\end{bmatrix} \\
\f = \begin{bmatrix}
[...]
\end{bmatrix}
$$

and

$$
A = \begin{bmatrix}
[...]
\end{bmatrix}
$$
