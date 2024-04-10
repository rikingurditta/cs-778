# Systems of hyperbolic conservation laws

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

## System

$$
\partial_t \u + \partial_x \F(\u) = \bzero \\
\text{where } \u = \begin{pmatrix} u_1 \\ \vdots \\ u_m \end{pmatrix} \text{ and } \F(\x) = \begin{pmatrix} f_1(\x) \\ \vdots \\ f_m(\x) \end{pmatrix}
$$

