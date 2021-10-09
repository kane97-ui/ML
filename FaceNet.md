**Triplet**
		为了使类内距离更小，类间距离更大，如何选择合适的triplet显得格外重要。因此，训练中我们尽可能选择两种困难样本对（hard negative pair 和 hard positive pair）。在n张样本（包含同一个人和不同人的）的集合里，总共可能出现n2种比较。对于每张训练样本，首先可以考虑从一个ID的自身样本集中找出与当前训练样本最不像的样本形成hard positive pair；然后与其他类的所有样本比对，找出最像的样本，与之形成hard negative pair。

​		当然，对于大量训练样本集，每次训练要在完整数据集上计算argmin和argmax是不可能的，而且容易被样本分布左右训练结果。因此，这里设置可以在每N步过后，使用最近生成的网络，计算子集里的两两差异，生成一些triplet，再进行下一步训练。也可以设置mini-batch，这个mini-batch固定的同一类数量分布（比如都为40张）和随机负样本，用在线生成和选取mini-batch里的hard pos/neg 样例。这里hard negative pair可以选择mini-batch里的所有同类形成的正样本对，而不一定是最困难那对。Batch大小会影响梯度收敛，但是太小batch对选择triplet不利。经过多次实验，这里batch size设置为1800。