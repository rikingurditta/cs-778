# Temp

$$
\newcommand{\x}{\mathbf x}
\newcommand{\y}{\mathbf y}
\newcommand{\f}{\mathbf f}
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
