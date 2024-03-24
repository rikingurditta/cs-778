# Hyperbolic problems

$$
\newcommand{\x}{\mathbf x}
\newcommand{\y}{\mathbf y}
\newcommand{\b}{\mathbf b}
\newcommand{\e}{\mathbf e}
\newcommand{\f}{\mathbf f}
\newcommand{\j}{\mathbf j}
\newcommand{\n}{\mathbf n}
\newcommand{\u}{\mathbf u}
\newcommand{\v}{\mathbf v}
\newcommand{\w}{\mathbf w}
\newcommand{\U}{\mathbf U}
\newcommand{\I}{\mathcal I}
\newcommand{\bzero}{\mathbf 0}
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
\DeclareMathOperator{\ess}{ess}
\DeclareMathOperator{\supp}{supp}
$$

This section will be based on several papers, not a single book.

## Hyperbolic problems

We want to solve

$$
\partial_t u + \mathbf b \cdot \nabla u = 0 \text{ on } \Omega = (0, 1)^2
$$

we may need discontinuous functions to solve this, so we cannot use the finite element method.

### Example

Take $\b = (1, 2)$, $u(\x, 0) = 0$ (initial condition), and $u(\bzero, t) = 1$ (boundary condition).

Then the solution is a PDE where the "1" region propagates up and right.

The continuous Galerkin finite element method doesn't work here!

## Finite volume and discontinuous Galerkin methods for hyperbolic conservation laws

Consider a 1D domain $x \in \Omega = (x_1, x_2)$ and denote the density of a fluid along this domain by $\rho(x, t)$. Then the total mass is

$$
M(t) = \int_\Omega \rho(x, t) \d x
$$

Let $v(x, t)$ denote the local velocity of the fluid.

Then the **first integral form of mass conservation** is

$$
\dbyd{}{t}M(t) = \dbyd{}{t} \int_\Omega \rho(x, t) \d x = \rho(x_1, t) v(x_1, t) - \rho(x_2, t)v(x_2, t)
$$

(this has a positive $x_1$ term and negative $x_2$ term so that the normals agree, in agreement with the higher dimensional case, $-\int_{\partial \Omega} \rho \v \cdot \n \d s$)

Define a temporal domain $[t_1, t_2]$. Integrating our mass conservation equation over this interval, we find the **second integral form of mass conservation**:

$$
\int_\Omega \rho(x, t_2) \d x - \int_\Omega \rho(x, t_1) \d x = \int_{t_1}^{t_2} \parens{\rho(x_1, t) v(x_1, t) - \rho(x_2, t)v(x_2, t)} \d t
$$

If $\rho$ and $v$ are differentiable, then using the facts:

$$
\rho(x, t_2) - \rho(x, t_1) = \int_{t_1}^{t_2} = \partials{}{t} \rho(x, t) \d t \\
\rho(x_2, t) v(x_2, t) - \rho(x_1, t) v(x_1, t) = \int_\Omega \partials{}{x} \rho(x, t) v(x, t) \d x
$$

we can rewrite the second integral form as

$$
\int_\Omega \int_{t_1}^{t_2} \partials{}{t} \rho(x, t) \d t \d x = -\int_{t_1}^{t_2} \int_\Omega \partials{}{x} (\rho(x, t) v(x, t)) \d x \d t \\
\int_\Omega \int_{t_1}^{t_2} \parens{\partials{}{t} \rho(x, t) + \partials{}{x} (\rho(x, t) v(x, t))} \d x \d t = 0
$$

This equation must hold over any section $\Omega = (x_1, x_2)$ and any time interval $[t_1, t_2]$, so we know that

$$
\partials{}{t} \rho + \partials{}{t} (\rho v) = 0
$$

If the velocity is known, or if $v$ is a function of $\rho$, i.e. $\rho v = f(\rho)$, then we have

$$
\partial_t \rho + \partial_x f(\rho) = 0
$$

And so we find the general form of a **scalar hyperbolic conservation law**:

$$
\partial_t u + \partial_x f(u) = 0
$$

$f$ is called the **flux function**

### Examples

- **advection equation**: $f(u) = au$ where $a$ is a constant
  - simplest linear hyperbolic PDE
  - whole field just moves around (advect)
- **burger??? equation 🍔**: $\displaystyle f(u) = \frac{1}{2} u^2$
  - simplest nonlinear hyperbolic PDE

### Jacobian

The eigenvalues of the Jacobian of $f(u)$, i.e. $\displaystyle \partials{f}{u}$​, play an important role in numerical methods.

The eigenvalues are always real by definition, because a problem with complex eigenvalues is an elliptic problem.

## Model problem

$$
\begin{cases}
\partial_t u + \partial_x f(u) = 0 & \text{on } x \in \Omega = (0, 1) \text{ and } t \in I = [0, T] \\
u(x, 0) = u_0(x) & \text{on } x \in \Omega
\end{cases}
$$

## Formulating finite volume/discontinuous Galerkin

