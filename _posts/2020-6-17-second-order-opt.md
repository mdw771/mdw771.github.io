---
layout: post
title: Second order optimization methods - a quick overview
---

**1. Basics**

Expanding function \\( F(w) \\) at \\( w_k \\) (the solution at the \\( k \\)-th iteration) results in

$$ q_k(w) = F(w_k) +\nabla F(w_k)^T(w - w_k) + \frac{1}{2} (w - w_k)^T \nabla^2 F(w) (w - w_k). \qquad(1)$$ 

Let \\( z = w - w_k \\) be the update vector for the \\( k \\)-th iteration. We want to find the \\( z \\) that minimizes the quadratic approximation of Eq. (1). To do this, we find \\( dq_k / dz \\) to be

$$ dq_k / dz = \nabla F(w) + \nabla^2 F(w) z.  \qquad(2) $$

Setting \\( dq_k / dz = 0 \\), the value of \\( z^* \\) that minimizes \\( F \\) at the vicinity of \\( w_k \\) then satisfies

$$ \nabla^2 F(w_k)z^* = -\nabla F(w_k).  \qquad(3) $$
 
Parameter w should then be updated along vector sk which again is \\( w - w_k \\):

$$ w_{k+1} \leftarrow w_k + \alpha z^*.  \qquad(4) $$
  
For a quadratic function \\( F \\), the Taylor expansion is exact, and the minimum should be reached in one step with \\( \alpha = 1 \\).


**2. Hessian-free inexact Newton methods**

Eq. (3) is solved as an inner iterative cycle using methods like conjugate gradient descent. In doing so, one never explicitly builds the Hessian \\( \nabla^2 F(w_k) \\) but only need to know the Hessian-vector product \\( \nabla^2 F(w_k)z^* \\), and this is why it's called Hessian-free.


**3. Quasi-Newton methods (e.g. BFGS)**

The inverse of the Hessian \\( \nabla^2 F(w_k) \\) is approximated iteratively along with the parameter optimization process. Eq. (4) becomes

$$ w_{k+1} \leftarrow w_k - \alpha H_k^{-1} \nabla F(w_k) \qquad(5) $$
 
where \\( H_k^{-1} \\) tries to approximate \\( (\nabla^2 F(w_k))^{-1} \\). \\( H_k^{-1} \\) is updated following each iteration. For BFGS, it's done as

$$ H_{k+1}^{-1} \leftarrow \left( I - \frac{v_k z^{*T}}{z^{*T} v_k} \right)^T H_k^{-1} \left( I - \frac{v_k z^{*T}}{z^{*T} v_k} \right) + \frac{z^* z^{*T}}{z^{*T} v_k} \qquad(6) $$
  
where \\( I \\) is the identity matrix, and

$$ v_k = \nabla F(w_{k+1}) - \nabla F(w_k). $$

A variant, low-memory BFGS (L-BFGS) doesn't explicitly construct \\( H_k^{-1} \\), but update the product 
\\( H_k^{-1}\nabla F(w_k) \\) directly ([1], Algorithm 6.2).


**4. Gauss-Newton methods**

