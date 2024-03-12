# $n$-dimensional problems

$$
\newcommand{\x}{\mathbf x}
\newcommand{\y}{\mathbf y}
\newcommand{\e}{\mathbf e}
\newcommand{\f}{\mathbf f}
\newcommand{\j}{\mathbf j}
\newcommand{\n}{\mathbf n}
\newcommand{\u}{\mathbf u}
\newcommand{\v}{\mathbf v}
\newcommand{\w}{\mathbf w}
\newcommand{\U}{\mathbf U}
\newcommand{\I}{\mathcal I}
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

Assume from here on that our domain $\Omega \subseteq \R^n$ is open, bounded, and Lipschitz, i.e. $\partial \Omega$ is locally the graph of a Lipschitz function.

## Useful results

### Interpolation error

Let $\Omega \subseteq \R^d$ where $d \in \curlies{1, 2, 3}$.

$$
\norm{u - \I_h u}_{H^1(\Omega)} \leq h^{m-1} \abs{u}_{H^m(\Omega)} \text{ for all } u \in H^m(\Omega)
$$

where $\I_h u \in V_h$ is the interpolant of $u$

### Friedrich's inequality

Define the **mean** of an integrable function $g$ as

$$
\overline g = \frac{1}{\abs \Omega} \int_\Omega g(x) \d x
$$

where $\abs \Omega$ is the measure (size) of $\Omega$

Let $\Omega$ be bounded and Lipschitz (its boundary is locally the graph of a Lipschitz function). Then

$$
\norm{u = \overline u}_{W_p^1(\Omega)} \leq c \abs{u}_{W_p^1(Omega)} \text{ for all } u \in W_p^1(\Omega)
$$

This theorem helps us prove coercivity

### Generalizations of calculus

#### Divergence theorem

Let $\u \in W_1^1(\Omega)^n$. Then the divergence theorem from calculus holds:

$$
\int_\Omega \nabla \cdot \u \d x = \int_{\partial \Omega} \u \cdot \n \d s
$$

Since we assume that our domain is bounded, by proposition (1.4.2) we know $W_q^1(\Omega) \subseteq W_1^1(\Omega)$ for $1 \leq q \leq \infty$, so this holds for $\u \in W_q^1(\Omega)$ for any $q$.

#### Integration by parts

Let $\v, \w \in H^1(\Omega)$ and $i \in \curlies{1, ..., n}$. Then

$$
\int_\Omega \partials{v}{x_i} w \d x = -\int_\Omega v \partials{w}{x^i} \d x + \int_{\partial \Omega} v w \n_i \d s
$$

where $\n_i$ is the $i^\text{th}$ component of $\n$.

(i.e., $\u = v w \e_i$)

#### Green's first identity for Sobolev functions

Let $u \in H^2(\Omega)$ and $v \in H^1(\Omega)$, then
$$
\int_\Omega (-\Delta u) v \d x = \int_\Omega \nabla u \cdot \nabla v \d x - \int_{\partial \Omega} (\nabla u \cdot \n) v \d s
$$

## Example problem: Poisson equation

Let $\Gamma_D \subseteq \partial \Omega$ be the region where we have Dirichlet boundary conditions, and $\Gamma_N = \partial \Omega \setminus \Gamma_D$ be where we have Neumann boundary conditions. Let $\n$ be the normal vector to $\partial \Omega$. Then Poisson's equation is

$$
\begin{cases}
-\Delta^2 u = f & \text{on } \Omega \\
u = 0 & \text{on } \Gamma_D \\
\nabla u \cdot \mathbf n = 0 & \text{on } \Gamma_N
\end{cases}
$$

Define our set of functions

$$
V = \curlies{ v \in H^1(\Omega) : v \big\vert_{\Gamma_D} = 0 } \subseteq H^1(\Omega) = W_2^1(\Omega)
$$

We cannot interpret $v \big\vert_{\Gamma_D} = 0$ in a pointwise sense, i.e. it might have singularities, since Sobolev's inequality does not hold for $n \geq 2, k = 1, p = 2$. However, we can use the trace inequality to see:

