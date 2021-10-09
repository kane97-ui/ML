**Basic notion of Linear algebra**
Inner product:
$<x,y>=X^Ty=\sum_{i=1}^nx_iy_i$
$l_2-norm$:
$||x||_2=\sqrt{x^Tx}= \sqrt{\sum_{i=1}^n x_i^2} $

**Category:**
* supervised learning(labeled data)
* unsupervised learning(unlabled data)
* reinforcement learning(exploitation+exploration)

**Hypotheis/model**
* hypothesis/model space $H$ : is the one of the hard parts to be pre_determined in a learning process
* choose a hypothesis/model $f\in H $:
  $f:x\rarr y$
* Two main categories of hypothesis/model space $H$
  * linear: linear regression/linear classification
  * nonlinear: kernel methods, deep learning

**Perceptron learning algorithm**
* $f_\theta(x)=sign(\theta^Tx)$
* misclassified data:
  $sign(\theta^Tx)\neq y_i$
* learning/update
  $\theta=\theta+y_ix_i$