Gauss-Newton methods approximate the Hessian \\( \nabla^2 F(w_k) \\) in a different way, where the approximation is guaranteed to be positive definite (however, it doesn't account for second-order interactions between elements of \\( w \\) (e.g., \\( \frac{\partial^2 F}{\partial w_i \partial w_j} \\))):

$$ G_k(w_k) = J_h(w_k)^T H_l(w_k)J_h(w_k) \qquad(7) $$
 
where \\( h \\) is the prediction function as in \\( F(w_k) = \|\| h(w_k) - y \|\|^2 \\) if the loss function \\( F(w_k) \\) is least-squares. \\( H_l \\) is the Hessian of the loss function with regards to \\( h \\), and \\( J_h \\) is the Jacobian of \\( h \\) with regards to \\( w \\). The update vector \\( z^* \\) as in Eq. (3) is then

$$ G_k(w_k) z^* = -\nabla F(w_k).  \qquad(8) $$

**4.1. Levenberg algorithm**

Eq. (8) can be solved using conjugate gradient. However, more often \\( G(w_k) \\) in (8) is regularized with a damp factor. This yields the Levenberg algorithm which solves

$$ [G(w_k) - \lambda I] z^* = -\nabla F(w_k). \qquad(9). $$

**4.2. Curveball algorithm**

An alternative way is to go back to (1). Using \\( G(w_k) \\) to approximate the Hessian, (1) becomes

$$ q_k(w) = F(w_k) +\nabla F(w_k)^T(w - w_k) + \frac{1}{2} (w - w_k)^T G(w_k) (w - w_k). $$

and we want to solve for the update vector \\( z^* \\) as

$$ z^* = \mbox{argmin}_z \left[ F(w_k) + \nabla F(w_k)^T z + \frac{1}{2} z^T G(w_k) z \right].  \qquad(10) $$

So far it might appear that we are just restepping along the route introduced following Eq. (1), but now we shall notice that given the iterative nature of non-linear optimization, the update vector of each iteration doesn't have to be very precise. Thus, instead of solving exactly \\( dq_k / dz = 0 \\), we minimize \\( q_k(w_k) \\).

The Curveball algorithm does this by first getting an estimate of \\( z^* \\) using one gradient descent iteration on (10), and then mix it with the old update vector:

$$ z' = G(w_k) z + \nabla F(w)  \qquad(11) $$

$$ z^* = \rho z - \beta z'.  \qquad(12) $$

This results in the Curveball algorithm, which eliminates the need of inverting the Gauss-Newton matrix at each iteration. The regularized version of (11) would be

$$ z' = [G(w_k) + \lambda I] z + \nabla F(w_k).  \qquad(13) $$

Parameters \\( \rho \\) and \\( \beta \\) can be estimated using Eq. 19 of [2].


**5. Natural gradient methods**

The prediction function \\( h(w) \\) dictates the parameters of the probability distribution that the samples are subject to (for example, prediction function of an optical forward model gives the expectation and variance of a Poisson PDF). The difference between two distributions, one determined by \\( h(w_k) \\), the other determined by \\( h(w_{k+1}) \\), is measured by the KL divergence, \\( D[h(w_k) \|\| h(w_{k+1})] \\). Natural gradient methods minimize the loss function \\( F(w) \\) subject to the constraint that \\( D[h(w_k) \|\| h(w_k + dw)] \\) is small ( \\( D \le \eta k \\) ).

On the other hand, the KL divergence \\( D[h(w_k) \|\| h(w_k + dw)] \\) can be approximated by in terms of the Fisher information matrix G(w) as

$$ D[h(w_k) \| h(w_k + dw)] \approx \frac{1}{2} dw^T G(w_k) dw.  \qquad(14) $$

The Fisher information matrix in turn can be approximated as

$$ \hat{G}(w_k) = \left(\frac{\partial \log[h(w_k)]}{\partial w_k}\right) \left(\frac{\partial \log[h(w_k)]}{\partial w_k}\right)^T $$
  
which resembles the Gauss-Newton approximation of the Hessian when the loss function is of a least-squares type.

To minimize \\( F(w) \\) subject to \\( D[h(w_k) \|\| h(w_k + dw)] \le \eta k \\), one can turn the constrained optimization problem into an unconstrained optimization problem using Lagrangian multiplier:

$$ w_{k+1} = \mbox{argmin}_w \left[ \nabla F(w_k)^T (w - w_k) + \frac{1}{2\alpha}(w - w_k)^T G(w_k) (w - w_k) \right] $$
  
and the update relation would then be

$$ w_{k+1} = w_k - \alpha G^{-1}(w_k) \nabla F(w_k). \qquad(17) $$

Comparing (17) with (4), we will notice that the natural gradient method is nearly equivalent to a quasi-Newton method that approximate the Hessian using a Gauss-Newton matrix.


**References**

[1] Bottou, L., Curtis, F. E. & Nocedal, J. Optimization Methods for Large-Scale Machine Learning. (2016).

[2] Henriques, J. F., Ehrhardt, S., Albanie, S. & Vedaldi, A. Small steps and giant leaps: Minimal Newton solvers for Deep Learning. (2018).

