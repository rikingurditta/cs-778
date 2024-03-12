# Reformulating boundary value problems

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
$$

## Boundary value problems

Let $C^2_D(\overline \Omega)$ be the space of $C^2$ functions on $\overline \Omega$ that are $0$ at $0$:

$$
C^2_D(\overline \Omega) = \curlies{ u \in C^2(\overline \Omega : u(0) = 0}
$$

Consider the boundary value problem

$$
\begin{cases}
-u'' = f & \text{in } \overline \Omega = (0, 1) \\
u(0) = 0 \\
u'(1) = 0
\end{cases}
$$

If we assume $f \in C^0(\overline \Omega)$, then $u \in C^2_D(\overline \Omega)$.

Consider some function $v \in C^2_D(\Domain)$, then we can equivalently solve $-u''v = fv$. Integrating both sides,

$$
\begin{align*}
-\int_0^1 u''(x) v(x) \d x &= \int_0^1 f(x) v(x) \d x \\
-\int_0^1 u'(x) v'(x) \d x - (u'(1) v(1) - u'(0) v(0)) &= \int_0^1 f(x) v(x) \d x
\end{align*}
$$

And thus our boundary value problem is equivalently to find $u \in C^2_D(\Domain)$ such that

$$
\int_0^1 u'(x)v'(x) \d x = \int_0^1 f(x) v(x) \d x
$$

for all $v \in C^2_D(\Domain)$

### Problems

In practice, data (i.e. $f$) is not always $C^0(\Domain)$, so $-u'' = f$ is not continuous, so $u$ cannot be $C^2$.

Thus requiring $u \in C^2(\Domain)$ is too restrictive

## $L^2$ functions

We define the **space of $L^2$ functions on $\Omega$**:

$$
L^2(\Omega) = \curlies{  u : \Omega \to \R : \int_\Omega u(x)^2 \d x < \infty \ }
$$

And define a subset $V$ as

$$
V = \curlies{  u \in L^2(\Omega) : \int_\Omega u'(x)^2 < \infty, u(0) = 0 }
$$

i.e.

$$
V = \curlies{  u \in L^2(\Omega) : u' \in L^2(\Omega), u(0) = 0}
$$

We define the **weak formulation of our PDE** as

$$
\text{find } u \in V \text{ so that } \int_0^1 u'v' \d x = \int_0^1 f v \d x \text{ for all } v \in V
$$

This is much less restrictive than looking for $u \in C^2_D(\Domain)$, so it allows us to solve a much larger class of problems (e.g. $f$ is discontinuous)

## Terminology

- a function that satisfies our original BVP (must be $C^2$) is called a **classical solution**
- a function that satisfies the weak formulation is called a **weak solution**
- $v \in V$ is called a **test function**
- $u \in V$ is called a **trial function** - this is what we solve for
- the Dirichlet boundary conditioin $u(0) = 0$ is called **essential** because it appears explicitly in the definition of $V$
- the Neumann boundary condition $u'(1) = 0$ is called **natural** because it is incorporated implicitly in the weak formulation

### Remarks

1. if a weak solution is $C^2_D(\Domain)$, then it is also a classical solution
2. if $u \in V$ is a weak solution, it may not be smooth enough to be a classical solution
   - e.g. if $f$ is discontinuous

#### Proof of 1

(Theorem 0-14)

Suppose $f \in C^0(\Domain)$, $u \in C^2(\Domain)$, and they satisfy

$$
u \in V,\ \ \int_0^1 u'v' \d x = \int_0^1 fv \d x
$$

Then $u$ solves

$$
\begin{cases}
-u'' = f & \text{on } \Omega = (0, 1) \\
u(0) = 0, u'(1) = 1
\end{cases}
$$

*Proof.*

If our condition holds for all $v \in V$, it also holds for $v \in C^1(\Domain)$ since $C^1(\Domain) \subseteq V$.

Then since all of our functions are differentiable enough, we can use integration by parts:

$$
\begin{align*}
\int_0^1 fv \d x &= \int_0^1 u' v' \d x \\
&= \int_0^1 (-u'') v \d x + u'(1) v(1) - u'(0) v(0) \\
&= \int_0^1 (-u'') v \d x + u'(1) v(1) - u'(0) (0) \\
\Longrightarrow \int_0^1 (f + u'') v \d x &= 0
\end{align*}
$$

Let $f + u'' = w \ ( \in C^0(\Domain) )$. Suppose $w \neq 0$.

Then there must be some interval $(x_0, x_1) \subseteq [0, 1]$ so that $w$ is either only positive or only negative on this interval.

Since we assumed that $u$ satisfies our weak formulation, it must hold for all $v \in V$, including

$$
v(x) = \begin{cases}
(x - x_0)^2 (x - x_1)^2 & x \in [x_0, x_1] \\
0 & \text{otherwise}
\end{cases}
$$

Note that $v(x) > 0$ for $x \in (x_0, x_1)$. Then,

$$
\begin{align*}
\int_0^1 w(x) v(x) \d x &= \int_{x_0}^{x_1} w(x) v(x) \d x \tag{$v$ is 0 otherwise}
\end{align*}
$$

Since $v$ is positive on the interval being integrated over, $wv$ is either positive or negative on this interval depending on $w$, so the integral is not 0. We have reached a contradiction, so we can conclude that $w = 0$, and thus $f = -u''$.

Since $u \in V$, it satisfies $u(0) = 0$.

Going back to our integration by parts, we have found $u$ such that

$$
\begin{align*}
\int_0^1 f v \d x &= \int_0^1 u' v' \d x + u'(1) v(1) \\
\int_0^1 wv \d x &= u'(1) v(1) \\
0 &= u'(1) v(1)
\end{align*}
$$

This must hold for all $v \in V$, including $v(x) = x$. So in this case, $v(1) = 1$ and $u'(1)v(1) = 0$, so $u'(1) = 0$​.

And so for all $v \in V$, we have

$$
\int_0^1 fv \d x = \int_0^1 u' v' \d x
$$


#### Proof of 2

(Theorem 0-14)
