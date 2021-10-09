**Law of large numbers:**
if we have $X_1,X_2,\cdots,X_n$ are i.i.d.,then
${1\over n}\sum_{i=1}^nX_i \rArr E[X]$
This is asmptotic bound.

**Concentration inequalities using method**
* Markov's inequality
  $Pr[X\ge t]\le {E[X]\over t} \qquad \forall t>0$ 

* Chebyshevs inequality：extension
$Pr[|X-E[X]\ge t]\le {var(x)\over t^2} \qquad \forall t>0$
  *  suppose$X_1,X_2,X_3,\cdots,X_n$ are i.i.d. random variables with zero mean
  $Var(\bar{X})={Var(X)\over n}$
  Applying Chebysheves inequality, for any $t\geq 0$, we have
  $Pr[|\bar{X}-E[X]\ge t]\le {var(x)\over nt^2}$
  **large n or samll variance imply better concentration**

**Concentration inequalities for sub_Gaussian**
* MGF(Moment generating function)
  $M_X(\lambda)=E[exp(\lambda X)]$
  * normal distribution $X \sim N(0,\sigma^2)$
    $M_X(\lambda)=exp({\lambda^2 \sigma^2 \over 2})$
  * Rademacher random variable:$Pr[\epsilon=1]={1\over2}\quad and \quad Pr[\epsilon=-1]={1\over2}$, in this case $\sigma^2=1$
  $M_\epsilon(\lambda)\le exp({\lambda^2 \over 2})$

