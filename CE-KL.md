# CE-KL联系

* 熵：表示一个事件A的自信息量，也就是A包含多少信息
  $$
  S(x)=-\sum_i p(x_i)logp_i(x_i)
  $$

* KL散度：可以用来表示从事件A的角度来看，事件B有多大不同
  $$
  D_{KL}(A||B)=\sum_i P_A(x_i)log({P_A(x_i)\over P_B(x_i)})\\
  \qquad\qquad\qquad\qquad\qquad\qquad\quad=\sum_iP_A(x_i)logP_A(x_i)-P_A(x_i)logP_B(x_i)
  $$

* 交叉熵：可以用来表示事件A的角度来看，如何描述事件B

$$
H(A,B)=-\sum_i P_A(x_i)logP_B(x_i)=D_{KL}(A||B)+S(A)
$$

机器学习的过程就是希望在训练数据上模型学到的分布和真实的分布越接近越好

1. 假设训练数据从总体独立分布采样而来，可以利用最小化训练数据的经验误差来降低模型的误差

2. 模型的分布和真实分布一致性：
   $$
   P(model)\simeq P(real)\\
   P(training)\simeq P(real)\\
   P(model) \simeq P(training)
   $$

3. 最小化$P(model)$与$P(training)$ 的差异等价于最小化这两个分布之间的KL散度
   $$
   minKL(P(training)||P(model))
   $$
   由于训练数据分布固定
   $$
   minKL(P(training)||P(model))=min H(A,B)-S(training)\\
   \qquad\quad=min H(A,B)-C
   $$
   所以优化两个分布之间的KL散度等价于优化两个分布之间的交叉熵。