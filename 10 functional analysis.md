# Functional analysis

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
\DeclareMathOperator{\ess}{ess}
\DeclareMathOperator{\supp}{supp}
$$

## Lebesgue norms

Note that we are using Lebesgue integration throughout.

We have the $L^2$ norm

$$
\norm{f}_{L^2(\Omega)} = \parens{\int_\Omega f(x)^2 \d x}^{1/2}
$$

This generalizes to the $L^p$ norm:

For $1 \leq p < \infty$

$$
\norm f_{L^p(\Omega)} = \parens{\int_\Omega f(x)^p \d x}^{1/p}
$$

For $p = \infty$,

$$
\norm f_{L^\infty(\Omega)} = \ess \sup \curlies{\abs{f(x)} : x \in \Omega}
$$

Where we define the **essential supremum** by, if $x \in \Omega$, $x = \ess \sup f(x)$ if the set of points $\curlies{y \in \Omega : f(y) > f(x)}$ has measure 0.

## Lebesgue spaces

Lebesgue spaces are

$$
L^p(\Omega) = \curlies{ f : \norm{f}_{L^p} < \infty }
$$

Lebesgue spaces don't care about trivial differences - differences on measure 0:

If we define functions on $\Omega = [-1, 1]$:

$$
\begin{align*}
f(x) = \begin{cases}
1 & x \geq 0 \\ 0 & x < 0
\end{cases} \\
g(x) = \begin{cases}
1 & x > 0 \\ 0 & x \leq 0
\end{cases}
\end{align*}
$$

Then $f(0) \neq g(0)$, but $\curlies{0}$ has measure 0, so the lebesgue integral does not care about it:

$$
\norm{f - g}_{L^p(\Omega)} = \parens{\int_{-1}^1 \abs{f(x) - g(x)}^{p} \d x}^{1/p} = 0
$$

## Useful inequalities

### Minkowski's inequality:

For $1 \leq p \leq \infty$ and $f, g \in L^p(\Omega)$, we have

$$
\norm{f + g}_{L^p(\Omega)} \leq \norm{f}_{L^p(\Omega)} + \norm{g}_{L^p(\Omega)}
$$

This is the triangle inequality!

### Hölder's inequality

For $1 \leq p, q \leq \infty$ so that $1 = 1/p + 1/q$, and $f \in L^p(\Omega)$ and $g \in L^q(\Omega)$, we have

$$
fg \in L^1(\Omega)
$$

and

$$
\norm{fg}_{L^1(\Omega)} \leq \norm{f}_{L^p(\Omega)} + \norm{g}_{L^q(\Omega)}
$$

#### Schwarz inequality

This is Hölder's inequality with $p = q = 2$:

$$
\norm{fg}_{L^1(\Omega)} = \int_\Omega \abs{f(x)g(x)} \d x \leq \norm{f}_{L^2(\Omega)} + \norm{g}_{L^2(\Omega)}
$$

## Topology of $L^p$ as a vector space

$L^p$ is a normed vector space over the field $\R$, with the norm defined above! Everything works out.

Normed vector spaces such as $L^p$ can have **Cauchy sequences**: A sequence $\curlies{v_n}$ is Cauchy if for any $\epsilon > 0$, there is some $N$ so that if $m, n > N$ we know $\norm{v_m - v_n} < \epsilon$

$L^p$ is also **complete** - every Cauchy sequence $\curlies{v_n}$ in $L^p$ converges to a function $v \in L^p$

A normed vector space that is complete is called a **Banach space**

## Weak derivatives

### Multi-index notation

Let $\alpha = (\alpha_1, ... \alpha_n)$ with each $\alpha_i \geq 0$. Also define

$$
\abs{\alpha} = \sum_{i=1}^n \alpha_i
$$

Let $\phi \in C^\infty$, then

$$
D^\alpha \phi,\ D_x^\alpha \phi,\ \parens{\partials{}{x}}^\alpha \phi,\ \phi^{(\alpha)},\  \partial_x^{(\alpha)} \phi
$$

are all shorthand notation for

$$
\parens{\partials{}{x_1}}^{\alpha_1} \cdots \parens{\partials{}{x_n}}^{\alpha_n} \phi
$$

### Support

