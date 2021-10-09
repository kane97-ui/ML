PCA: 是一种常见的数据降维方法，其目的是在“信息”损失较小的前提下，将高维的数据转换到低维。
* 样本点在这个超平面的投影都尽可能分开，如果样本在新坐标系下的投影足够近的话，这样将无法对样本进行区分，大量样本的信息都将丢失。
* 用方差来进行衡量样本之间散开的程度
  * $X=(x_1,\cdots,x_n) \in R^{d\times m}$ 
  
  * $W=(w_1,\cdots,w_{d'}) \in R^{d\times d'}$      $W$是基向量，有
   $||w_i||_2=1,w_i^Tw_j=0$
  * $\sigma^2={1\over n}\sum_{i=1}^n(w^Tx_i-\mu)^2
    \\ \quad={1\over n}\sum_{i=1}^n(w^Tx_i-{1\over n}\sum_{i=1}^n w^Tx_i)^2\\
    \quad={1\over n}\sum_{i=1}^n(w^Tx_i-w^T({1\over n}\sum_{i=1}^nx_i))^2\\
    \quad={1\over n}\sum_{i=1}^n(w^T(x_i-\bar{x}))^2\\
    \quad={1\over n}\sum_{i=1}^n(w^T(x_i-\bar{x})(x_i-\bar{x})^Tw)\\
    \quad=w^T[{1\over n}\sum_{i=1}^n(x_i-\bar{x})(x_i-\bar{x})^T]w\\
    \quad=w^T\Sigma w=w^TXX^T w$
* $max_w\qquad W^TXX^T W\\
s.j. \qquad W^TW=I$
* 拉格朗日乘数法构造一个目标函数
  $L(w,\lambda)=W^T\Sigma W+\lambda(1-W^TW)\\
  {\partial L(W,\lambda)\over \partial{W}}=2\Sigma W-2\lambda W$ 令偏导为0，得到
  $\Sigma W=\lambda W$
* $\lambda$ 是 $\Sigma$的特征值,$W$ 是对应的特征向量，我们只要将所有的特征值排序，然后选择前$q$个最大的特征值对应的特征向量。
* 由于$\Sigma$是一个对称矩阵，所以可以得到如下特征分解
   $\Sigma=G\Lambda G^T \in R^{d\times d} \quad G \in R^{d\times d}$
* 我们可以取前d'个列向量来组成$W$
  $W=[g_1 \cdots g_{d'}]$

**SVD**
* $X=UKV^T$
* $\Sigma=XX^T\\
\quad=UKV^TVKU^T\\
\quad=UKK^TU^T\\
\quad=U\Lambda U^T$
$U$则是$\Sigma$特征值分解后的$G$
* 我们可以取前d'个列向量来组成$W$
  $W=[u_1 \cdots u_{d'}]$
* 在数据量很大时求协方差，然后进行特征分解是一个很漫长的过程，因此在PCA背后的是实现也是借助奇异值分解来做的，在这里我们只要求的奇异值分解中的左奇异向量或右奇异向量中的一个（具体是哪个根据X向量的书写方式，左奇异向量用来压缩行数，右奇异向量用来压缩列数）