# Proving FEM

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

## Finite element method

The weak formulation of our PDE is, given $F \in V'$, find $u \in V$ so that $a(u, v) = F(v)$ for all $v \in V$.

The finite element method formulation is, let $V_h \subseteq V$ be finite dimensional. Given $F \in V'$, find $u_h \in V_h$ so that $a(u_h, v_h) = F(v_h)$ for all $v_h \in V_h$.

### Theorem 2.5.8

Suppose $(H, \angles{\cdot, \cdot})$ is a Hilbert space, $V_h \subseteq H$ is a subspace, and $a(\cdot, \cdot)$ is a bounded symmetric bilinear form that is coercive on $V_h$. Then there exists a unique solution to our finite element method problem.

*Proof.*

$(V_h, a(\cdot, \cdot))$ is a Hilbert space.

We know that $F \in V'$, i.e. $\abs{F(v)} \leq c \norm{v}_V$ for all $v \in V$. But, $V_h \subseteq V$ so we also have

$$
\abs{F(v_h)} \leq c \norm{v_h}
$$

Thus, we know that for the restriction of $F$ to $V_h$,

$$
F \big \vert_{V_h} \in V_h'
$$

Using the Riesz representation theorem, we know that there is an element of the primal space that corresponds to this functional, i.e.

$$
\text{there is some } u_h \in V_h \text{ so that } a(u_h, v_h) = F(v_h) \text{ for all } v_h \in V
$$

This is exactly what we want!

## Nonsymmetric weak formulations

If $a(\cdot, \cdot)$ is not symmetric, then it is not an inner product. However, the Lax-Milgram theorem generalizes the Riesz representation theorem to show the same result

### Lax-Milgram theorem

If $(V, \angles{\cdot, \cdot})$ is a Hilbert space, $a(\cdot, \cdot)$ is a continuous coercive bilinear form, and $F \in V'$ is a continuous linear functional, then there is a unique $u \in V$ so that $a(u, v) = F(v)$ for all $v \in V$.

## Conditions for solutions

Thus the conditions to find a solution to our weak formulation are

1. $(H, \angles{\cdot, \cdot})$ is a Hilbert space
2. $V \subseteq H$ is a subspace
3. $a(\cdot, \cdot)$ is a bilinear form on $V$
4. $a(\cdot, \cdot)$ is continuous on $V$
5. $a(\cdot, \cdot)$ is coercive on $V$​
6. $F \in V'$ is bounded

If we use a finite dimensional subspace $V_h$, then we solve our finite element method problem.

## Example problem

Consider the boundary value problem:

$$
\begin{cases}
-u'' + u' + u = f & \text{on } \Omega = (0, 1) \\
u'(0) = u'(1) = 0
\end{cases}
$$

### Weak formulation

To find the weak formulation of this problem, we introduce a test function $v$ and integrate by parts:

$$
\begin{align*}
\int_\Omega v(-u'' + u' + u) \d x &= \int_\Omega f v \d x \\
\underbrace{\int_\Omega (u' v' + u' v + uv) \d x}_{a(u, v)} - \underbrace{(u'(1) v(1) - u'(0) v(0))}_{=0} &= \underbrace{\int_\Omega f v \d x}_{F(v)} \\
a(u, v) &= F(v)
\end{align*}
$$

Note here that $a(u, v)$ is not symmetric! Also note that since we are only dealing with first derivatives, we only require this to be true for all $v \in V = H^1(\Omega)$.

So the weak formulation of our problem is

$$
\text{find } u \in H^1(\Omega) \text{ so that for all } v \in H^1(\Omega) \text{, } a(u, v) = F(v)
$$

Let $u, v \in V$ be arbitrary. Then

$$
\begin{align*}
\abs{a(u, v)} &= \abs{\int_\Omega (u'v' + uv) \d x + \int_\Omega u'v \d x} \\
&= \abs{\angles{u, v}_{H^1(\Omega)} + \angles{u', v}_{L^2(\Omega)}} \\
&\leq \abs{\angles{u, v}_{H^1(\Omega)}} + \abs{\angles{u', v}_{L^2(\Omega)}} \tag{triangle ineq} \\
&\leq \norm{u}_{H^1(\Omega)} \norm{v}_{H^1(\Omega)} + \norm{u'}_{L^2(\Omega)} \norm{v}_{L^2(\Omega)} \\
&\leq 2\norm{u}_{H^1(\Omega)} \norm{v}_{H^1(\Omega)} \tag{$H^1$ norm $\geq$ $L^2$ norm}
\end{align*}
$$

Thus $a$ is bounded.

$$
\begin{align*}
a(v, v) &= \int_\Omega ((v')^2 + v'v + v^2) \d x \\
&= \frac{1}{2} \int_\Omega \parens{(v' + v)^2 + (v')^2 + v^2} \d x \\
&= \frac{1}{2} \int_\Omega \underbrace{(v' + v)^2}_{\geq 0} \d x + \frac{1}{2} \int_\Omega (v')^2 + v^2 \d x \\
&\geq \frac{1}{2} \int_\Omega (v')^2 + v^2 \d x \\
&= \frac{1}{2} \norm{v}_{H^1(\Omega)}
\end{align*}
$$

So $a$ is coercive.

## Céa's theorem

Suppose $u \in V$ solves the weak formulation of our problem and $u_h \in V_h$ solves the finite element method problem. Then

$$
\begin{align*}
\text{WF:}\quad a(u, v) &= F(v) \text{ for all } v \in V \\
\Rightarrow a(u, v_h) &= F(v_h) \text{ for all } v_h \in V_h (\subseteq V) \\
\text{FEM:}\quad a(u_h, v_h) &= F(v_h) \text{ for all } v_h \in V_h
\end{align*}
$$

We can combine these facts to get an orthogonality result:

$$
a(u - u_h, v_h) = 0 \text{ for all } v_h \in V_h
$$

Consider $a(u - u_h, u - u_h)$. Since $a$ is coercive, we know

$$
\begin{align*}
\alpha \norm{u - u_h}_V^2 &\leq a(u - u_h, u - u_h) \\
&= a(u - u_h, u - v_h + v_h - u_h) \\
&= a(u - u_h, u - v_h) + a(u - u_h, \underbrace{v_h - u_h}_{\in V_h}) \\
&= a(u - u_h, u - v_h) \tag{orthogonality} \\
\alpha \norm{u - u_h}_V^2 &\leq c\norm{u - u_h}_V \norm{u - v_h}_V \\
\norm{u - u_h}_V &\leq \frac{c}{\alpha} \norm{u - v_h}_V
\end{align*}
$$

Since this holds for all $v_h \in V_h$, it holds for the $v_h$ that minimizes $\norm{u - v_h}_V$, so we can conclude

$$
\norm{u - u_h}_V \leq \frac{c}{\alpha} \min_{v \in V_h} \norm{u - v_h}_V
$$

Therefore the finite element method finds a good solution!

