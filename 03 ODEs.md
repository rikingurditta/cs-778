# ODE methods

$$
\newcommand{\x}{\mathbf x}
\newcommand{\y}{\mathbf y}
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

Consider the ODE

$$
\dbyd{\y}{t} = \f(t, \y(t)) \text{ for } t \geq 0, \y(0) = \y_0, \y \in \R^d
$$

## Discretization

Discretize time into $0 = t_0$ and $t_j = jk$ where $k = \Delta t$ denotes our time interval

## Time integration

Given $\y(t_n)$, we want to find $\y(t_{n+1})$. We can do this by solving the integral equation

$$
\begin{align*}
\y(t_{n+1}) &= \y(t_n) + \int_{t_n}^{t_{n+1}} \y'(t) \d t \\
&= \y(t_n) + \int_{t_n}^{t_{n+1}} \f(t, \y(t)) \d t
\end{align*}
$$

This may not have a closed form solution, so we approximate it

We use $\y^n$ to denote our numerical approximation of $\y(t_n)$

## Common time integration schemes

### Forward Euler

The simplest way to approximate the integral is

$$
\int_{t_n}^{t_{n+1}} f(t, \y(t)) \d t \approx (t_{n+1} - t_n) \f(t_n, \y(t_n)) \approx  k\f(t_n, \y^n)
$$

This gives us the time integration scheme

$$
\y^{n+1} := \y^n + k\f(t_n, \y^n)
$$

It is first-order accurate

### Backward Euler

Similarly, we can approximate our integral using the next value of $\y$, i.e.

$$
\int_{t_n}^{t_{n+1}} f(t, \y(t)) \d t \approx k\f(t_{n+1}, \y^{n+1})
$$

And so

$$
\y^{n+1} := \y^n + k\f(t_{n+1}, \y^{n+1})
$$

Note that this is an **implicit scheme** - our $\f$ value depends on the *next* value of $\y$. In practice, this means that the solution to the above equation is not as simple as forward Euler, it may require some sort of equation solving scheme.

It is first-order accurate (though it is unconditionally stable)

### Trapezoid rule/Crank-Nicolson

$$
\y^{n+1} = \y^n + \frac{k}{2}(\f(t_n, \y^n) + \f(t_{n+1}, \y^{n+1}))
$$

This is implicit, and it is second-order accurate