### Discretization

We define a grid $0 = x_0, ..., x_N = 1$ and elements $K_j = [x_{j-1}, x_j]$, just like in the finite element method.

Our finite dimensional space of discontinuous functions will be

$$
V_h^d = \curlies{v_h \in L^2(\Omega) : v_h \big \vert_{K_j} \in P^k(K_j)}
$$

(the $^d$ is for discontininuous)

i.e. each segment of the function is a polynomial of degree $k$ just like in the finite element method, but there is no requirement that $v$ is continuous. When $k = 0$ we call this the **finite volume method**, otherwise it is the **discontinuous Galerkin method**.

For $v_h \in V_h^d$, we denote $v_h$ restricted to an element $K_j$ by $v_h^j = v_h \big \vert_{K_j}$

### Weak formulation

Let $v$ be an arbitrary smooth test function. We can derive a weak formulation by multiplying our model problem by $v$ and integrating over *each element individually*, rather than the whole domain as in the finite element method:

$$
\begin{cases}
\displaystyle \int_{K_j} v(x) \partial_t u(x, t) \d x + \int_{K_j} v(x) \partial_x f(u(x, t)) \d x = 0 &\text{for } j = 1, ..., N \text{ and } t \in I \\
\displaystyle \int_{K_j} v(x) u(x, 0) \d x = \int_{K_j} v(x) u_0(x) \d x
\end{cases}
$$

We can integrate by parts:

