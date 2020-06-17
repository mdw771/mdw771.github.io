(See a better formatted version at: [https://www.evernote.com/l/ASPz1IsCBhtDXrsqgj11-slWkjiLiQA50hk](https://www.evernote.com/l/ASPz1IsCBhtDXrsqgj11-slWkjiLiQA50hk))

**1. Basics**

Expanding function F(w) at w_k results in

qk(w) = F(w_k) + ∇F(w_k)^T(w - w_k) + 1/2 (w - w_k)^T ∇2F(w) (w - w_k).  (1)

Let z = w - w_k, then dqk / dz is

dqk / dz = ∇F(w) + ∇2F(w) z.  (1.5)

Setting dqk / dz = 0, the value of z* = sk that minimizes F at the vicinity of w_k then satisfies

![](https://github.com/mdw771/mdw771.github.io/blob/master/images/20200617/001.png)  (2)
 
Parameter w should then be updated along vector sk which again is w - w_k:

![](https://github.com/mdw771/mdw771.github.io/blob/master/images/20200617/002.png)  (3)
  
For a quadratic function F, the Taylor expansion is exact, and the minimum should be reached in one step with α = 1.


**2. Hessian-free inexact Newton methods**

Eq. (2) is solved as an inner iterative cycle using methods like conjugate gradient descent. In doing so, one never explicitly builds the Hessian ![](https://github.com/mdw771/mdw771.github.io/blob/master/images/20200617/003.png) but only need to know the Hessian-vector product ![](https://github.com/mdw771/mdw771.github.io/blob/master/images/20200617/004.png), and this is why it's called Hessian-free.


**3. Quasi-Newton methods (e.g. BFGS)**

The inverse of the Hessian ![](https://github.com/mdw771/mdw771.github.io/blob/master/images/20200617/005.png) is approximated iteratively along with the parameter optimization process. Eq. (3) becomes

![](https://github.com/mdw771/mdw771.github.io/blob/master/images/20200617/006.png) (4)
 
where H_k tries to approximate 
![](https://github.com/mdw771/mdw771.github.io/blob/master/images/20200617/007.png). H_k is updated following each iteration. For BFGS, it's done as


![](https://github.com/mdw771/mdw771.github.io/blob/master/images/20200617/008.png)  (5)
  
where

![](https://github.com/mdw771/mdw771.github.io/blob/master/images/20200617/009.png).

A variant, low-memory BFGS (L-BFGS) doesn't explicitly construct H_k, but update the product 
![](https://github.com/mdw771/mdw771.github.io/blob/master/images/20200617/010.png) directly ([1], Algorithm 6.2).


**4. Gauss-Newton methods**

Approximate the Hessian ![](https://github.com/mdw771/mdw771.github.io/blob/master/images/20200617/011.png) using a different way, where the approximation is guaranteed to be positive definite. But doesn't account for second-order interactions between elements of w (e.g., ∂2F/∂wiwj).

![](https://github.com/mdw771/mdw771.github.io/blob/master/images/20200617/012.png) (6)
 
where h is the prediction function, and l is the loss. Hl is the Hessian of the l with regards to h, and Jh is the Jacobian of h with regards to w. The update vector sk as in Eq. (2) is then

G(w_k) sk = -∇F(w_k).    (7)

**4.1. Levenberg algorithm**
(7) can be solved using conjugate gradient. However, more often G(w) in (7) is regularized with a damp factor. This yields the Levenberg algorithm which solves

[G(w_k) - λI] sk = -∇F(w_k).    (8)

using CG.

**4.2. Curveball algorithm**
An alternative way is to go back to (1). Using G(w) to approximate the Hessian, (1) becomes

qk(w) = F(w_k) + ∇F(w_k)^T z + 1/2 z^T G(w) z.  (9)

and we want to solve for the update vector sk as

sk = argminz F(w_k) + ∇F(w_k)^Tz + 1/2 z^T G(w) z.  (10)

That is, instead of solving dqk/dz = 0, we minimize q.

The Curveball algorithm does this by first getting an estimate of sk using one gradient descent iteration on (10), and then mix it with the old update vector:

z' = G(w_k) z + ∇F(w)  (11)

sk = ρ z - β z'.  (12)

This results in the Curveball algorithm. The regularized version of (11) would be

z' = [G(w_k) + λI] z + ∇F(w).  (13)

Parameters ρ and β can be estimated using Eq. 19 of [2].


**5. Natural gradient methods**

The prediction function with parameter w, h(w), dictates the parameters of the probability distribution that the samples are subject to (for example, prediction function of an optical forward model gives the expectation and variance of a Poisson PDF). The difference between two distributions, one determined by h(w_k), the other determined by h(w_k+1), is measured by the KL divergence, D[h(w_k) || h(w_k+1)]. Natural gradient methods minimize the loss function F(w) subject to the constraint that D[h(w_k) || h(w_k + dw)] is small (D <= ηk).

On the other hand, the KL divergence D[h(w_k) || h(w_k + dw)] can be approximated by in terms of the Fisher information matrix G(w) as

D[h(w_k) || h(w_k + dw)] ≈ 1/2 dwT G(w) dw.  (14)

The Fisher information matrix in turn can be approximated as

![](https://github.com/mdw771/mdw771.github.io/blob/master/images/20200617/013.png)  (15)
  
which resembles the Gauss-Newton approximation of the Hessian when the loss function is of a least-squares type.

To minimize F(w) subject to D[h(w_k) || h(w_k + dw)] <= ηk, one can turn the constrained optimization problem into an unconstrained optimization problem using Lagrangian multiplier:

![](https://github.com/mdw771/mdw771.github.io/blob/master/images/20200617/014.png)  (16)
  
and the update relation would then be

![](https://github.com/mdw771/mdw771.github.io/blob/master/images/20200617/015.png).  (17)

Comparing (17) with (4), we will notice that the natural gradient method is nearly equivalent to a quasi-Newton method that approximate the Hessian using a Gauss-Newton matrix.


**References**

[1] Bottou, L., Curtis, F. E. & Nocedal, J. Optimization Methods for Large-Scale Machine Learning. (2016).

[2] Henriques, J. F., Ehrhardt, S., Albanie, S. & Vedaldi, A. Small steps and giant leaps: Minimal Newton solvers for Deep Learning. (2018).

