**$l_2$-retularization/ridge regression.weight decay**
* $l_2$-regularized problem is:
  $\hat{\theta}=argmin||X\theta-y||_2^2+\lambda||\theta||^2_2$
  * $\lambda$ controls the trade-off between underfitting and overfitting
  * too large $\lambda$ results in underfitting
  * too small $\lambda$ leads to overfitting
  * close-form:$\hat{\theta}=(X^TX+\lambda I)^{-1}X^Ty$
  * for suitable choice of $\lambda$, always invertible
  

**$l_1$-regularization/LASSO**
$||\theta||_1=\sum_{i=1}^d|\theta_i|$

* promotes sparsity(sparsity plays a very important role in many applications)
* not differentiable
* it can prevent overfitting when $n<d$ as we indeed has much less parameters than $d$ 
* Proximal gradient descent for LASSO:
$\theta_{k+1}=argmin_{\theta\in R^d} \{g(\theta_k)+\nabla g(\theta_k)^T(\theta-\theta_k)+\lambda||\theta||_1+{1\over 2\mu_k}||\theta-\theta_k||_2^2\}$
* finally we have
  $$
\theta_{k+1}[i]=argmin{1\over{2\mu_k}(\theta_i-\alpha)^2}=
\begin{cases}
\alpha_i-\lambda\mu_k,\quad \alpha\geq \lambda\mu_k\\\\
0, \quad -\lambda\mu_k\le\alpha\leq \lambda\mu_k\\\\
\alpha_i+\lambda\mu_k,\quad \alpha\leq -\lambda\mu_k
\end{cases}
$$
where $\alpha=\theta_k-\mu_k\nabla g(\theta_k)$

