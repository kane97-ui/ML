**Prop: Descent direction**
suppose $L$ is a continuously differentiable, if there exists a $d$ such that 
$\qquad\qquad\nabla L(\theta)d<0$
then，there exists a $\mu>0$ such that
$\qquad\qquad L(\theta+\mu d)<L(\theta)$

**Gradient descent：interpretation**
* The gradient descent
$\theta_{k+1}=\theta_k-\mu_k\nabla L(\theta_k)$
which can be rewritten as
$\theta_{k+1}=argmin_{\theta\in R^d} L(\theta_k)+\nabla L(\theta_k)^T(\theta-\theta_k)+{1\over 2\mu_k}||\theta-\theta_k||_2^2$
* $L(\theta_k)+\nabla L(\theta_k)^T(\theta-\theta_k)$ is a linear approximation of $L$ at $\theta_k$
* ${1\over 2\mu_k}||\theta-\theta_k||_2^2$ is a proximal term: the approximation is a accurately only around $\theta_k$, thus we need the proximal term
  **The proximal term is used to control how far the algorithm goes**

**A very useful algorithm design framework**
Suppose the task is $min_{\theta\in R^d}L(\theta)$, we can design an algorithm as
$\theta_{k+1}=argmin_{\theta\in R^d} \{q_k(\theta)+\nabla{1\over 2\mu_k}||\theta-\theta_k||_2^2\}$
* $q_k(\theta)$ is a linear approximation of $L$ $\rArr$ gradient descent
* $q_k(\theta)$ is a second-order approximation of $L$ $\rArr$ Newton's method
*  $q_k(\theta)$ is $L$ itself $\rArr$ proximal method
*  $q_k(\theta)$ is a single component linear approximation of $L$ $\rArr$ SGD

**Converge of GD**
* Suppose that $L$ is convex and differentialble and has Lipschits continuous gradient with parameter $L$
  $||\nabla L(w)-\nabla L(\mu)||_2\le L||w-\mu||_2$
* converge of gd
  $L(\theta_k)-L(\theta^*)\le{L||\theta_0-\theta^*||^2_2\over 2k}$
* $L(\theta_k)$ converges to $L(\theta^*)$ at the rate of $O(1/k)$

**Exact line search**
$\mu_k=argmin_{\mu\ge 0} L(\theta_k-\mu\nabla L(\theta_k))$
* obtain the most possible descent along direction $-\nabla L(\theta_k)$ 
* solving this univariate optimization problem is often not easy
* leads to inefficient algorithm

**Backtracking line search: Armijo condition**
$L(\theta_k-\mu\nabla L(\theta_k))<L(\theta_k)-\alpha\mu||\nabla L(\theta_k)||_2^2$
* A condition ensuring sufficient decrease

**AGM**
$\theta_{k+1}=w_k-\mu_k\nabla L(w_k)$
$w_k=\theta_k+{k+1\over k+2}(\theta_k -\theta_{k-1})$
* Alternates between gradient updates and proper extrapolation
* Each iteration takes nearly the same cost as GD
* Widely used in practice
* Convergence of AGD with constant stepsize $\mu_k=\mu={1\over L}$ satisfies
    $L(\theta_k)-L(\theta^*)\le{2L||\theta_0-\theta^*||^2_2\over (k+1)^2}$
    * $L(\theta_k)$ converges to $L(\theta^*)$ at the rate of $O(1/k^2)$
    * much faster than GD