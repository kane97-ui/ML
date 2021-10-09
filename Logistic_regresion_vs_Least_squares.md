**Logistic regression**

* Posteriori probability
  $Pr[y_i|x_i;\theta]={1\over 1 +exp(-y_i\cdot \theta^Tx_i) }$
* The likelihood of all $y_i$
  $\prod_{i=1}^n Pr[y_i|x_i;\theta]$
* The log-likelihood of all $y_i$
  $\prod_{i=1}^n Pr[y_i|x_i;\theta]=-\sum_{i=1}^nlog(1+exp(-y_i\cdot \theta^Tx_i))$
* Maximum likelihood estimation leads to logistic regression
  $\hat{\theta}=argmin_{\theta \in R^d}-{1\over n}\sum_{i=1}^nlog(1+exp(-y_i\cdot \theta^Tx_i))$

**least squares**

* squared $l_2$-norm loss
  $\hat{\theta}=argmin_{\theta\in {R^d}}||X\theta-y||_2^2$

**optimization**
* Least squares: closed -form solution
* Logistic regression: no closed-form

**Convex function**
* Definition: $L(\alpha\theta+(1-\alpha)w)\le \alpha L(\theta)+(1-\alpha)L(w)$
* First order convexity characterization
  $L(w)\ge L(\theta)+\nabla L(\theta)(w-\theta)$

**CVX examples**
* Linear function: $L(\theta)=\theta^Tx+c$
* least squares: $L(\theta)=||X\theta-y||_2^2$
* Logistic regression: $L(\theta)={1\over n}\sum_{i=1}^nlog(1+exp(-y_i\cdot \theta^Tx_i))$

**The advantege of cvx optimization**
* No local mimimum
* Though usually no closed-form solution, but we have reliable and efficient algorithm to find the global minimum
* almost a technology