* Chernoff bounds
  $Pr[X-E[X] \ge t]=Pr[e^{\lambda(X-E[X])} \ge e^{\lambda t}]\le {E[e^\lambda(X-E[X]) \over e^{\lambda t}}$(Applying the Markov's inequality)
  $\rArr Pr[X-E[X] \ge t] \le min_{\lambda\ge 0}E[e^{\lambda(X-E[X])}]e^{-\lambda t}$
* sub-Gaussian random variable
  $ E[e^{\lambda(x-\mu)}]\le e^{({{\lambda^2\sigma^2}\over 2})}$
  * Gaussian distribution
  * Rademacher random variable
  * any bounded random variable
* sub-Gaussian concentration
  $Pr[X-E[X] \ge t]\le2e^{-{t^2 \over 2\sigma^2}}$

**Hoeffding's inequality**
$Pr[\sum_{i=1}^n(X_i-\mu_i)\ge t]\le e^{-{t^2}\over 2\sum_{i=1}^n \sigma^2_i} $
for bounded with mean $\mu_i$ and bounded on $[a_i,b_i]$
$Pr[\sum_{i=1}^n(X_i-\mu_i)\ge t]\le e^{-{2t^2}\over \sum_{i=1}^n (b_i-a_i)^2} $


**Generalization**
$Er_{out}=Er_{out}-Er_{in}+Er_{in}$
generalization:$Er_{out}-Er_{in}$(less complex model/hypothesis $H$)
training: $Er_{in}$(more complex model/hypothesis $H$)
Theory of generalization: bounding the generalization error

**In-sample error vs out-of-sample error**
* In expection of fixed $f\in H$
  $Er_{in}(f)=E_{s\sim D}[Er_{in}(f)]$
  * $Er_{in}$ is unbiased estimator for $Er_{out}$
  * Law of large numbers:
    when $n\rarr \infin$, we have consistency

**generalization for fixed model $f$: a lemma**
* high probability bounds:
  $Pr[|Er_{in}(f)-Er_{out}(f)|\ge t]\le 2e^{-2nt^2}$
  $Pr[|Er_{in}(f)-Er_{out}(f)|\le t]\le 1-2e^{-2nt^2}$
  (proved by Hoeffding's inequality for bounded random variables)
* Generalization bound-fixed $f$
  $Er_{out}(f)\le Er_{in}(f)+\sqrt{{log({2\over \sigma}) \over 2n}}$ (with probability at least $1-\sigma \quad \forall \sigma >0$)

**generalization bound for finite model space**
$\forall f\in H \quad Er_{out}(f)\le Er_{in}(f)+\sqrt{log|H|+log{2\over \sigma}\over {2n}}$
* on the training side, we need: more complex model/hypothesis H
* on the generalization side, we need: less complex model/hypothesis H
  

**Empirical Radmacher complexity**
let $S=\{{z_i=(x_i,y_i)\}}_{i=1}^n$
$\hat{R_s}(H):=E_\epsilon[sup_{f\in H}{1\over n}\sum_{i=1}^n\epsilon_i f(x_i)]$（$\epsilon_i$ is a Rademacher random variable)
* Rademacher complexity
The Rademacher complexity of $H$ over the sample $S$ woith respect to the distribution $D$ is defined as:
$R(H):=E_{s\sim i.i.d.D}[\hat{R_{s}}(H)]$
* Rademacher complexity: interpretation
  $R(H)=E_{S,\epsilon}[sup_{f\in H}{\epsilon^Te_S \over n}]$($e_S$ is the vector of $f(x)$)
  $\epsilon^T e_S=||\epsilon||\cdot ||e_S||\cdot cos(\alpha)$
  **more complex $H$ can generate more vectors $e_S$,thus have better chance to correlate teh random noise $\epsilon$, on average.**

**Generalization bound using Rademacher complexity**
$\forall f\in H \quad Er_{out}(f)\le Er_{in}(f)+R(H)+\sqrt{log{1\over\sigma}\over{2n}}$

**McDiarmid's inequality**
let $S=\{z_1,\cdots,z_i,\cdots,z_n\}$
Suppose that $|h(z_1,\cdots,z_i,\cdots,z_n)-h(z_1,\cdots,z_i',\cdots,z_n)|\le c_i$
we have $Pr[h(S)-E[h(S)]\ge t]\le e^{2t^2\over \sum_{i=1}^n c_i^2}$
* Look at the supremum of generalization error:
  $h(S):=sup_{f\in H}[Er_{out}(f)-Er_{in}(f;S)]$
  $h(S')-h(S)\le{1\over n}$
* Apply McDiarmid's inequality to $h(S)$, we have
  $Pr[h(S)-E[h(S)]\ge t]\le e^{-2nt^2}$
  set $\sigma=e^{-2nt^2}$
  $Er_{out}(f)\le Er_{in}(f)+E[h(S)]+\sqrt{log({1\over\sigma})\over 2n}$
  $E[h(S)]:=2R(L)=R(H)$

**Growth function**
$\forall n\in N \quad G_H(n)=max_{\{x_1,\cdots,x_n\}}|\{f(x_1),\cdots,f(x_n)\}:f\in H|$
* $G_H(n)$ counts the most dichotomies that can possibly generated on $n$ points in $X$
* measure the richness of the hypothesis set $H$
* combinatorial concept, independent of distribution $D$
* Generalization bound using growth function
  $\forall f\in H \quad Er_{out}(f)\le Er_{in}(f)+\sqrt{2logG_H(n)\over n}+\sqrt{log{1\over\sigma}\over{2n}}$

**VC-Dimension**
* The VC-dimension of a hypothesis set $H$ is the size of teh largest dataset that can be shattered by $H$ :
$VCdim(H):=max\{ n:G_H(n)=2^n\}$
* let the hypothesis set $H$ be a hyperplane in $R^d$, then
  $VCdim(H)=d+1$
* generalization bound:
  $\forall f\in H \quad Er_{out}(f)\le Er_{in}(f)+\sqrt{2dlog{e\cdot n\over d}\over n}+\sqrt{log{1\over\sigma}\over{2n}}$
  $\forall f\in H \quad Er_{out}(f)\le Er_{in}(f)+O(\sqrt{d\over n})$
