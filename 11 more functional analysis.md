# More functional analysis

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

## Inner products

For a vector space $V$, an inner product $\angles{\cdot, \cdot} : V^2 \to \R$ is an operation that satisfies:

- symmetry - $\angles{u, v} = \angles{v, u}$
- linearity - $\angles{\alpha u + v, w} = \alpha\angles{u, w} + \angles{v, w}$
- positive-definiteness - $\angles{u, u} = 0$ if and only if $u = 0$, otherwise $\angles{u, u} > 0$

### Examples of inner product spaces

- $V = \R^n$ with $\angles{\x, \y} = \x \cdot \y$
- $V = L^2(\Omega)$ with $\angles{u, v}_{L^p(\Omega)} = \displaystyle \int_\Omega uv \d x$
  - ($L^p$ spaces are not inner product spaces in general, only $L^2$)
- $V = W_2^k(\Omega)$ where $\angles{u, v} = \displaystyle \sum_{\abs \alpha \leq k} \angles{ D^\alpha u, D^\alpha v }_{L^p(\Omega)}$​
  - so for $W_2^1$, $\angles{u, v} = \angles{u, v}_{L^p(\Omega)} + \angles{u', v'}_{L^p(\Omega)}$
  - for $W_2^2$, $\angles{u, v} = \angles{u, v}_{L^p(\Omega)} + \angles{\partial_x u, \partial_x v}_{L^p(\Omega)} + \angles{\partial_y u, \partial_y v}_{L^p(\Omega)}$
  - Due to the importance of $p=2$, we denote $H^k(\Omega) = W_2^k(\Omega)$

### Schwartz inequality

For any inner product space,
$$
\abs{\angles{u, v}} \leq \sqrt{\angles{u, u}} \sqrt{\angles{v, v}}
$$
with equality only for linear dependence, i.e.
$$
\abs{\angles{u, v}} = \sqrt{\angles{u, u}} \sqrt{\angles{v, v}} \text{ if and only if } u = \lambda v
$$

### Norms/Hilbert spaces

Inner products induce norms:
$$
\norm{v} = \sqrt{\angles{v, v}}
$$
If the vector space is complete with respect to this norm, then the inner product space is called a **Hilbert space**.

$H^k(\Omega)$ are Hilbert spaces. $L^p(\Omega)$ are not Hilbert spaces in general, only for $p = 2$.

#### Subspaces of Hilbert spaces

If $H$ is a Hilbert space and $S \subseteq H$ is a linear subspace of $H$ that is closed in $H$, then $S$ is called a **subspace** of $H$. $S$ is also a Hilbert space.

Examples:

$\overset{0}{W_2^1}$ is a subspace of $H^1(\Omega) = W_2^1(\Omega)$

### Reisz Representation Theorem

