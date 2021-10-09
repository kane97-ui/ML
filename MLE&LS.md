**Conditional probability and likelihood:**

* conditional probability $Pr[x|y]$
* The probablity of observed data given parameter $\theta$: $Pr[D|\theta]$

**Least squares**
$L(\theta)=||x\theta-y||_2^2\\
\quad\quad =\theta^TX^TX\theta-2\theta^TX^Ty+y^Ty$
* Take the gradient
  $\nabla_\theta L(\theta)=2X^T(X\theta-y)=0$
* setting it to zero
  $X^TX\bar{\theta}=X^Ty$
  * case I:
    $X\in R^{n\times d}$ has full column rank
    $\rArr X^TX \in R^{d\times d}$ is non-singular/invertible
    $\rArr \bar{\theta}=(X^TX)^{-1}X^Ty$
    $X^\psi=(X^TX)^{-1}X^T$
    $X^\psi X=I$
    **closed form solution exists in non-singular case**
  * case II:
   $X\in R^{n\times d}$ does not have full column rank
   $\rArr$ does not have unique solution
   $\rArr$ typically, $n<d$, thus 'overfitting '

**LS for linear regresion: MLE interpretation**
$y_i=x_i^T\theta+\epsilon_i$  with $\epsilon_i \sim N(0,\delta^2) $
Equivalently
$\epsilon=y-X\theta \sim N(0,\sum)$
**The likelihhod:**
$P(D|\theta)={1\over(2\pi)^{n\over2}}{1\over(\sum)^{n\over2}}exp(-{1\over2}\epsilon^T\sum^T\epsilon)
\\\quad\quad\quad={1\over(2\pi)^{n\over2}}{1\over(\sum)^{n\over2}}exp(-{1\over2}\epsilon^T\sum^T(y-X\theta))$

**log—likelihood**
$L(\theta)=log(p(D|\theta))=constant-{1\over2}(y-X\theta)^T\sum^{-1}(y-X\theta)\\
=constant-{1\over{2 \delta^2}}||y-X\theta||_2^2$

**MLE principle：**
$\bar{\theta}_{MLE}=argmin||y-X\theta||_2^2=X^\psi y$
**Least square is maximum likelihood estimator under Gaussian noise**

**How good is this estimator**
$y=X\theta^*+\epsilon$ with $\epsilon \sim N(0,\sum)
$
$\theta^* $ is the target to learn
**MLE or LS solution:**
$\bar{\theta}_{MLE}=X^\psi y=X^\psi(X\theta^*+\epsilon)=X^\psi X\theta^*+X^\psi \epsilon=\theta^*+X^\psi\epsilon$
$E(\bar{\theta})=\theta^*+E(X^\psi \epsilon)=\theta^*$
**Unbiased:** $E[\bar{\theta}_{MLE}]=\theta^*$
**Variance:** $Cov(\bar{\theta}_{MLE})=(X^TX)^{-1}$

