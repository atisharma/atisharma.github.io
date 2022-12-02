---
layout: post
title: Discretising the continuous resolvent problem whilst approximating in the right norm
categories: [fluids, resolvent, tutorial]
---

I sometimes get emails asking how to form the weighing matrices during the discretisation of the resolvent problem, such as this nicely put one.

> In particular, (in [your notes](https://arxiv.org/abs/1909.04515)) you remark that prior to conducting the Schmidt decomposition of the resolvent, it is required to "*preserve the appropriate inner product in the form of whatever mass matrix, grid weighting or similar is appropriate*".
>
> As I understand, this just means that a weighting matrix $$\mathbf{Q}$$ must be constructed, which is later applied towards the singular value decomposition of the *weighted* resolvent $$ \mathbf{W} \mathbf{R} \mathbf{W}^{-1} $$.
> Here, of course, $$\mathbf{R}$$ is the "original", unweighted resolvent, and $$\mathbf{W}$$ satisfies the Cholesky decomposition of $$ \mathbf{Q} \equiv \mathbf{W}^*\mathbf{W} $$.
>
> I have noticed this same statement in many papers on the resolvent formulation but I have yet to recover any literature that outlines this step explicitly. I was wondering if you had any papers/articles that could potentially describe this portion of the analysis a bit rigorously.

This question arises when you have the state described by a function (a field) and you want to do a resolvent computation with a discretisation. It's not conceptually difficult, but it's a surprisingly common source of confusion. So I will describe what is going on here in as much detail as I can.


## Resolvent SVD, continuous setting  / Schmidt decomposition

We start having already set up the resolvent problem from the system's dynamics (I'll write this up later).

The state is described by the vector-valued state function $$q(x, s) \in Q$$ and driven by the vector-valued  forcing function $$f(x, s) \in F$$. Here, $$x$$ is a point in $$X \subset \mathbb{R}^D$$ (the spatial domain) and $$s$$ is a complex scalar. Physically, imaginary $$s$$ corresponds to a harmonic forcing and response.

Equip $$Q$$ with an inner product

$$
    \left<a, b\right>_Q := \int_X a(x)^* b(x) \mu_Q(x) ~dx
$$

where $$\mu_Q$$ is some weight function that you choose,
and do similarly for $$F$$. The "energy" of the state at a particular $$s$$ is then its $$\mathcal{L}^2$$ norm, $$\left<q(s), q(s)\right>_Q$$.

The resolvent of operator $$L$$ is the operator $$R(s)$$ relating $$f(s)$$ and $$q(s)$$,

$$
    q(s) = R(s) f(s), \quad R(s) := \left( s I - L \right)^{-1}.
$$

We want the left and right singular vectors of the resolvent operator, so we do the [Schmidt decomposition](/tutorial/linear-algebra/svd/) of the resolvent operator,

$$
    Rx = \sum_{n=1}^\infty \sigma_n \left< x, \phi_n \right>_F \psi_n
$$

giving the $$\phi_n$$ and the $$\psi_n$$ as the left and right singular vectors respectively. The $$\phi_n$$ form an orthonormal basis in $$F$$ (as do the $$\psi_n$$ in $$Q$$). The singular values are real, positive and ordered etc.


## Resolvent SVD, discrete setting 

The problem is almost the same in the discrete setting. The key is to link the two problems via the inner products. 

In the discrete setting, let the counterpart of the continuous system be some discretisation so that the vector $$\mathbf{q}(s) \in \mathbb{R}^N$$ corresponds to $$q(x, s)$$ etc.

To be a meaningful discretisation, its $$\mathcal{l}^2$$ (discrete) norm needs to approximate the $$\mathcal{L}^2$$ (continuous) one,

$$
    \left<a, a\right>_Q \simeq \mathbf{a}^* \mathbf{Q} \mathbf{a}
$$

and the approximation should converge as the fidelity of the discretisation is increased (and similarly for $$F$$).

How you construct $$\mathbf{Q}$$ depends on the specifics of your discretisation scheme. Generally, though, it needs to be Hermitian ($$\mathbf{Q} = \mathbf{Q}^*$$). One [example of such quadrature](https://github.com/mluhar/resolvent/blob/modular/clencurt.m) can be seen for [pipe flow](https://github.com/mluhar/resolvent)

Remember that the SVD provides a left (right) basis --- the columns of $$\mathbf{U}$$ ($$\mathbf{V}$$) --- that is orthogonal and optimal under the simple vector inner product,

$$
    \mathbf{M} = \mathbf{U}\mathbf{\Sigma} \mathbf{V}^*, \quad \mathbf{u}^*_j\mathbf{u}_k=\delta_{jk}, \quad \mathbf{v}^*_j\mathbf{v}_k=\delta_{jk}.
$$

The "unweighted" resolvent is the $$\mathbf{R}$$ in

$$
    \mathbf{q}(s) = \mathbf{R}(s) \mathbf{f}(s)
$$

so doesn't take into account any weighting special to our discretisation ($$\mathbf{Q}$$).
This would give singular vectors that are orthogonal under the wrong inner product. Recalling the meaning of the singular values, it would also mean that the decomposition would be optimal under the wrong inner product (i.e. wrong).

Instead, with the appropriate $$\mathbf{Q}$$, we can form a realisation in which the "natural" inner product is the correct one. The [Cholesky decomposition](https://en.wikipedia.org/wiki/Cholesky_decomposition) of $$\mathbf{Q}$$ is
$$ \mathbf{Q} = \mathbf{W}_Q^* \mathbf{W}_Q $$. Using this (and similarly $$\mathbf{W}_f$$), we have

$$
    \left<a, a\right>_Q \simeq \mathbf{a}^* \mathbf{Q} \mathbf{a} = \mathbf{a}^* \mathbf{W}_Q^* \mathbf{W}_Q \mathbf{a}
$$

and we can rewrite the discrete resolvent as 

$$
    \mathbf{W}_q \mathbf{q}(s) = \left[ \mathbf{W}_q \mathbf{R}(s) \mathbf{W}_f^{-1} \right] \mathbf{W}_f \mathbf{f}(s) = \tilde{\mathbf{q}}(s) \mathbf{R}_W(s) \tilde{\mathbf{f}}(s).
$$

The weighting in $$\mathbf{R}_W(s)$$ induces a singular value decomposition in which the singular vectors are orthogonal with the desired inner product, and optimal in the right "energy".
To see this, recall the leading singular value is a solution to an optimisation problem, and that its discretisation looks like

$$
    \sigma_1^2(s) = \max_{f(s)} \frac{ \| q(s) \|_Q^2 }{ \| f(s) \|_F^2 } \simeq \max_{\mathbf{f}(s)} \frac{ \| \tilde{\mathbf{q}}(s) \|^2 }{ \| \tilde{\mathbf{f}}(s) \|^2 }
$$

where we use

$$
    \|a\|_Q^2 \equiv \left< a, a \right>_Q, \quad \| \mathbf{a} \|^2 = \mathbf{a}^* \mathbf{a}.
$$

The "natural" modes (with point values corresponding to the discretisation, but optimal under the correct inner product) can be recovered via the similarity transformation $$\mathbf{W}_Q \mathbf{q}(s) = \tilde{\mathbf{q}}(s)$$ and so on.