Let $H$ be a Hilbert space. For each bounded linear functional on $H$, $L : H \to \R$, there exists a unique $u \in H$ so that
$$
L(v) = \angles{u, v}
$$
and
$$
\norm{L}_{H'} = \norm{u}_H
$$

## Symmetric weak formulation

Let $H$ be a normed linear space. Then a bilinear form $a: H^2 \to \R$ on $H$ satisfies:

- linearity: $a(\alpha u + v, w) = \alpha a(u, w) + a(v, w)$
- positivity: $a(u, u) \geq 0$ and $u = 0$ means $a(u, u) = 0$

A bilinear form $a$ could also be:

- symmetric: $a(u, v) = a(v, u)$
- bounded: there is some $c \in \R$ so that $\abs{a(u, v)} \leq c\norm{u}_H \norm{v}_H$
- coercivity: there is some $\alpha > 0$ so that $a(u, u) \geq \alpha \norm{u}_H^2$

### Observation

Let $H$ be a Hilbert space and $V$ a subspace of $H$. If a bilinear form $a$ is symmetric and continuous on $H$ and coercive on $V$, then $a$ is an inner product on $V$. $V$ with this inner product is a Hilbert space.

Proof.

We already know $a$ is symmetric and linear. Since $a$ is coercive on $V$, we know that there is some $\alpha$ so that $a(u, u) \geq \alpha \norm{u}_H^2$. Since this is a norm, it is only $0$ when $u = 0$, so $a(u, u)$ can only be $0$ when $u = 0$. Thus $a$ is an inner product.

Define the norm $\norm{u}_E = \sqrt{a(u, u)}$ induced by the inner product $a$. Let $\curlies{u_n}$ be a Cauchy sequence in $V$ with respect to $\norm \cdot _E$. In other words, for any $\epsilon > 0$ there is some $N$ so that if $m, n > N$ $, then

$$
\norm{u_m - u_n}_E < \epsilon
$$

Since $V$ is closed in $H$, it contains its limit points, so the limits of Cauchy sequences (with respect to $\norm{\cdot}_H$) are contained in $V$. i.e., if $\norm{u_n - v}_H \to 0$, then $v \in V$.

Since $a$ is coercive, we know $\alpha \norm{u}_H^2 \leq a(u, u) = \norm{u}_E^2$, and thus $$

### $V$ is a subspace of $H^1(\Omega)$

$$
V = \curlies{u \in H^1(\Omega) : u(0) = 0}
$$

We can show that $V$ is a subspace of $H^1(\Omega)$ by showing that $V$ is closed in $H^1(\Omega)$

Define
$$
\begin{align*}
\delta_0: H^1(\Omega) &\to \R \\
u &\mapsto u(0)
\end{align*}
$$
This is bounded:
$$
\begin{align*}
\norm{\delta_0}_{H'} &= \norm{\delta_0}_{H^{-1}(\Omega)} \\
&= \sup_{0 \neq v \in H^1(\Omega)} \frac{\abs{\delta_0(v)}}{\norm{v}_{H^1(\Omega)}} \\
&= \sup_{0 \neq v \in H^1(\Omega)} \frac{v(0)}{\norm{v}_{H^1(\Omega)}} \\
&\leq \sup_{0 \neq v \in H^1(\Omega)} \frac{\norm{v}_{L^\infty(\Omega)}}{\norm{v}_{H^1(\Omega)}}
\end{align*}
$$
Sobolev's inequality tells us that
$$
\norm{v}_{L^\infty(\Omega)} \leq c\norm{v}_{H^1(\Omega)}
$$
and thus
$$
\norm{\delta_0}_{H'} \leq c
$$
So $\delta_0$ is a bounded linear functional on $H^1(\Omega)$, which means it is continous.

From real analysis, we know that the preimage of a closed set under a continuous function is closed. Thus $V = \inv{\delta_0}(\curlies{0})$ is closed in $H^1(\Omega)$, so it is a subspace.

## Well-posedness of finite element method

Recall:

For our original boundary value problem, the weak form is to find $u \in V$ such that $a(u, v) = F(v)$ for each $v \in V$ where
$$
a(u, v) = \int_\Omega u'v' \d x \text{ and } F(v) = \int_\Omega f v\ d x
$$
We know that $V \subseteq H^1(\Omega)$ is a subspace.

We want $a$ to be an inner product on $V$. Clearly $a$ is symmetric. $a$ is bounded, since
$$
\begin{align*}
\abs{a(u, v)} &= \abs{\int_\Omega u' v' \d x} \\
&\leq \int_\Omega \abs{u' v'} \d x \\
&\leq \norm{u'}_{L^2(\Omega)} \norm{v'}_{L^2(\Omega)} \tag{Hölder's inequality}
\end{align*}
$$
$a$ is coercive too, since
$$
\begin{align*}
a(v, v) &= \int_\Omega (v')^2 \d x \\
&= \norm{v'}^2_{L^2(\Omega)} \\
&\geq \frac{2}{3} \norm{v}^2_{H^1(\Omega)} \tag{proof below}
\end{align*}
$$

#### Lemma: $H^1$ norm of $v$ is bounded by $L^2$ norm of $v'$

$$
\begin{align*}
\abs{v(x)} &= \abs{\int_0^x v(s) \d s} \tag{fund. thm. of calculus?} \\
&\leq \int_0^1 \abs{v'(s)} \d s \\
&\leq \sqrt{\int_0^1 \abs{1^2} \d s} \ \sqrt{\int_0^x \abs{v'(s)} \d s} \tag{Cauchy-Schwartz} \\
&= \sqrt x \sqrt{\int_0^x \abs{v'(s)} \d s} \\
\abs{v(x)}^2 &\leq x \int_0^x \abs{v'(s)} \d s \tag{square both sides} \\
\int_0^1 \abs{v(x)} \d x &\leq \int_0^1 \parens{ x \int_0^x \abs{v'(s)} \d s } \d x \\
&\leq \int_0^1 \parens{ x \int_0^1 \abs{v'(s)} \d s } \d x \\
&= \parens{\int_0^1 x \d x} \parens{\int_0^1 \abs{v'(s)} \d s} \\
&= \frac{1}{2} \int_0^1 \abs{v'(s)} \d s \\
\norm{v}_{L^2(\Omega)}^2 &\leq \frac{1}{2} \norm{v'}_{L^2(\Omega)}^2 \\
\norm{v}_{L^2(\Omega)}^2 + \norm{v'}_{L^2(\Omega)}^2 &\leq \frac{3}{2} \norm{v'}_{L^2(\Omega)}^2 \\
\norm{v}_{H^1(\Omega)}^2 &\leq \frac{3}{2} \norm{v'}_{L^2(\Omega)}^2
\end{align*}
$$

### Bounding functional

The work above shows that $a$​ is an inner product on $V$​.

Is $F$ a bounded functional on $V$, i.e. is there some $c$ so that $\abs{F(v)} \leq c\norm{v}_V$ (where $\norm{v}_V = \norm{v}_{H^1(\Omega)}$)?
$$
\begin{align*}
\abs{F(v)} &= \abs{\int_\Omega fv \d x} \\
&\leq \int_\Omega \abs{f v} \d x \\
&\leq \norm{f}_{L^2(\Omega)} \norm{v}_{L^2(\Omega)} \\
&\leq \norm{f}_{L^2(\Omega)} \norm{v}_{H^1(\Omega)}
\end{align*}
$$
If $f \in L^2(\Omega)$, then $\norm{f}_{L^2(\Omega)} = c < \infty$ so $F$ is bounded.

### Riesz representation theorem

Since $(V, a)$ is a Hilbert space (since $a$ is an inner product) and $F$ is a linear functional, Riesz representation theorem shows that there is a unique $u \in V$ so that $a(u, v) = F(v)$​. Thus, our weak form problem has a unique solution, so it is well-posed.
