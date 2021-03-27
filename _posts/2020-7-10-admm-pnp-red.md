---
layout: post
title: Plug-and-play prior, ADMM, Regularizer as Denoiser (RED), and MACE
---

## ADMM

Alternating direction method of multipliers (ADMM) is a meta-algorithm for
numerical optimization, which is featured by subdividing a large problem
into several smaller subproblems.

We take for example the problem of retrieving the phase of an image at the same
time of suppressing its noise. A typical way to formulate the problem is to
build a composite loss function, where the first term measures the mismatch
between the predicted and measured intensity, and the second term serves as
a regularizer quantifying the noise level:

$$ \operatorname{argmin}_{x} l(y, x) + \lambda\rho(x). $$

In PnP + ADMM, one would split this global optimization problem into 2
subproblems: the phase retrieval subproblem, and the denoising subproblem.
One would also use a different variable \\( v \\) as the unknown of the denoising
subproblem. Since we are solving for a **single** image which is **both** phase
retrieved **and** denoised, we want \\( x \\) and \\( v \\) to be equal when
the computation converges:

$$ \operatorname{argmin}_{x, v} l(y, x) + \lambda\rho(v)~~{s.t.}~x = v. $$

In ADMM, one deals with this constrained optimization problem
by turning it into an unconstrained optimization problem of

$$ \operatorname{argmin}_{x, v} l(y, x) + \lambda\rho(v) + \frac{\beta}{2}\|{x - v + u}\|^2 $$

where \\( u \\) is also known as the dual variable, functioning as a communicator
to keep both subproblems mutually informed of each other.

Quoting the paper of Romano *et al.*, the optimization is done through the following:

Step 1 - Update of \\( x \\):

$$ \hat{x} = \operatorname{argmin}_{x} l(y, x) + \frac{\beta}{2}\|{x - v + u}\|^2. $$

Step 2 - Update of \\( v \\):

$$ \hat{v} = \operatorname{argmin}_{v} \lambda\rho(v) + \frac{\beta}{2}\|{x - v + u}\|^2. $$

As Romano *et al.* explain about this step:
> This stage is nothing but a denoising of the image \\( x + u \\), assumed to be
contaminated by a white additive Gaussian noise of power \\( \sigma^2 = 1 / \beta \\).
... (it is suggested) to replace the direct solution of the equation above by
activating an image denoising engine of choice. This way, we do not need to define
explicitly the regularization \\( \rho(\cdot) \\) to be used, as it is implied by
the engine chosen.

Step 3 - Update of \\( u \\):

$$ \hat{u} = u + x - v. $$

## Plug-and-play

Plug-and-play (PnP) refers to a technique where, after one formulates an optimization
problem under the ADMM framework, can freely replace the solver for a certain
one of its subproblems, and experiment its efficacy.

Remember the quote from Romano *et al.* we've read above?

> ... (it is suggested) to replace the direct solution of the equation above by
activating an image denoising engine of choice. This way, we do not need to define
explicitly the regularization \\( \rho(\cdot) \\) to be used, as it is implied by
the engine chosen.

That is, we can complete the regularizer optimization (denoising) step with a
denoiser that does not have an explicit minimizable form (e.g., a CNN black
box or BM3D). This is the idea of PnP + ADMM: if we have a black-box-like
denoiser \\( f(x) \\), we may replace the entire Step 2 of ADMM with a simple
application of that denoiser on \\( x + u \\), *i.e.*,

$$ \hat{v} = f(x + u). $$

## Regularizer as Denoiser

Although PnP is convenient, simply inserting a denoiser may hurt the convergence
of the overall algorithm.
Regularizer as Denoiser (RED) tries to solve this by creating a **minimizable** form
of the regularizer, no matter how it is done. If the black box denoiser is
f(x), then the regularization term is written as

$$ \rho(x) = \frac{1}{2}x^T[x - f(x)]. $$

RED further introduces a **local homogeneity** assumption of

$$ \nabla_x f(x)x = f(x) $$

The meaning of this is that for a \\( c \\) satisfying
\\( |c - 1| < \epsilon \\), \\( f(cx) \approx cf(x) \\). The approximation
is proven by Romano *et al.* to be satisfied for many common types of
denoisers. Then, the gradient of the regularizer depends on \\( f(x) \\) itself
instead of \\( \nabla f(x) \\):

$$ \nabla\rho(x) = \lambda[x - f(x)]. $$

Then, the denoising step can be again evaluated explicitly:

$$ \hat{v} = \operatorname{argmin}_{v} \frac{\lambda}{2}v^T[v - f(v)] + \frac{\beta}{2}\|{x - v + u}\|^2. $$

## MACE

Multi-agent consensus equilibrium (MACE) is a meta-algorithm that is on a lower
level of abstraction compared to ADMM. The philosophy of MACE is that one can
formulate a problem as the combination of \\( P \\) subproblems, whose unknowns
are \\( v_1, v_2, \cdots, v_P \\), and whose solvers are
\\( f_1, f_2, \cdots, f_P \\). Each subproblem is also assigned a weight,
\\( \mu_1, \mu_2, \cdots, \mu_P \\). By treating MACE as a fixed-point
problem, one can find the set of solutions, \\( v^* \\), such that the
weighted average of \\( v^*_i \\) for all subproblems stabilizes --
that is, all subproblems now reach a "consensus equilibrium".
This weighted average is then the global solution of MACE.

The following figure is taken from the presentation of Charles Bouman
given in SIAM Imaging Science 2020.

![MACE](https://github.com/mdw771/mdw771.github.io/raw/master/images/20200710/mace.png "MACE")

For more details, please refer to the paper by Buzzard *et al*.


## References

[1] Y. Romano, M. Elad, P. Milanfar, The Little Engine that Could: Regularization by Denoising (RED). *Arxiv* (2016).

[2] C. A. Bouman, SIAM Imaging Science Keynote: Plug and Play for Model Fusion. [https://www.youtube.com/watch?v=GjCmxTqAJDo](https://www.youtube.com/watch?v=GjCmxTqAJDo).

[3] G. T. Buzzard, S. H. Chan, S. Sreehari, C. A. Bouman, Plug-and-Play Unplugged: Optimization Free Reconstruction using Consensus Equilibrium. *Arxiv* (2017).
