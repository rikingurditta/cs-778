## Temp

$$
\newcommand{\x}{\mathbf x}
\newcommand{\y}{\mathbf y}
\newcommand{\f}{\mathbf f}
\newcommand{\a}{\mathbf a}
\newcommand{\j}{\mathbf j}
\newcommand{\n}{\mathbf n}
\newcommand{\v}{\mathbf v}
\newcommand{\U}{\mathbf U}
\newcommand{\abs}[1]{\left\lvert #1 \right\rvert}
\newcommand{\norm}[1]{\left\lVert #1 \right\rVert}
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

$$
\begin{align*}
A_h U_j &= -\disclapl U_j + U_j \\
\end{align*}
$$

$$
\begin{align*}
-1 + \exp(-x) + x &> 0 \\
e^{-x} + x &> 1 \\
1 + xe^x &> e^x
\end{align*}
$$

$$
\begin{align*}
f(x) &= e^\lambda - e^{\lambda x} \\
\mathcal A_h f(x) &= -\frac{f(x+h)-2f(x)+f(x-h)}{h^2} + \frac{f(x+h)-f(x-h)}{2h} \\
&= -\frac{e^\lambda - e^{\lambda x}e^{\lambda h} - 2e^{\lambda} + 2e^{\lambda x} + e^\lambda - e^{\lambda x}e^{-\lambda h}}{h^2} + \frac{e^{\lambda} - e^{\lambda x}e^{\lambda h} - e^\lambda + e^{\lambda x}e^{-\lambda h}}{2h} \\
&= -\frac{-e^{\lambda x}e^{\lambda h} + 2e^{\lambda x} - e^{\lambda x}e^{-\lambda h}}{h^2} + \frac{- e^{\lambda x}e^{\lambda h} + e^{\lambda x}e^{-\lambda h}}{2h} \\
&= \frac{e^{\lambda x}}{h} \parens{ -\frac{2 - e^{\lambda h} - e^{-\lambda h}}{h} + \frac{e^{-\lambda h} - e^{\lambda h}}{2} } \\
&= \frac{e^{\lambda x}}{h} \parens{ \frac{e^{\lambda h} + e^{-\lambda h} - 2}{h} + \frac{e^{-\lambda h} - e^{\lambda h}}{2} } \\
\end{align*}
$$




$$
\begin{align*}
p(x, y) = \sin
\end{align*}
$$

$$
\abs{x - \delta} \leq \abs{x - E} \text{ because } \abs{\delta} \leq E
$$


$$
\abs{a - b} \leq \abs a - \abs b
$$

$$
\begin{align*}
\abs a = \abs{a - b + b} &\leq \abs{a - b} + \abs{b} \\
\abs a - \abs b &\leq \abs{a - b}
\end{align*}
$$







Let $\Omega = (0, 1)$ be our domain. Our original boundary value problem is, find a function $u$ such that
$$
-u'' + \beta u = f \text{ with } u'(0) = 0 \text{ and } u(1) = 0
$$
Define:
$$
\begin{align*}
V &= \curlies{v \in H^1(\Omega) : v(1) = 0} \\
a(u, v) &= \int_\Omega u' (\beta v + v') \d x \\
F(v) &= \int_\Omega f v \d x \\
\end{align*}
$$
Suppose $u \in V$ solves $F(u) = a(u, v)$ for every $v \in V$. Then $u$ solves the original boundary value problem.

*Proof.*

Since $u \in V$, we know that $u(1) = 0$.

Let $v \in V$​ be arbitrary.

Recall that the $L^2(\Omega)$ inner product is
$$
\angles{u, v} = \int_\Omega u v \d x
$$
Then, rewriting our equation using this inner product,
$$
\begin{align*}
F(v) &= a(u, v) \\
\angles{f, v} &= \angles{-u'' + \beta u', v} \\
\angles{f - (-u'' + \beta u'), v} &= 0
\end{align*}
$$
Since this holds for all $v \in V$, by the properties of the inner product we must have $f = -u'' + \beta u'$.
$$
\begin{align*}
F(v) &= a(u, v) \\
&= \int_\Omega u'(\beta v + v') \d x \\
&= \int_\Omega u' v' \d x + \beta \int_\Omega u' v \d x \\
&= u' v \bigg \vert_0^1 - \int_\Omega u'' v \d x + \beta \int_\Omega u' v \d x \tag{integration by parts} \\
&= u'(1) \underbrace{v(1)}_{=0} - u'(0)v(0) + \underbrace{\int_\Omega (-u'' + \beta u') v \d x}_{= F(v)} \\
0 &= -u'(0) v(0)
\end{align*}
$$
Since $v \in V$ is arbitrary, this must hold for $v(x) = 1 - x$, in which case $v(0) = 1$. So we must have $-u'(0) = 0$.

So $u$ solves the original BVP.








$$
\begin{align*}
\int_\Omega (u'v' + \beta uv) \d x &= \int_\Omega u'v' \d x + \beta \int_\Omega uv \d x \\
&= u'(1) v(1) - u'(0) v(0) - \int_\Omega u'' v \d x + \beta \int_\Omega u v \d x
\end{align*}
$$



$$
\begin{align*}
\int_\Omega \a \cdot \nabla u \d x &= ... \\
\a \cdot \nabla u &= a_1 \partials{u}{x} + a_2 \partials{u}{y} \\
s(\x) &= \a \cdot \x + b \\
\a \cdot \nabla u &= \nabla \cdot (u\a) \\
&= \nabla \cdot (u \nabla s) \\
-\nabla \cdot (\kappa \nabla u) + \a \cdot \nabla u &= f \\
\nabla \cdot (-\kappa \nabla u + u\nabla s) &= f \\
\int_\Omega \nabla \cdot (-\kappa \nabla u + u\nabla s) v \d x &= \int_\Omega f  v \d x \\
v(-\kappa \nabla u + u\nabla s) \cdot \n \bigg\vert_{\partial \Omega} -\int_\Omega (-\kappa \nabla u + u \nabla s) \cdot \nabla v \d x &= \int_\Omega fv \d x
\end{align*}
$$





















$$
\begin{align*}
\partial_t U + \partial_x F(U) &= 0 \\
U &= \begin{pmatrix} h \\ hu \end{pmatrix} \\
\partial_t U &= \begin{pmatrix} \partial_t h \\ \partial_t (hu) \end{pmatrix} \\
\partial_t U &= \begin{pmatrix} \partial_t h \\ u\partial_t h + h\partial_t u \end{pmatrix} \\
y &= hu \\
U &= \begin{pmatrix} h \\ y \end{pmatrix} \\
\partial_t U &= \begin{pmatrix} \partial_t h \\ \partial_t y \end{pmatrix} \\
F(U) &= \begin{pmatrix} hu \\ hu^2 + \frac{1}{2}gh^2 \end{pmatrix} \\
&= \begin{pmatrix} y \\ \frac{y^2}{h} + \frac{1}{2}gh^2 \end{pmatrix}
\end{align*}
$$



$$
\text{PDE is } -\Delta u + f = 0 \\
\tau = \disclapl u + f \\
\text{why not } \tau = -\disclapl u + f \text{ ? }
$$
