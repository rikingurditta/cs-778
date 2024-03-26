# I was REALLY late today and I'm not sure what's going on

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

### Non-uniqueness example

$$
\begin{cases}
\partial_t u + \partial_x (\frac{1}{2}u^2) = 0 & x \in \R, t > 0 \\
u(x, 0) = 1 & x < 0 \\
u(x,0) = 2 & x > 0
\end{cases}
$$

The weak formulation  of this problem could have infinite solutions. But there is only one weak solution that satisfies the second law of thermodynamics - entropy must be non-decreasing.

## Conservative schemes

A scheme is **conservative** if it has the form

$$
u_j^n = u_k^{n-1} + \frac{\Delta t}{h} \parens{ \hat f_{j-1}^{n-1} + \hat f_j^{n-1} }
$$

### Lax and Wendroff theorem

A conservative scheme, if convergent, will converge to a weak solution of the conservation law

This means that the finite volume method will always converge to a weak solution, but not necessarily the entropy solution

We need more restrictions

A very severe requirement to lead us to the entropy solution is to require our conservative scheme to be *monotone*.

### Monotonicity

Consider the problem

$$
\partial_t u + \partial_x f(u) = 0 \text{ where } u(x, 0) = u_0(x) \\
\partial_t v + \partial_x f(v) = 0 \text{ where } v(x, 0) = v_0(x)
$$

Then if $v_0(x) \geq u_0(x)$ for all $x$, then $v(x, t) \geq u(x, t)$ for all $t > 0$ as well.

### Monotone numerical method

A scheme

$$
u_i^n = u_i^{n-1} + \frac{\Delta t}{h} (\hat f(u_{i-1}^{n-1}, u_i^{n-1}) - \hat f(u_i^{n-1}, u_{i+1}^{n-1}))
$$

is monotone if for solutions $u, v$, we know

$$
v_i^{n-1} \geq u_i^{n-1} \text{ implies that } v_i^n \geq u_i^n
$$

An equivalent statement that is easier to deal with is, the scheme is monotone if

$$
\partials{}{u_i^{n-1}} \hat f(u_i^{n-1}, u_{i+1}^{n-1}) \geq 0 \text{ and } \partials{}{u_{i+1}^{n-1}} \hat f(u_i^{n-1}, u_{i+1}^{n-1}) \leq 0
$$

### Theorem

Given $\curlies{u_i^{n-1}}$, if $\curlies{u_i^n}$ is found using a monotone method, then we know

$$
\max_i u_i^n \leq \max_i u_i^{n-1} \text{ and } \min_i u_i^n \geq \min_i u_i^{n-1}
$$
*Proof.*

Define $$v_i^{n-1} = \max_j$$. Then we know that for each $i, j$, $v_i^{n-1} = v_j^{n-1}$. Since $\hat f$ is consistent, we know $\hat f(v_i^{n-1}, v_j^{n-1}) = f(v_i^{n-1})$, so

$$
\begin{align*}
v_i^n &= v_i^{n-1} + \frac{\Delta t}{h} (\hat f(v_{i-1}^{n-1}, v_i^{n-1}) - \hat f(v_i^{n-1}, v_{i+1}^{n-1})) \\
&= v_i^{n-1} + \frac{\Delta t}{h} (f(v_i^{n-1}) - f(v_i^{n-1})) \\
&= v_i^{n-1}
\end{align*}
$$

Since $v_i^{n-1} \geq u_i^{n-1}$ and our scheme is monotone, we know that $v_i^n \geq u_i^n$ for all $i$, including the maximum. So,

$$
\max_i u_i^n \leq v_i^n = v_i^{n-1} = \max_i u_i^{n-1}
$$

A similar proof can show the relation between the minima.

Our theorem thus guarantees that if we use a monotone scheme, the max and min of any time step are bounded by the initial conditions:

$$
\max_i u_i^n \leq \max_i u_i^{n-1} \leq \cdots \leq \max_i u_i^0 \\
\min_i u_i^n \geq \min_i u_i^{n-1} \geq \cdots \geq \min_i u_i^0 \\
$$

### Main theorem for finite volume methods for hyperbolic conservation laws

The numerical solution computed with a consistent and monotone method with fixed $$\frac{\Delta t}{h}$$ converges to the entropy solution as $\Delta t \to 0$.

### Godanov theorem

A monotone scheme is as most first-order accurate.

> "This is quite a depressing result."
> \- Sander Rhebergen, 2024

## Numerical flux

Our finite volume method is

$$
(\hat u_i^j)^n = (\hat u_i^j)^{n-1} + \frac{\Delta t}{h} (\hat f_{j-1}^{n-1} - \hat f_j^{n-1})
$$

We need to define $\hat f_j^n$ to be consistent, nondecreasing in its first argument, and nonincreasing in its second argument (i.e. monotone).

> "There is no 'best' numerical flux."

### Local Lax-Friedrich's flux (LLF):

$$
\hat f(a, b) = \frac{1}{2}(f(a) + f(b)) - \frac{1}{2} \beta (b - a) \text{ where } \beta = \max \abs{\lambda(S)}
$$

(recall that $S$ is the shock speed)

This is a good scheme! It is consistent and monotone.

### Bad scheme

Never use this scheme:

$$
\hat f(a, b) = \frac{1}{2} (f(a) + f(b))
$$

### Boundary conditions

Boundary conditions are imposed through the numerical flux, i.e. we pass the boundary values into the flux function at the appropriate places.
