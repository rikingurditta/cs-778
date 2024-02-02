# Finite difference methods for parabolic problems

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
$$

Consider the initial value problem for $u(x, t)$: (1d Heat equation)

$$
\begin{align*}
\partials{u}{t} &= \partials{^2 u}{x^2} &\text{ for } t \geq 0, -\infty < x \leq \infty \\
u(x, 0) &= u(x)
\end{align*}
$$

## Bounds

Two useful bounds:

1. $\displaystyle \sup_x \abs{u(x, t)} \leq \sup_x \abs{u(x)}$
2. $\displaystyle \sup_x \abs{\partials{^4u}{x^4} (x, t)} \leq \sup_x \abs{\partials{^4u}{x^4} (x)}$

## Discretizing with finite differences

Use $h$ as spacial grid size and $k$ as time step

$$
\begin{align*}
\dbyd{U_j(t)}{t} &= \disclapl[x] U_j (t) \\
U_j(0) &= u(x_j)
\end{align*}
$$

## Integrating using Euler's method

$$
U_j^{n+1} = U_j^n + k \disclapl[x] U_j^n
$$

Can look at forward difference over time:

$$
\partial_t U^n = \frac{U^{n+1} - U_n}{k}
$$

Three equivalent ways to write the Euler's method for this PDE:

1. $U_j^{n+1} = U_j^n + \frac{k}{h^2} (U_{j+1}^n - 2U_j^n + U_{j-1}^n)$
2. $\frac{1}{k}(U_j^{n+1} - U_j^n) = \frac{1}{h^2}(U_{j+1}^n - 2U_j^n + U_{j-1}^n)$
3. $\partial_t U_j^n = \disclapl[x] U_j^n$

### Iteration

Let $\lambda = k/h^2$, then we can rewrite the method as

$$
(E_k U^n)_j := \lambda U_{j-1} + (1 - 2\lambda) U_j^n + \lambda U_{j+1}^n = U_j^{n+1}
$$

Thus $E_k: U^n \mapsto U^{n+1}$

Using this operator, we can write the evolution of our PDE as iteration:

$$
\begin{align*}
U_j^0 &= u(x_j) \\
U_j^1 &= (E_k U^0)_j \\
U_j^2 &= (E_k U^1)_j = (E_k^2 U^0)_j \\
&\ \  \vdots \\
U_j^n &= (E_k^n U^0)_j
\end{align*}
$$

### Discrete maximum norm

Define

$$
\norm{u}_{\infty, h} = \sup_j \abs{u_j}
$$

### Stability

Suppose $\lambda \leq \frac{1}{2}$. Then

$$
\begin{align*}
\abs{U_j^{n+1}} &= \abs{\lambda U_{j-1} + (1 - 2\lambda) U_j^n + \lambda U_{j+1}^n}\\
&\leq \lambda \abs{U_{j-1}} + (1 - 2\lambda) \abs{U_j^n} + \lambda \abs{U_{j+1}^n} \\
&\leq \lambda \norm{U^n}_{\infty, h} + (1 - 2\lambda) \norm{U^n}_{\infty, h} + \lambda \norm{U^n}_{\infty, h} \\
&= \norm{U^n}_{\infty, h}
\end{align*}
$$

Thus,

$$
\begin{align*}
\norm{U_j^{n+1}}_{\infty, h} &\leq \norm{U_j^n}_{\infty, h} \\
&= \norm{E_k U_j^n}_{\infty, h} \\
&= \norm{E_k^n u}_{\infty, h}
\end{align*}
$$

And so our discrete solution operator is bounded by our initial data

Suppose $\lambda > \frac{1}{2}$. Then

Suppose $u_j = (-1)^j \epsilon$, where $\epsilon$ is small. Then $\displaystyle \norm{u}_{\infty, h} = \sup_j \abs{u_j} = \epsilon$. Applying this to our update rule,

$$
\begin{align*}
U_j^{n+1} &= \lambda U_{j-1} + (1 - 2\lambda) U_j^n + \lambda U_{j+1}^n \\
&= \lambda (-1)^{j-1} + (1 - 2\lambda) (-1)^j + \lambda (-1)^{j+1} \\
&= (1 - 4\lambda) (-1)^j \epsilon
\end{align*}
$$

