# KKT

## constrained optimization

**不等式约束 $\Rightarrow$ 等式约束 $\Rightarrow$ 无约束优化**

* 等式约束（lagrange乘子）

$$
minf(x_1,x_2,\cdots,x_n)\\
s.t. h_k(x_1,x_2,\cdots,x_n)=0
$$

我们令**$L(x,\lambda)=f(x)+\sum_{k=1}^l \lambda_kh_k(x)$**  ,$L(x,\lambda)$ 称为lagrange函数，**$\lambda$** 为lagrange乘子，$\lambda$和$x$均看作变量

再联立方程组
$$
\begin{cases}
{\delta L \over \delta x_i}=0 (i=1,2,\cdots,n)\\
{\delta L \over \delta \lambda_k}=0 (i=1,2,\cdots,n)
\end{cases}
$$

* 不等式约束
  $$
  minf(x)\\
  s.t. g_1(x)=a-x \le 0\\
  g_2(x)=x-b\le 0
  $$
  引入松弛变量$a_1,b_1$ 
  $$
  \begin{cases}
  h_1(x,a_1)=g_1(x)+a^2_1=a-x+a^2_1=0\\
  h_2(x,b_1)=g_2(x)+b^2_1=x-b+b^2_1=0\\
  \end{cases}
  $$
  此时为等式约束

  **$L(x,a_1,b_1,\mu_1,\mu_2)=f(x)+\mu_1(a-x+a_1^2)+\mu_2(x-b_1+b_1^2)$** ，此时$x_1,a_1,b_1,\mu_1,\mu_2$ 均看作变量

  按等式约束问题求解
  $$
  \begin{cases}
  {\delta F \over{\delta x}}={\delta f \over \delta x}+\mu_1 {dg_1\over dx}+\mu_2 {dg_2\over dx}={\delta f \over \delta x}-\mu_1 +\mu_2=0\\
  {\delta F \over{\delta \mu_1}}=g_1+a^2_1=0\\
  {\delta F \over{\delta \mu_2}}=g_2+b^2_1=0\\
  {\delta F \over{\delta a_1}}=2\mu_1a_1=0\\
  {\delta F \over{\delta b_1}}=2\mu_1b_1=0\\
  \mu_1 \ge0, \mu_2 \ge0
  
  \end{cases}
  $$
  对于 **$\mu_1a_1=0$** 两种情况

  1. $\mu_1=0, a_1 \neq 0$：此时没有发挥限制作用，但有$g_1(x)=a-x < 0$
  2. $\mu_1 \ge 0, a_1 = 0$：此时发挥限制作用，但有$g_1(x)=a-x = 0$

  **合并情况1和2得：$\mu_1 g_1=0$，**

  **且在约束起作用，$\mu_1>0,g_1(x)=0$,**

  **约束不起作用：$\mu_1=0，g_1(x)<0$,**
  $$
  \begin{cases}
  {df\over dx}+\mu_1 {dg_1\over dx}+\mu_2 {dg_2\over dx}=0\\
  \mu_1g_1(x)=0, u_2g_2(x)=0\\
  \mu_1\ge0,\mu_2\ge0
  \end{cases}
  $$

  * 多元多次不等式
    $$
    minf(x)\\
    s.t. g_j(x)\le  0(j=1,2,\cdots,w)\\
    $$
    我们有
    $$
    {\delta f(x^*) \over x_i}+\sum_{j=1}^m\mu_i{\delta g(x^*)\over \delta_{x^*}}=0\\
    \mu_jg_j(x^*)=0(j=1,2,\cdots,m)\\
    \mu_j\ge0(j=1,2\cdots,m)
    $$
    上式称为不等式的约束优化问题的**KKT**条件，$u_j$ 称为KKT乘子，且约束起作用时$\mu_j \ge0,g_j(x)=0$  且约束不起作用时$\mu_j =0,g_j(x)<0$

  

  * 同时包含等式约束和不等式约束
    $$
    minf(x)\\
    s.t. g_j(x)\le  0(j=1,2,\cdots,w)\\
    h_k(x)=0 (k=1,2,\cdots,l)
    $$
    满足KKT条件
    $$
    {\delta f(x^*) \over \delta x_i}+\sum_{j=1}^m\mu_i {\delta g(x^*)\over \delta{x^*}}+\sum_{k=1}^l \lambda_k {\delta{h_i}\over \delta{x_i}}=0,(i=1,2,\cdots,m)\\
    h_k(x)=0\\
    u_ig_j(x)=0(j=1,2,\cdots,m)\\
    \mu_j\ge0
    $$
    $L(x,\alpha,\beta)=f(x)+\sum_{i=1}^l \lambda_jh_j(x)+\sum_{j=1}^k \mu_ig_j(x)$

    

  * 拉格朗日对偶



# SVM

## 硬间隔

$$
min {1\over 2}||w||^2\\
s.t \quad y_i(w\cdot X_i+b)-1 \ge0, i=1,2,\cdots,N
$$

