# Helmert Transformation

The Helmert transformation formula as we know it:

$$
\begin{equation}
    \begin{bmatrix}
        x
        \\
        y
        \\
        z
    \end{bmatrix}^B
%
    =
%
    \begin{bmatrix}
        t_x \\ t_y \\ t_z
    \end{bmatrix}
%
    + (1+s\cdot10^{-6})
%
    \begin{bmatrix}
        1       & -r_{z}  &  r_{y}   \\
        r_{z}   &  1      & -r_{x} \\
        -r_{y}   &  r_{x}  &  1
    \end{bmatrix}
%
    \begin{bmatrix}
        x \\ y \\ z
    \end{bmatrix}^A
\end{equation}
$$

can also be expressed as

$$
\begin{equation}
    \label{eq:simplehelmert}
    P_{B} = T + c\mathbf{R}P_{A}
\end{equation}
$$

where $P_{A}$ and $P_{B}$ are the input and output coordinate vectors,
$T$ is the translation vector, $\mathbf R$ is the rotation matrix and the scale
factor $c$ is expressed as

$$
\begin{equation}
    c = 1+s\cdot10^{-6}
\end{equation}
$$

### Chaining transformations


It is quite common to perform several Helmert transformations
in a row, e.g.

$$
\begin{equation}
    P_B = T_1 + c_1\mathbf{R_1}P_A
\end{equation}
$$

$$
\begin{equation}
    P_C = T + c_2\mathbf{R_2}P_B
\end{equation}
$$

where the coordinate $P_A$ is transformed with the parameters of the first
step ($T_1$, $c_1$ & $\mathbf{R_1}$) and then the parameters of the second step
($T_2$, $c_2$ & $\mathbf{R_2}$).

This is computationally heavy and can be avoided by
substituting one into the other:

$$
\begin{equation}
    P_C = T_2 + c_2\mathbf{R_2} \left( T_1 + c_1 \mathbf{R_1} P_A \right)
\end{equation}
$$

$$
\begin{equation}
    P_C = T_2 + c_2\mathbf{R_2}T_1 + c_2c_1 \mathbf{R_2}\mathbf{R_1} P_A
\end{equation}
$$

which can be simplified using the following expressions

$$
\begin{equation}
    T_3 = T_2 + c_2\mathbf{R_2}T_1
\end{equation}
$$

$$
\begin{equation}
    c_3 = c_2 \cdot c_1
\end{equation}
$$

$$
\begin{equation}
    R_3 = \mathbf{R_2}\mathbf{R_1}
\end{equation}
$$

And with a bit of substitution we get

$$
\begin{equation}
    P_C = T_3 + c_3 \mathbf{R_3} P_A
\end{equation}
$$

which is on the same form as the Helmert transformation shown in
in $\ref{eq:simplehelmert}$.

A chained Helmert transformation can be sped up significantly by first
determining $T_3$, $c_3$ and $\mathbf R_3$ and using them as parameters in
the original Helmert transformation formula.

Shown here is only the 7-parameter version of the Helmert transformation
but of course this obviously also extends to the 14-parameter version and
combinations of the two.