$$
\norm{v}_{L^2(\Omega)} \leq c \sqrt{\norm{v}_{L^2(\Omega)}}\sqrt{\norm{v}_{H^1(\Omega)}} \text{ for all } v \in H^1(\Omega)
$$

So we can interpret $v \big \vert_{\Gamma_D}$ as a function in $L^2(\partial \Omega)$, i.e. it may be infinite at points but it is integrable.

### Weak formulation

Using our generalizations of one dimensional calculus to $n$-dimensional Sobolev functions, we can derive a weak formulation the same way we did for $1$-dimensional problems:

$$
\begin{align*}
\int_\Omega f v \d x &= \int_\Omega (-\Delta u) v \d x \\
&= \int_\Omega \nabla u \cdot \nabla v \d x - \int_{\partial \Omega} (\nabla u \cdot \n) v \d s \\
&= \int_\Omega \nabla u \cdot \nabla v \d x - \int_{\Gamma_D} (\nabla u \cdot \n) \underbrace{v}_{= 0} \d s  - \int_{\Gamma_N} \underbrace{(\nabla u \cdot \n)}_{= 0} v \d s \\
&= \int_\Omega \nabla u \cdot \nabla v \d x
\end{align*}
$$

Thus if we define

$$
a(u, v) = \int_\Omega \nabla u \cdot \nabla v \d x \\
F(v) = \int_\Omega f v \d x
$$

Then we can rewrite the weak formulation of our problem as:

$$
\text{find } u \in V \text{ so that } a(u, v) = F(v) \text{ for all } v \in V
$$

We can investigate the well-posedness of this problem similarly to how we did in one dimension - using the Riesz representation theorem and the Lax-Milgram theorem.

### Well-posedness of only Neumann boundary conditions

$$
\begin{cases}
-\Delta^2 u = f & \text{on } \Omega \\
\nabla u \cdot \mathbf n = 0 & \text{on } \Gamma_N = \partial \Omega
\end{cases}
$$

We cannot solve this the same way we solve the mixed problem, i.e. just find $u$ that fits

$$
a(u, v) = F(v)
$$

This is because $a(u, v)$ only depends on $\nabla u$, so $u + c$ for any constant $c$ would also be a solution, so $u$ is only unique up to a constant. To deal with this, we impose that $\overline u = 0$, i.e.

$$
V = \curlies{v \in H^1(\Omega) : \overline v = \frac{1}{\abs{\Omega}} \int_\Omega v \d x = 0}
$$

Suppose $v \in H^1(\Omega)$, then $v - \overline v \in V$:

$$
\begin{align*}
\int_\Omega (v - \overline v) \d x &= \int_\Omega v \d x - \int_\Omega \frac{1}{\abs{\Omega}} \parens{\int_\Omega v \d x} \d x \\
&= \int_\Omega v \d x - \frac{1}{\abs{\Omega}} \parens{\int_\Omega v \d x} \parens{\int_\Omega \d x} \\
&= \int_\Omega v \d x - \frac{\abs{\Omega}}{\abs{\Omega}} \int_\Omega v \d x \\
&= 0
\end{align*}
$$

#### Well-posedness

$a$ is symmetric:

$$
a(u, v) = \int_\Omega \nabla u \cdot \nabla v \d x = \int_\Omega \nabla v \cdot \nabla u \d x = a(v, u)
$$

$a$ is bounded:

$$
\begin{align*}
\abs{a(u, v)} &= \abs{\int_\Omega \nabla u \cdot \nabla v \d x} \\
&\leq \int_\Omega \abs{\nabla u \cdot \nabla v} \d x \\
&= \int_\Omega \abs{\angles{\nabla u, \nabla v}} \d x \\
&\leq \int_\Omega \norm{\nabla u} \norm{\nabla v} \d x \\
&= \int_\Omega \abs{\nabla u} \abs{\nabla v} \d x \tag{to make notation match 1d} \\
&= \angles{\abs{\nabla u}, \abs{\nabla v}}_{L^2(\Omega)} \\
&\leq \norm{\nabla u}_{L^2(\Omega)} \norm{\nabla v}_{L^2(\Omega)} \\
&\leq \norm{\nabla u}_{H^1(\Omega)} \norm{\nabla v}_{H^1(\Omega)}
\end{align*}
$$
