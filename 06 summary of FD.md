# Summary of finite difference methods

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

- use $\partial_x$, $\overline \partial_x, $$\hat \partial_x$ to to approximate $\partials{}{x}$
- which one to use for each PDE?
  - discretization may be stable or unstable (conditionally or unconditionally) depending on choice of difference operator
- great for rectangular domains $[a_1, b_1] \times \cdots \times [a_n, b_n] \subseteq \R^n$
- 