So by induction,

$$
U_j^n = (1-4\lambda)^n (-1)^j \epsilon
$$

This will blow up if $\lambda > \frac{1}{2}$, so Euler's method is not stable in this case.

### Consistency

Suppose $u$ is the true solution to our problem (and use the shorthand $u_j^n = u(x_j, t_n)$) and consider truncation error:

$$
\begin{align*}
\tau_j^n &= \partial_t u_j^ n - \disclapl[x] u_j^n \\
&= \partial_t u_j^ n - \disclapl[x] u_j^n - \partials{u}{t} + \partials{^2u}{x^2} \\
&= \parens{ \partial_t u_j^ n - \partials{u}{t} }
- \parens{ \disclapl[x] u_j^n - \partials{^2u}{x^2} }
\end{align*}
$$

For some $\tilde t_n \in [t_n, t_{n+1}]$, Taylor's theorem says

$$
u_j^{n+1} = u_j^n + k\partials{u}{t}(x_j, t_n) + \frac{1}{2} k^2 \partials{^2u}{t^2}(x_j, \tilde t_n)$
$$

And therefore

$$
\begin{align*}
\frac{u_j^{n+1} - u_j^n}{k} - \partials{u}{t}(x_j, t_n) &= \frac{1}{2}k \partials{^2 u}{t^2} (x_j, t_n) \\
\partial_t u_j^n - \partials{u}{t} &= \frac{1}{2}k \partials{^2 u}{t^2} (x_j, \tilde t_n)
\end{align*}
$$

We also know from our studies of elliptic PDEs that for some $\tilde x_j$

$$
\disclapl[x]u_j^n - \partials{^2 u}{x^2} (x_j, t_n) = \frac{1}{12} h^2 \partials{^4 u}{x^4} (\tilde x_j, t_n)
$$

And so we can bound our truncation error

$$
\begin{align*}
\tau_j^n &= \frac{1}{2}k \partials{^2 u}{t^2} (x_j, \tilde t_n) + \frac{1}{12} h^2 \partials{^4 u}{x^4} (\tilde x_j, t_n) \\
\tau_j^n &\leq \frac{1}{2} k \max_{t_n \leq t \leq t_{n+1}} \sup_x \abs{\partials{^2 u}{t^2}} + \frac{1}{12} h^2 \sup_x \abs{\partials{^4 u}{x^4}} \\
&= \frac{1}{2} k \sup_x \abs{\partials{^4 u}{x^4}} + \frac{1}{12} h^2 \sup_x \abs{\partials{^4 u}{x^4}} \tag{$\partials{^2u}{t^2} = \partials{^4u}{x^4}$} \\
&= h^2 \sup_x \abs{\partials{^4 u}{x^4}} \underbrace{\parens{ \frac{k}{2h^2} + \frac{1}{12}}}_\text{constant} \\
\norm{\tau^n}_{\infty, n} &\leq Ch^2 \sup_x \abs{\partials{^4 u}{x^4}}
\end{align*}
$$

This goes to 0 [...]

### Theorem 9.1

Let $u$ be the true solution to the 1D heat equation and $U_j^n$ be its approximation using the finite difference and Euler's method scheme described above. Let $\lambda = k/h^2 \leq 1/2$. Then there is some $c > 0$ so that

$$
\norm{U^n - u^n}_{\infty, h} \leq ct_n h^2 \sup_x \abs{\partials{^4 u}{x^4}}
$$

*Proof.*

Let $z_j^n = U_j^n - u_j^n$. Then

$$
\begin{align*}
\partial_t z_j^n - \disclapl[x] z_j^n &= \partial_t U_j^n - \partial_t u_j^n - \disclapl[x] U_j^n + \disclapl[x] u_j^n \\
&= -\partial_t y_j^n + \disclapl[x] u_j^n \tag{$\partial_t U_j^n = \disclapl[x] U_j^n$} \\
&= -\tau_j^n
\end{align*}
$$