* 构建拉格朗日函数
  $$
  L(w,b,\alpha)={1\over 2}||w||^2-\sum_{i=1}^N \alpha_iy_i(w\cdot x_i+b)+\sum_{i=1}^N \alpha_i\\
  \alpha=(\alpha_1,\alpha_2,\cdots,\alpha_N)^T 为拉格朗日乘子向量
  $$
  根据拉格朗日对偶性原始问题的对偶问题是极大极小问题：
  $$
  max_\alpha min_{w,b}L(w,b,\alpha)
  $$

  1. 求$min_{w,b}L(w,b,\alpha)$
     $$
     \nabla_w L(w,b,\alpha)=w-\sum_{i=1}^N \alpha_iy_ix_i=0\\
     \nabla_bL(w,b,\alpha)=-\sum_{i=1}^N\alpha_iy_i=0\\
     w=\sum_{i=1}^N \alpha_iy_ix_i \quad\sum_{i=1}{\alpha_i y_i}=0
     $$
     代入拉格朗日函数，即得
     $$
     L(w,b,\alpha)={1\over 2}\sum_{i=1}^N\sum_{j=1}^N\alpha_i\alpha_jy_iy_j(x_ix_j)-\sum_{i=1}^N \alpha_iy_i((\sum_{j=1 }^N\alpha_j\alpha_jx_j)\cdot x_i+b)+\sum_{i=1}^N\alpha_i\\
     =-{1\over 2}\sum_{i=1}^N\sum_{j=1}^N\alpha_i\alpha_jy_iy_j(x_ix_j)+\sum_{i=1}^N\alpha_i
     $$

  2. 求$min_{w,b}L(w,b,\alpha)对\alpha$的极大值，即是对偶问题
     $$
     max_{\alpha}-{1\over 2}\sum_{i=1}^N\sum_{j=1}^N\alpha_i\alpha_jy_iy_j(x_ix_j)+\sum_{i=1}^N\alpha_i\\
     \Rightarrow min_\alpha {1\over 2}\sum_{i=1}^N\sum_{j=1}^N\alpha_i\alpha_jy_iy_j(x_ix_j)-\sum_{i=1}^N\alpha_i\\
     s.t \sum_{i=1}^N \alpha_i y_i=0 \quad \alpha_i \ge0,i=1,2,\cdots,N\\
     KKT condition
     \begin{cases}
     1-y_i(W^TX_i+b)\le0\\
     \alpha_i\ge0\\
     \alpha_i(1-y_i(W^Tx_i+b))=0
     \end{cases}
     $$
     $\alpha>0$的点是支持向量点，求解方式用smo算法

     


## 软间隔

* 构建松弛变量之后转为不等式约束问题

线性不可分意味着某些样本点不能满足间隔大于等于1的约束条件，为了解决这个问题，可以对每个样本点$(x_i,y_i)$ 引进一个松弛变量$\epsilon_i \ge0$ , 所以约束条件变为
$$
y_i(w_i\cdot Xi+b) \ge1-\epsilon_i
$$
同时，对每个松弛变量$\epsilon_i$ 支付一个代价
$$
min_{w,b,\epsilon}{1\over 2}||w||^2+C\sum_{i=1}^N \epsilon_i\\
s.t \quad y_i(w\cdot X_i+b)\ge1-\epsilon_i
$$

* Hinge Loss
  $$
  \sum_{i=1}^Nmax(0,1-y_i(w\cdot x_i+b))+\lambda||w||^2
  $$
  



## 简单讲述一下SVM

1. 首先SVM分为两类：一类是硬间隔：主要应用于线性可分，第二类是软间隔，应用于线性不可分。第三类是引入核函数。
2. 那SVM硬件隔是一个约束优化问题，它的优化目标是最大化两条margin之间的距离，而约束条件是每个点的函数距离与y相乘大于1.
3. 而为了求解该问题，通过引入拉格朗日乘子，以及拉格朗日的对偶性，把问题转化成了无约束的优化问题，且根据KKT条件，可以得到支持向量点
4. 那软间隔的是在硬间隔基础上加了一个松弛变量，从而放宽限制条件，那求解方式的话，一种是和硬间隔一样，那另一种是用hinge loss。那这个hinge loss主要由两部分组成，一部分是w的第二范式的平方，一部分是0和$1-y_i(w\cdot x_i+b)$ 的max函数。其中有一个$\lambda$ 参数，目的是权衡两条margin的距离和尽可能是正负样本区分开这两个问题的占比。
5. 那引入核函数的目的也是针对线性不可分的问题，它的目的是将数据从低纬空间映射到高维空间，从而实现可分，核函数有线性核函数，多项式核函数，高斯核函数。那线性核函数主要解决线性问题，多项式核函数主要解决线性不可分的问题，但对于大数量级的幂数的话，是不适用的。那高斯核函数的话，可以映射到无限维，只有一个参数，相比多项式更容易选择，缺点是可解释性差，计算速度比较慢，容易过拟合。