---
layout: post
title: Discretisation of flow on a sphere
categories: fluids
---

These are some quick notes on the discretisation of flow on a sphere presented in [LN-JFM-2022], written with a view to coding it up at some point.


## Coordinate system

<table>
  <tbody>
    <tr>
      <td>$$a$$</td>
      <td>radius</td>
    </tr>
    <tr>
      <td>$$z$$</td>
      <td>polar axis</td>
    </tr>
    <tr>
      <td>$$\mathbf{r}$$</td>
      <td>vector from the sphere's centre</td>
    </tr>
    <tr>
      <td>$$\theta$$</td>
      <td>colatitude (zero at zenith)</td>
    </tr>
    <tr>
      <td>$$\phi$$</td>
      <td>longitude</td>
    </tr>
    <tr>
      <td>$$\psi(\theta, \phi)$$</td>
      <td>a real-valued scalar function</td>
    </tr>
  </tbody>
</table>


## Expansions

Expansion of $$\phi$$,
\\[
    \psi(\theta, \phi) = \sum_{l=0}^\infty \psi_l(\theta, \phi),
\\]
\\[
    \psi_l(\theta, \phi) = \sum^l_{m=-1} a_{lm}Y^m_l(\theta, \phi).
\\]

Spherical harmonics
\\[ Y_l^m(\theta, \phi) = \sqrt{ (2l + 1) \frac{(l-m)!}{(l+m)!} } P_l^m(\cos \theta ) e^{i\phi}, \\]
where $$P_l^m$$ are the Legendre functions.


## Properties

Because $$\psi$$ is real,
$$a_{l(-m)} = (-1)^m (a_{lm})^*$$
(so the $$\psi_l$$ are also real).

$$Y_l^m$$ are eigenfunctions of the Laplacian,
\\[
    \nabla^2 Y_l^m = - \frac{l (l+1)}{a^2} Y_l^m.
\\]

They are orthogonal wrt to integration over the sphere (as you would expect of eigenfunctions of a Laplacian),
\\[
    \frac{1}{4\pi} = \int_\Omega Y_l^m Y_s^{p*} d\Omega = \delta_{ls} \delta_{mp}
\\]

and have symmetry
\\[
    Y_l^{-m} = (-1)^m Y_l^{m*}.
\\]

The $$\psi_l$$ are invariant under change of coordinate system.


## Streamfunction definition of the velocity field

Define $$\nabla$$ to give the surface gradient along a vector tangent to the sphere.
Introduce the [Hodge star](https://en.wikipedia.org/wiki/Hodge_star_operator) operator, $$\star$$.
In this case the $$\star$$ rotates the gradient anti-clockwise by $$\pi/2$$ in the tangent plane (around $$\mathbf{r}$$).

Then,
\\[
    \mathbf{u} := \star \nabla \psi.
\\]

The velocity field is then divergence-free, according to
\\[
    \nabla \cdot \mathbf{u} = \nabla \cdot \star \nabla \psi = 0.
\\]

The vorticity is defined as
\\[
    \omega := \nabla \cdot \star \mathbf{u} = - \nabla^2 \psi.
\\]

Then
\\[
    \mathbf{u} = \sum_{l=1}^\infty \mathbf{u}_l, \quad \mathbf{u}_l = \star \nabla \psi_l,
\\]

\\[
    \omega = \sum_{l=1}^\infty \omega_l, \quad \omega_l = \frac{l (l+1)}{a^2} \psi_l.
\\]


### Navier-Stokes on a sphere

\\[
    \partial_t \mathbf{u} = - \mathbf{u}\cdot\nabla\mathbf{u} - \nabla(p/\rho) - (2\Omega \cdot \mathbf{e}_r) \star \mathbf{u} + \nu \left( \Delta \mathbf{u} + \frac{\mathbf{u}}{a^2} \right),
\\]

\\[
    \Delta \mathbf{u} = - \star \nabla (\nabla \cdot \star \mathbf{u}) + \frac{\mathbf{u}}{a^2}.
\\]


### To do

* time evolution of expansion coefficients
* efficient transformation between physical and harmonic coordinates


[LN-JFM-2022]: https://doi.org/10.1017/jfm.2021.1130