Let $\Omega \subseteq \R^n$ be open. For a continuous function $u : \R^n \to \R$, its **support** is the closure of the region where it is nonzero:

$$
\supp(u) = \overline{\curlies{x \in \R^n : u(x) \neq 0}}
$$

If $\supp(u)$ is a compact subset of $\Omega$, then we say *$u$ has compact support in $\Omega$*, and we know that $u$ vanishes near $\partial \Omega$.

Define $C_0^\infty(\Omega)$ as the set of smooth (infinitely differentiable) functions on $\Omega$ that have compact support.

### Locally integrable

The set of **locally integrable functions** is the set of functions that are integrable on all compact sets in the interior of the domain:

$$
L^1_\text{loc}(\Omega) = \curlies{f : f \in L^1(K) \text{ for all compact sets } K \subseteq \Domain^O}
$$

Note that $L^1(\Omega)$ and $L^1_\text{loc}(\Omega)$ are similar:

$$
\begin{align*}
u \in L^1(\Omega) &\Rightarrow \int_\Omega \abs u \d x < \infty \\
u \in L^1_\text{loc}(\Omega) &\Rightarrow \int_K \abs u \d x < \infty \text{ for every compact } K \subseteq \Omega^O
\end{align*}
$$

and furthermore $L^1_\text{loc}(\Omega) \subseteq L^1(\Omega)$

However, they are not the same - functions that blow up close to $\partial \Omega$ might be in $L^1_\text{loc}(\Omega)$ but not in $L^1(\Omega)$

e.g. $f(x) = 1/x$ with $\Omega = (0, 1)$:

$$
\int_0^1 \frac{1}{x} \d x = \infty
$$

However, for any compact $K \subseteq (0, 1)$,

$$
\int_K \frac{1}{x} \d x < \infty
$$

We can define the weak derivative for functions in $L^1_\text{loc}(\Omega)$

### Weak derivative - motivation

First suppose $f \in C^1(\Omega)$ and $\phi \in C^\infty(\Omega)$. Then the product rule tells us that $\partial_x(f \phi) = f\partial_x \phi + \phi \partial_x f$, so

$$
\int_\Omega \partial_x(f \phi) = \int_\Omega f\partial_x \phi + \int_\Omega \phi \partial_x f
$$

Using the divergence theorem we know that for $\mathbf F : \R^m \to \R^n$, we have

$$
\int_\Omega \nabla \cdot \mathbf F = \int_{\partial \Omega} \mathbf F \cdot \mathbf n
$$

If $\mathbf F = (f\phi, 0, 0)$, then

$$
\int_\Omega \partial_x (f \phi) = \int_\Omega \nabla \cdot \mathbf F = \int_{\partial \Omega} \mathbf F \cdot \mathbf n = \int_{\partial \Omega} f \phi n_1
$$

Putting this together with our earlier work,

$$
\begin{align*}
\int_{\partial \Omega} f\phi n_1 &= \int_\Omega f\partial_x \phi + \int_\Omega \phi \partial_x f \\
\int_{\partial \Omega} f\phi n_1 - \int_\Omega \phi \partial_x f  &= \int_\Omega f\partial_x \phi
\end{align*}
$$

If $\phi \in C^\infty_O(\Omega)$, i.e. it vanishes at $\partial \Omega$, then the boundary integral term is $0$.

## Weak derivative

Let $f \in L^1_\text{loc}(\Omega)$. $f$ has a **weak derivative** if there exists $g \in L^1_\text{loc}(\Omega)$ so that for all $phi \in C_0^\infty(\Omega)$,

$$
\int_\Omega g(x) \phi(x) \d x = (-1)^{\abs \alpha} \int_\Omega f(x) \phi^{(\alpha)} \d x
$$

If $g$ exists then we say it is the weak derivative of $f$ and we denote it as

$$
g = D_w^\alpha f
$$

### Example

Let $\Omega = (0, 1)$ and $f : \Omega \to \R$ is

$$
f(x) = \begin{cases}
x & 0 < x \leq \frac12 \\
1 - x  & \frac12 \leq x < 1
\end{cases}
$$

$f$ is not differentiable in the normal sense. However, we have the function

$$
g(x) = \begin{cases}
1 & x < \frac12
-1 & x > \frac12
\end{cases}
$$

