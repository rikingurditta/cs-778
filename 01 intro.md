# Introduction

$$
\newcommand{\x}{\mathbf x}
\newcommand{\b}{\mathbf b}
\newcommand{\j}{\mathbf j}
\newcommand{\n}{\mathbf n}
\newcommand{\v}{\mathbf v}
\newcommand{\abs}[1]{\left\lvert #1 \right\rvert}
\newcommand{\brackets}[1]{\left[ #1 \right]}
\newcommand{\angles}[1]{\left\langle #1 \right\rangle}
\newcommand{\curlies}[1]{\left\lbrace #1 \right\rbrace}
\newcommand{\inv}[1]{#1^{-1}}
\newcommand{\d}{\, \text{d}}
\newcommand{\dbyd}[2]{\frac{\d #1}{\d #2}}
\newcommand{\partials}[2]{\frac{\partial #1}{\partial #2}}
$$

##  Modelling of heat flow

Consider a body $\Omega \in \R^3$, and some subset $\Omega_0 \subseteq \Omega$ with boundary $T_0$

### Conservation of energy

The rate of change of the total energy in $\Omega_0$ is inflow of heat through $T_0$  + heat produced by sources in $\Omega_0$

#### Notation

- $e = e(\x, t) \in \R$ ($Jm^{-3}$) is the density of internal energy
- $\j = \j(\x, t) \in \R^3$ is the heat flux
- $p = p(\x, t) \in \R$ ($J m^{-3} s^{-1})$) is the power density of heat sources

If we have a surface $S$ embedded in $\Omega_0$ with normal $\n$, then the heat flowing through $S$ in the direction of $\n$ per unit time is

$$
\int_S \j \cdot \n \d s
$$

The total amount of heat in $\Omega_0$ is

$$
\int_{\Omega_0} e \d \x
$$

The net outflow of heat through $T_0$ is

$$
\int_{T_0} \j \cdot \n \d s
$$

(this is outflow because the normal $\n$ faces outward)

#### Calculating conservation of energy

$$
\begin{align*}
\dbyd{}{t} \int_{\Omega_0} e \d \x
&= -\int_{T_0} \j \cdot \n \d s + \int_{\Omega_0} p \d \x \\
\int_{\Omega_0} \partial_t e \d \x &= - \int_{\Omega_0} (-\nabla \cdot \j + p) \d \x \tag{divergence theorem} \\
\int_{\Omega_0} (\partial_t e + \nabla \cdot \j - p) \d \x &= 0
\end{align*}
$$

Since $\Omega_0$ is an arbitrary subset of $\Omega$, this is true for any subset, so the integrand must be 0. So we arrive at the PDE

$$
\partial_t e + \nabla \cdot \j - p = 0 \text{ in } \Omega \text{ for } t > 0
$$

If $e$ depends linearly on the temperature $T$ ($K$), then we know $e = e_0 + \sigma(T - T_0)$ where $e_0$ is the reference internal energy at the reference temperature $T_0$ and $\sigma$ is a constant determined emperically. By substituting $\theta = T - T_0$, we get

$$
e = e_0 + \sigma \theta
$$

Fourier's law: heat flux is proportional to temperature gradient, so

$$
\j = -\lambda \nabla \theta + \v e
$$

Where $\lambda$ is the heat conductivity of the material and $\$ is the conductive velocity

Combining these equations with our conservation law, we have

$$
\begin{align*}
0 &= \partial_t (e_0 + \sigma \theta) + \nabla \cdot (-\lambda \nabla \theta + \v(e_0 + \sigma \theta)) - p \\
p - \nabla \cdot \v e_0 &= \sigma \partial_t \theta - \nabla \cdot (\lambda \nabla \theta) + \nabla \cdot (\v \sigma \theta) \\
q &= \sigma \partial_t \theta - \nabla \cdot (\lambda \nabla \theta) + \nabla \cdot (\v \sigma \theta) \tag{$q$ is a known constant}
\end{align*}
$$

#### Rewriting and studying

Consider the general equation modeling heat on $\Omega$

$$
\partial_t u
+ \underbrace{\b \cdot \nabla u}_\text{advection}
- \underbrace{\nabla \cdot (a \nabla u)}_\text{diffusion}
+ \underbrace{cu}_\text{reaction} = f
$$

We need initial conditions on $\Omega$ and and boundary conditions on its boundary $T$:

- Initial conditions: $u(\x, 0) = u_0(\x)$ on $\Omega$
- Dirichlet boundary conditions: $u(\x, t) = g(\x)$ on $T$
- Neumann boundary conditions: $\partials{u}{\n} = g(\x)$ on $T$
- Robin boundary conditions: $a\partials{u}{\n} + h(u - g) = 0$ on $T$

#### Simplifications

If convection and reaction are negligible, then we get the heat equation (parabolic)

$$
\partial_t u - \nabla \cdot (a \nabla u) = f
$$

If the problem doesn't depend on time and doesn't have advection and reaction terms (elliptical)

$$
\begin{align*}
\Delta u &= f \tag{Poisson equation} \\
\Delta u &= 0 \tag{Laplace equation}
\end{align*}
$$

If diffusion and reaction are negligible, then we get ... (hyperbolic)

$$
\partial_t u + \b \cdot \nabla u = f
$$