$$
\begin{align*}
&\int_{K_j} v(x) \partial_t u(x, t) \d x - \int_{K_j} f(u(x, t)) \partial_x v(x) \d x \\
&\quad+ f(u(x_j, t) v(x_j) - f(u(x_{j-1}, t))v(x_{j-1}) = 0
\end{align*}
$$

We can now weaken our problem by replacing $u$ with $u_h \in V_h^d$:

$$
\begin{align*}
&\int_{K_j} v_h^j(x) \partial_t u_h^j(x, t) \d x - \int_{K_j} f(u_h^j(x, t)) \partial_x v_h^j(x) \d x \\
&\quad+ f(u_h^j(x_j, t)) v_h^j(x_j) - f(u_h^j(x_{j-1}, t))v_h^j(x_{j-1}) = 0
\end{align*}
$$

The issue here is that for different elements, we will find different solutions for $u_h^j(x_j, t)$ and $u_h^{j+1}(x_j, t)$. so we will have different evaluations of $f$ at the same point. So how do we define $f$ at $x_j$?

We replace $f$ at each $x_j$ with a "numerical flux" $\hat f$ that depends on both $u_h^j(x_j, t)$ and $u_h^{j+1}(x_j, t)$:

$$
f(u_h(x, t)) \longrightarrow \hat f_j = \hat f(u_h^j(x_j, t), u_h^{j+1}(x_j, t))
$$

We'll define $\hat f$ later, but for now we assume that $\hat f$ is consistent between elements - i.e. if $u$ is smooth, then $\hat f(u, u) = f(u)$.

Now we can rewrite our formulation for each $K_j$ as

$$
\begin{cases}
\displaystyle \int_{K_j} v_h^j(x) \partial_t u_h^j(x, t) \d x - \int_{K_j} f(u_h^j(x, t)) \partial_x v_h^j(x) \d x \\
\displaystyle \quad+ \hat f_j v_h^j(x_j) - \hat f_{j-1} v_h^j(x_{j-1}) = 0 & \text{for } t \geq 0 \\
\displaystyle \int_{K_j} v_h^j u_h^j(x, 0) \d x = \int_{K_j} v_h^j u_0(x) \d x \\ & \text{for } t = 0
\end{cases}
$$

This gives us a piecewise solution which we can write as a sum of functions whose supports are only their own elements:

$$
u_h(x, t) = \sum_{i=1}^N \delta_{i,j} \hat u_h^i (t)
$$

where

$$
\hat u_h^i = \begin{cases}
u_h^j & \text{on } K_j \\
0 & \text{otherwise}
\end{cases}
$$

This means that $u_h(x, t) \big \vert_{K_j} = u_h^j(x, t) = \hat u_h^j(t)$

Note that this is basically the same discretization as the finite element method, but using $\phi_j = \delta_{i,j}$ (which are discontinuous) as our basis functions.

## 2022-03-21

The finite volume method is an approximation tot he first integral form of the conservation law where $\hat u_1^j(t)$ is an approximation  to $\overline u^j(t)$, the mean of $u(x, t)$ in element $K_j$

## Time discretization

Let $0 = t_0 < t_1 < ... < t_M = T$ and $\Delta t_n = t_n - t_{n-1}$

We can apply any ODE time integration method we want. However, if our spatial discretization is only first-order accurate, there is no use in using a time stepping method with higher than first order accuracy.

Using Euler's method:

$$
(\hat u_1^j)^n = (\hat u_1^j)^{n-1} + \frac{\Delta t_n}{h_j} \parens{\hat f_{j-1}^{n-1} - \hat f_j^{n-1}}
$$

Our fully discrete scheme is an approximation to the second integral form of conservation

$$
\begin{align*}
\int_\Omega u(x, T) \d x - \int_\Omega u(x, 0) \d x - \int_0^T f(u(0, t)) \d t + \int_0^T f(u(1, t)) \d t &= 0\\
\sum_{j=1}^N \sum_{n=1}^M \brackets{\int_{K_j} u(x, t^n) \d x - \int_{K_j} u(x, t_{n-1}) \d x - \int_{t_{n-1}}^{t_n} f(u(x_{j-1}, t)) \d t + \int_{t_{n-1}}^{t_n} f(u(x_j, t)) \d t } &= 0
\end{align*}
$$

Define

$$
\overline f_j^{n-1} = \frac{1}{\Delta t_n} \int_{t_{n-1}}^{t_n} f(u(x_j, t)) \d t
$$

Then

$$
\begin{align*}
\underbrace{\int_{K_j} u(x, t^n) \d x}_{h_j \hat u_j^n}
&=
- \underbrace{\int_{K_j} u(x, t_{n-1}) \d x}_{h_j \hat u_j^{n-1}}
- \underbrace{\int_{t_{n-1}}^{t_n} f(u(x_{j-1}, t)) \d t}_{\Delta t_n \overline f_{j-1}^{n-1}}
+ \underbrace{\int_{t_{n-1}}^{t_n} f(u(x_j, t)) \d t}_{\Delta t_n \overline f_j^{n-1}} \\
\overline u_j^n &= \overline u_j^{n-1} + \frac{\Delta t_n}{h_j}\parens{\overline f_{j-1}^{n-1} + \overline f_{j-1}^n}
\end{align*}
$$

So this method is fully explicit!

### Courant-Friedrich-Lewy (CFL) condition

From now on, let's stick with uniform discretization, i.e. for all $j$ and $n$, $h_j = h$ and $\Delta t_n = \Delta t$

The **CFL condition** is a constraint on our time step size. Suppose $\lambda(u)$ returns the eigenvalue of $u$, then the CFL condition says that we require

$$
\max_j \abs{ \frac{\Delta t}{h}\lambda\parens{(u_1^j)^{n-1}} } \leq c \text{ where } 0 < c < 1
$$

## Discontinuous solutions

The PDE $\partial_t u + \partial_x f(u) = 0$ is not valid at points where $u(x, t)$ is discontinuous. Let's derive an expression that *is* true wehn $u$ is discontinuous.

For any $(a, b) \subseteq \Omega$, we know

$$
\dbyd{}{t} \int_a^b u(x, t) \d x = f(u(a, t)) - f(u(b, t))
$$

Let $x_d(t)$ denote the point where $u$ is discontinuous. Suppose $x_d(t) \in (x_L, x_R) \subseteq \Omega$, then

$$
\dbyd{}{t} \int_{x_L}^{x_d(t)} u(x, t) \d x + \dbyd{}{t} \int_{x_d(t)}^{x_R} u(x, t) \d x = f(u(x_L, t)) - f(u(x_R, t))
$$

Applying [Leibniz's rule](https://en.wikipedia.org/wiki/Leibniz_integral_rule) for differentiating an integral, we find

$$
[u(x_d^-, t) - u(x_d^+, t)] \dbyd{x}{t} + \int_{x_L}^{x_d(t)} \partial_t u(x, t) \d x + \int_{x_d(t)}^{x_R} \partial_t u(x, t) \d x = f(u(x_L, t)) - f(u(x_R, t))
$$

We refer to $\displaystyle \dbyd{x}{t}$ as the **shock speed** $S$.

This holds for all $(x_L, x_R) \subseteq \Omega$ that contain $x_d(t)$. So if we shrink the domain on both sides $x_L \rightarrow x_d^-$ and $x_d^+ \leftarrow x_R$​, then the integral terms go to 0, and we get

$$
[u(x_L, t) - u(x_R, t)] S = f(u(x_L, t)) - f(u(x_R, t)) \\
S = \frac{f(u(x_L, t)) - f(u(x_R, t))}{u(x_L, t) - u(x_R, t)}
$$

This is the **Rankine-Hugoniot condition**.

### Example: Burgers' equation

Consider Burgers' equation:

$$
\partial_t u + \partial_x \parens{\frac{1}{2} u^2} = 0
$$

When $u$ is discontinuous, by the Rankine Hugoniot condition, we have

$$
S = \frac{u_L^2/2 - u_R^2/2}{u_L - u_R} = \frac{1}{2} (u_L + u_R)
$$

However, consider the PDE we get by multiplying Burgers' equation by $2u$:

$$
2u\, \partial_t u + 2u\, \partial_x \parens{\frac{1}{2} u^2} = 0
$$

This will not satisfy the RH condition if we incorporate the new factors into the PDE. i.e. if we continue to say

$$
\partial_t ()
$$