And clearly $g = D_w^1 f$, since

$$
\frac14 = \int_0^1 f(x) \phi^1(x) \d x
$$

## Soblev spaces

Let $k \in \N$ and $f \in L^1_\text{loc}(\Omega)$. Suppose $D_\omega^\alpha f$ exist for all $\alpha$ with $\abs \alpha \leq k$.

The **Sobolev norm** is defined as

$$
\norm{f}_{W_p^k(\Omega)} = \begin{cases}
\displaystyle
\parens{\sum_{\abs{\alpha} \leq k} \norm{D_w^\alpha f}_{L^p(\Omega)}^p}^{1/p} & 1 \leq p < \infty \\
\displaystyle
\max_{\abs{\alpha} \leq k} \norm{D_w^\alpha f}_{L^\infty(\Omega)} & p = \infty
\end{cases}
$$

The **Sobolev space** is defined as

$$
W_p^k(\Omega) = \curlies{f \in L^1_\text{loc}(\Omega) : \norm{f}_{W_p^k(\Omega)} < \infty}
$$

Note that $W_p^0(\Omega) = L^p(\Omega)$

### Example

Let $\Omega \subset \R$, $k = 1$, so $\abs{\alpha} = 1$.

The corresponding Sobolev norms are

$$
\norm{f}_{W_p^1(\Omega)} = \begin{cases}
\displaystyle \parens{\norm{f}_{L^p(\Omega)}^p + \norm{f'}_{L^p(\Omega)}^p}^{1/p} & 1 \leq p < \infty
\\
\displaystyle \max \parens{\norm{f}_{L^p(\Omega)}^p, \norm{f'}_{L^p(\Omega)}^p} & p = \infty
\end{cases}
$$

### Relations to other spaces

Some Sobolev spaces can be related to other spaces, e.g. Lipschitz spaces

$$
\norm{f}_{\text{Lip}(\Omega)} = \norm{f}_{L^\infty(\Omega)} + \sup\curlies{\frac{\abs{f(x) - f(y)}}{\abs{x - y}} : x, y  \in \Omega, x \neq y}
$$

$$
\text{Lip}(\Omega) = \curlies{f \in L^\infty(\Omega) : \norm{f}_{\text{Lip}(\Omega)} < \infty}
$$

It can be shown that $\text{Lip}(\Omega) = W_\infty^1(\Omega)$.

### Sobolev spaces are Banach spaces

(A Banach space is a complete normed vector space)

Sobolev spaces are the "completions" of $C^\infty$ spaces. It is often difficult to work with functions in $W_p^k(\Omega)$, so instead we can deal with a sequence of $C^\infty(\Omega)$ functions that converge to our desired $W_p^k(\Omega)$ function.

Let $\Omega$ be any open set. Then $C^\infty(\Omega) \cap W_p^k(\Omega)$ is dense in $W_p^k(\Omega)$ for $1 \leq p < \infty$, i.e. for any $u \in W_p^(\Omega)$ there is a sequence $\curlies{u_m} \in C^\infty(\Omega) \cap W_p^k(\Omega)$ so that $\norm{u_m - u}_{W_p^k(\Omega)} \to 0$.

### Sobolev semi-norms

Let $k \in \N$ and $f \in W_p^k(\Omega)$. Then

$$
\abs{f}_{W_p^k(\Omega)} = \begin{cases}
\displaystyle
\parens{\sum_{\abs{\alpha} = k} \norm{D_w^\alpha f}_{L^p(\Omega)}^p}^{1/p} & 1 \leq p < \infty \\
\displaystyle
\max_{\abs{\alpha} = k} \norm{D_w^\alpha f}_{L^\infty(\Omega)} & p = \infty
\end{cases}
$$

This norm looks at the single case $\abs \alpha = k$ instead of every case $\abs \alpha \leq k$.

Semi norms have the same properties as norms, with the exception that we can have $\abs{v} = 0$ even if $v \neq 0$.

### Proposition 1.4.1

$W_p^m(\Omega) \subset W_p^k(\Omega)$ when $0 \leq k \leq m$ and $1 \leq p \leq \infty$.

(This is true because $\norm{u}_{W_p^k(\Omega)} \leq \norm{u}_{W_p^m(\Omega)}$)

### Proposition 1.4.2

$W_q^k(\Omega) \subset W_p^k(\Omega)$ when $1 \leq p < q \leq \infty$ and $\Omega$ is bounded

### Lipschitz domains

[insert formal definition]

Basically a Lipschitz domain is one where the domain does not lie on both sides of its boundary.

### Sobolev's inequality

Let $\Omega \subseteq \R^n$ be a Lipschitz domain. Let $k > 0$ and $1 \leq p < \infty$ such that

- $k \geq n$ when $p = 1$
- $k > n/p$ when $p > 1$

Then there exists a constant $C \in \R$ so that for all $u \in W_p^k(\Omega)$,

$$
\norm{u}_{L^p(\Omega)} \leq C \norm{u}_{W_p^k(\Omega)}
$$

and there is some function $w \in C^0(\Omega)$ so that

$$
\norm{w - u}_{L^\infty(\Omega)} = 0
$$

#### Example

Let $p = 2$, $k = 1$, and $n = 1$ i.e. $\Omega \subseteq \R$

Then there is some $C$ so that for all $u \in W_2^1(\Omega)$,

$$
\norm{u}_{L^\infty(\Omega)} \leq C \norm{u}_{W_2^1(\Omega)}
$$

and there is some $w \in C^0(\Omega)$ so that $\norm{w - u}_{L^\infty(\Omega)} = 0$

As a result, we can think of 1D functions in $W_2^1(\Omega)$ as continuous functions, and defining boundary conditions on them makes sense.

However, consider 2d functions i.e. $\Omega \subseteq \R^2$ with $p = 2$ and $k = 1$. Then $k \not > n/p$ so Sobolev's inequality's requirements are not satisfied.

We can still impose boundary conditions, howeveer we cannot interpret them in a pointwise sense.

### Trace theorem

Define the trace of $u \in W_p^k(\Omega)$ to be $u \bigg\vert_{\partial \Omega}$, i.e. $u$ restricted to the boundary

How do we interpret $u \bigg \vert_{\partial \Omega}$ for $n > 1$?

Let $\Omega$ be Lipschitz and $1 \leq p \leq \infty$. Then there is a $C \in \R$ so that

$$
\norm{v}_{L^p(\partial \Omega)} \leq C \norm{v}_{L^p(\Omega)}^{1 - 1/p} \norm{v}_{W_p^1(\Omega)}^{1/p}
$$

#### Example

Let $\Omega$ be Lipschitz and $p = 2$. Then we know

$$
\norm{v}_{L^2(\partial \Omega)} \leq C \norm{v}_{L^2(\Omega)}^{1/2} \norm{v}_{W_2^1(\Omega)}^{1/2}
$$

Since we know $\norm{v}_{W_2^1(\Omega)} < \infty$ and $\norm{v}_{L^2(\Omega)} < \infty$, we know that their product is finite, so $\norm{v}_{L^2(\partial \Omega)} < \infty$, so $v$ is something sensible.

Define new notation

$$
\mathring{W}_p^1(\Omega) = \curlies{v \in W_p^1 (\Omega) : v \bigg \vert_{\partial \Omega} = 0 \text{ in } L^2(\partial \Omega)}
$$

So $\mathring{W}_p^1(\Omega) \subset W_p^1(\Omega)$.

## Duality

A **linear functional** on a Banach space $B$ is a linear function $L : B \to \R$ so that for all $u, v \in B$ and $\alpha \in \R$,

$$
L(u + \alpha v) = L(u) = \alpha L(v)
$$

A linear functional on a Banach space is continuous if and only if it is bounded, i.e. if and only if $\abs{L(v)} \leq c\norm{u}_B$.

The set of all linear functionals on $B$ is the **dual space** of $B$, denoted by $B'$.

$B'$ is a Banach space with the dual norm

$$
\norm{L}_{B'} = \sup_{0 \neq v \in B} \frac{L(v)}{\norm{v}_B}
$$

The dual space of $L^p(\Omega)$ for $1 \leq p < \infty$ is $(L^p(\Omega))' = L^q(\Omega)$ where $\displaystyle \frac{1}{p} + \frac{1}{q} = 1$
