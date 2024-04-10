# CS 778 Final exam cheat sheet

$$
\newcommand{\x}{\mathbf x}
\newcommand{\y}{\mathbf y}
\newcommand{\b}{\mathbf b}
\newcommand{\e}{\mathbf e}
\newcommand{\f}{\mathbf f}
\newcommand{\j}{\mathbf j}
\newcommand{\n}{\mathbf n}
\newcommand{\u}{\mathbf u}
\newcommand{\F}{\mathbf F}
\newcommand{\U}{\mathbf U}
\newcommand{\v}{\mathbf v}
\newcommand{\w}{\mathbf w}
\newcommand{\I}{\mathcal I}
\newcommand{\L}{\mathcal L}
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
\newcommand{\uja}{\hat u_1^j}
\newcommand{\ujb}{\hat u_2^j}
\newcommand{\vja}{\hat v_1^j}
\newcommand{\vjb}{\hat v_2^j}
\DeclareMathOperator{\span}{span}
\DeclareMathOperator{\ess}{ess}
\DeclareMathOperator{\supp}{supp}
\DeclareMathOperator{\sign}{sign}
$$

## General

### Evaluating numerical methods

#### Truncation error $\tau$

error when substituting the exact solution into the discretization

e.g. if PDE is $-\Delta u + f = 0$, then discretization might be $-\disclapl U_j^n + f_j^n = 0$ so truncation error would be

$$
\tau_j^n = -\disclapl u_j^n + f_j^n
$$

#### Consistency

a method is consistent if $h \to 0$ implies $\abs{\tau_j} \to 0$

#### Convergence

a method is convergent if $h \to 0$ implies that the numerical solution $U$ approaches the true solution $u$, i.e.

$$
h \to 0 \text{ implies } \abs{U - u} \to 0
$$

#### Stability

a method is stable if it does not blow up, i.e. if the magnitude of the solution is bounded

if the method does not change in time, stability involves bounding $\abs U$ in general

if the method does change in time, we want to know whether $\norm{U^{n+1}} \leq \norm{U^n} \leq \norm{U^0}$

## Finite difference methods

### Finite difference operators

Forward difference

$$
\partial_x U = \frac{U_{j+1} - U_j}{h}
$$

Backward difference

$$
\overline \partial_x U = \frac{U_j - U_{j-1}}{h}
$$

Central difference

$$
\hat \partial_x U = \frac{U_{j+1} - U_{j-1}}{2h}
$$

Laplacian

$$
\disclapl[x]U = \frac{U_{j+1} - 2U_j + U_{j-1}}{h^2}
$$

### Mesh function norms

#### Max norm

$$
\abs{v} = \norm{v}_{\infty, h} = \sup_j \abs{v_j}
$$

#### $L^2$ norm

$$
\norm{v}_{2, h} = \sqrt{h \sum_j v_j^2}
$$

### Stability

#### Solution operator

Assume $E_k$ is the solution operator for a PDE, i.e. $\U^{n+1} = E_k \U^n$

Then for a specific mesh point, we have

$$
U_j^{n+1} = (E_k \U^n)_j = \sum_p a_p U_{j-p}^n
$$

e.g. if our scheme is $$\partial_t U_j^n = \disclapl[x] U_j^n$$, i.e. $U_j^{n+1} = U_j^n + \frac{k}{h^2} (U_{j+1}^n - 2U_j^n + U_{j-1}^n)$, then

$$
\begin{align*}
(E_k \U^n)_j &= a_{-1} U_{j+1}^n + a_0 U_j^n + a_1 U_{j-1}^n \\
&= \frac{k}{h^2} U_{j+1}^n + \parens{1 - \frac{2k}{h}} U_j^n + \frac{k}{h} U_{j-1}^n
\end{align*}
$$

#### Symbol

The symbol is the trigonometric polynomial associated with $E_k$:

$$
\tilde E(\xi) = \sum_p a_p e^{-ip \xi}
$$

For the scheme above, the symbol is

$$
\tilde E(\xi) = \frac{k}{h^2} e^{i\xi} + \parens{1 - \frac{2k}{h}} + \frac{k}{h} e^{-i\xi}
$$

A scheme is stable with respect to the max norm if $\abs{\tilde E(\xi)} \leq 1$ for all $\xi \in \R$.


## Weak formulations of PDEs

Finding a solution $u \in C^n(\Omega)$ for $$\L(u) = f$$ from might not be feasible in practice because our $f$ could be less smooth or discontinuous. It may also help us to consider non-smooth solutions as we search for a discrete solution. So we weaken our PDE $\L(u) = f$ to finding a solution $u \in V$ such that for every $v \in V$, we satisfy

$$
\int_\Omega \L(u) v \d x = \int_\Omega f v \d x
$$

where $V$ is a broader function space, e.g. $W_p^k(\Omega)$
