# 正则化

## 推导

* 正则化理解之基于约束条件的最优化

  * 模型的复杂度可以用VC维来衡量，VC维系数与系数W的个数成线性关系，为了控制W的个数，可以在原优化问题中加入一个约束条件。
    $$
    min_wJ(w;x,y)\\
    s.t. ||w||_0\le C
    $$
    $||\cdot||_0$ 范式表示向量中非零元素的个数，该问题是一个NP问题，不易求解，为此我们需要做松弛条件。 用$l1,l2$ 范数来近似$l_0$ 范数。即
    $$
    min_wJ(w;x,y)\\
    s.t. ||w||_1\le C \quad or \quad||w||_2\le C
    $$
    

  * 利用拉格朗日算子法，可将上述带约束条件的最优化问题转换为不带约束的优化问题。
    $$
    L(w,\alpha)=J(w;X,y)+\alpha(||w||_1-C)\\
    L(w,\alpha)=J(w;X,y)+\alpha(||w||_2-C)\\
    $$
    其中$\alpha >0$ , 假设最优解为$\alpha^*$,则对拉格朗日函数求最优解等价于：
    $$
    min_wJ(w;X,y)+\alpha^*||w||_1\\
    min_wJ(w;X,y)+\alpha^*||w||_2
    $$

* 正则化理解之最大后验概率估计

  在最大后验概率估计中，则将$w$ 看作随机变量，则具有某种分布
  $$
  P(w|x,y)={P(w,x,y)\over p(x,y)}={P(X,y|w)P(x)\over p(X,y)} \propto P(y|X,w)P(w)
  $$
  同样取对数有
  $$
  MAP=logP(y|X,w)P(w)=logP(y|X,w)+logP(w)
  $$
  可以看出后验概率函数为在原似然函数基础上加了一项$logP(w)$
  * 假设$w_j$ 的先验分布为0均值的高斯分布，即$w_j \sim N(0,\sigma^2)$ 则有
    $$
    logP(w)=log\prod_jP(w_j)=log\prod_j[{1\over \sqrt{2\pi\sigma}}e^{-{w_j^2\over 2\sigma^2}}]={-1\over 2\sigma^2}\sum_j w_j^2+C^{'}
    $$
    可以看到在高斯分布下$logP(w)$ 的效果等价于在代价函数中增加$l_2$正则项。

  * 若假设$w_j$服从均值为0，参数为$\alpha$ 的拉普拉斯分布，即：
    $$
    logP(w)=log\prod_jP(w_j)=log\prod_j[{1\over \sqrt{2\pi\sigma}}e^{-{|w_j|\over \sigma}}]={-1\over \sigma}\sum_j w_j^2+C^{'}
    $$
    可以看到在拉普拉斯分布下$logP(w)$ 的效果等价于在代价函数中增加$l_1$正则项。

## L1，L2比较

* L2可以直接求导，L1不能（可以用subgradient，在0点取任意值）
* L2一定只有一条最优解，而L1可能存在多个最优解
* L1鲁棒性更强怼异常值更敏感。
* L1输出稀疏，会把不重要的特征置零，而L2则不会
* L1天然的输出稀疏性，把不重要的特征置零，所以它也是一个天然的特征选择器。

## 应用场景

* 理论上讲：参数如果服从高斯分布就用L2，拉普拉斯就用L1
* 假设模型有很多特征，其中不乏相关性特征，可以用L1消除共线性问题（L1可作为特征选择器）
* 在训练样本多的情况下，尝试用L2来防止过拟合（计算更简单）