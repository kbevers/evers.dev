# Euler Poles

Euler poles are useful to describe the motion of a tectonic plate. An Euler pole is usually
expressed by a latitude and longitude, $(\phi, \lambda)$, locating an axis passing through
the Earth and a rotation rate, $\Omega$, about the axis given in degrees per million years.

In terms of plate motion models an Euler pole is describing the point a rigid tectonic
plate revolves around. We can use this to the describe how a point on a tectonic plate
moves over time in relation to a global reference frame.

The velocity of a point on a tectonic plate can be determined using the Euler theorem:

$$
\begin{equation}
    \mathbf{v} =
%
    \begin{bmatrix}
        0       & -\omega_{z}  &  \omega_{y}   \\
        \omega_{z}   &  0      & -\omega_{x} \\
        -\omega_{y}   &  \omega_{x}  &  0
    \end{bmatrix}
%
    \begin{bmatrix}
        x \\ y \\ z
    \end{bmatrix}
\end{equation}
$$

The rotation parameters $\omega_x$, $\omega_y$ and $\omega_z$ can be derived from
the Euler pole parameters $\Omega$, $\phi$ and $\lambda$:

$$
\begin{equation}
    \label{eq:rotrates}
    \begin{bmatrix}
        \omega_x \\ \omega_y \\ \omega_z
    \end{bmatrix}
%
    =
%
    \frac{\Omega}{10^6}
%
    \begin{bmatrix}
        cos(\phi) cos(\lambda) \\
        cos(\phi) sin(\lambda) \\
        sin(\phi)
    \end{bmatrix}
\end{equation}
$$

The rotation parameters in this form can be used as the rotation rates of a 14-parameter Helmert transformation.

Below is a simple Python implementation of the equation $\ref{eq:rotrates}$.

```python
from math import pi, sin, cos

def euler_pole_to_helmert_rotation_rates(lat: float, lon: float, rotation: float) -> tuple[float]:
    """
    Convert Euler pole parameters to Helmert rotation rate parameters.

    Latitude and longitude given in degrees.
    Rotation rate giving in degrees/Myr.

    Output in arcsec/yr.

    Based on

        Goudarzi, Mohammad & Cocard, Marc & Santerre, Rock. (2014).
        EPC: Matlab software to estimate Euler pole parameters.
        GPS Solutions. 18. 10.1007/s10291-013-0354-4.
    """
    def deg2rad(d: float) -> float:
        return d * (pi/180)

    def rad2deg(r: float) -> float:
        return r * (180/pi)

    def rad2arcsec(r: float) -> float:
        return rad2deg(r) * 3600

    phi = deg2rad(lat) # rad
    lam = deg2rad(lon) # rad
    omega = deg2rad(rotation) / 1e6 # rad/yr

    Omega = [
        omega * cos(phi) * cos(lam),
        omega * cos(phi) * sin(lam),
        omega * sin(phi),
    ] # rad/yr

    return (rad2arcsec(w) for w in Omega) # arcsec/yr
```
