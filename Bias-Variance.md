**Start the decomposition**
$\rArr E_S[Er_{out}(f_S)]=E_S[E_x[(f_S(x)-g(x))^2]]=E_x[E_S[(f_S(x)-g(x))^2]]$
* average hypothesis
  $\bar{f}(x)=E_S[f_S(x)]$
* using average hypothesis to decompose
  $E_S[(f_S(x)-g(X))^2]\\
\qquad =E_S[(f_S(x)-\bar{f}(x)+\bar{f}(x)-g(x)]\\
\qquad =E_S[(f_S(x)-\bar{f}(x))^2+(\bar{f}(x)-g(x))^2+2(f_S(x)-\bar{f}(x))(\bar{f}(x)-g(x))]\\
\qquad =E_S[(f_S(x)-\bar{f}(x))^2]+(\bar{f}(x)-g(x))^2$
* Plug the former decomposition
  $E_S[Er_{out}(f_S)]=E_X[E_S[(f_S(x)-g(X))^2]]\\
  \qquad \qquad \qquad =E_x[E_S[(f_S(x)-\bar{f}(x))^2]]+E_x[(\bar{f}(x)-g(x))^2]$
   * $E_x[E_S[(f_S(x)-\bar{f}(x))^2]]$ is variance
   * $E_x[(\bar{f}(x)-g(x))^2]$ is bias

**Overfitting**
* $H$ is more complex than is wanted
* number of training samples increase, overfitting decrease
* noise in data increase, overfitting increase
  * model will fit noise, instead of real data
  * we suppose training data follow the distribution of out sample

**Validation**
* It can help to choose for hyper-parameter which are not automatically determined by the learning algorithm
* The validation is used for model selection
* Use the validation set to from an estimate
  $Er_{in}(f)={1\over k}\sum_{i=1}^k e(f(x_i),y_i)$
  $\rArr$ In expectation
  $E[Er_{val}(f)]={1\over k}\sum_{i=1}^kE[e(f(x_i),y_i)]=Er_{out}(f)$
 $\rArr$ using  Chebyshev's inequality, we have
 $Er_{val}(f)\le Er_{out}(f)+O({1\over \sqrt{k}})$
 **The test data can never influence the training phase in  any way**

**Cross validation: leave one out**
* each time choose one sample as validation sample and average error
* Validation error:$Er_{val}(f'_j)=e(f'_j(x_j),y_j):=e_j$
* Repeat this for all possible choices of $j$ and average
  $Er_{CV}={1\over n}\sum_{j=1}^n e_j$

**Cross validation: leave more out**
* K-fold cross validation: Train $k$ times on $n-{n \over k}$ sample each
* iterate over all 5 choices of validation set and average
* remarks:
  * For k-fold, the estimate depends on the particular choices of partition
  * Several estimates based on different random partitions and then average them
  * Ensure that each of teh set $S_j$ contain training dara from each class in the same propotion as in the full data set

**Data snooping**
if a data set has affected any step in th learning process, its ability to assess the outcome has been compromised.
* leads to serious overfitting
*  can be very subtle
*  many ways to split up