**AGM**
* $y^{k+1}=x^{k}+\beta_k(x^k+x^{k-1})$
* select a step size $\alpha_k>0$, and set $x^{k+1}=y^{k+1}-\alpha\nabla f(y^{k+1})$
收敛速度：$f(x^k)-f(x^*)\le {2||x^0-x^*|| \over \bar{\alpha}(k+1)^2}$ 所以是二次收敛
* The accelareted gradient method is not a  descent method

**BFGS**
* quasi-Newton Method（拟牛顿法）
  * it is simlar to newton method,but we don't use Hessian matrix 
  * approximate the Hessian by  a suitable and "easier", invertible matrix
  * less memory storage is required
  * the resulting quasi-Newton step is much cheaper
  * we still can guarantee resonable convergence properties
* BFGS
  * initialization: select a symmetric pos. def matrix $H^0 \in R^{n\times n}$
  * compute the quasi-Newton direction $d^k=-H_k\nabla f(x^k)$
  * compute a step size $\alpha_k$ using Armijo line search
  * set $x^{k+1}=x^k+\alpha_k d^k$
  * set $s^k=x^{k+1}-x^{k+1}-x^k$ and $y^k=\nabla f(x^{k+1})-\nabla f(x^k)$. if $(s^k)^Ty^k<0$ set $H_{k+1}=H_k$, otherwise compute(如果 $(s^k)^Ty^k>0$,说明梯度的变化方向和x的变化方向相反，需要更新)
  $H_{k+1}=H_k+{(s^k-H_ky^k){(s^k)^T}+s^k(s^k-H_ky^k)^T \over {{(s^k)^T y^k}}}-{(s^k-H_ky^k){(y^k)^T}\over {{((s^k)^T y^k)^2}}}\cdot s^k(s^k)^T$

 * LBFGS
   * bfgs估计$B_k$和$H_k$时， 如果n很大，是很难计算的，也会很消耗计算资源。
   * 所以我们不需要直接计算出$H_k$，而是直接关注$d^k=-H_k\nabla f(x^k)$
   * 我们会存储$k$ 步的$y$和$k$步的$s$, 去循环计算出$d_k$
   * 但随着迭代次数的增加，这样会变得非常低效， 需要很多的内存存储
   * 那么LBFGS思想是存储最新的m步 


在小数据集上：AGM，LBFGS，BFGS差距不大，但要比普通的GD好一点
在中等数据集上：AGM相对来说好一点，虽然说迭代次数要远大于BFGS和LBFGS（因为AGM不是每一步都是收敛的，他会花费时间探索方向）， 但每次迭代 cpu消耗的时间是远小于另外两个的。因为LBFGS和BFGS都会花大量的时间估算Hessian矩阵
但在大数据集上，三者的效率是不如SGD的，数据集过大会导致每次迭代计算的时间过多（因为一次性会计算所有的数据）。但SGD通过打乱并且分成mini-batch之后，每次迭代的时间很少，对计算资源的消耗也很小。

