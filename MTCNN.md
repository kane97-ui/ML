**MTCNN**

* 训练图片准备：
  * Positive face数据：图片左上右下坐标和label的IOU>0.65的图片
  * part face数据：图片左上右下坐标和label的0.65>IOU>0.4的图片
  * negative face 数据：图片左上右下坐标和lable的IOU<0.3的图片
  * landmark face数据：图片带有landmark label的图片
  * 原因：训练不同类型，不同人脸占比的图片
* 训练图片是如何来的？
  * P_Net: 随机截取是基于图片实际label进行上下左右微调来截取，进而保障pos、part数据的足够。对每次截取我们会计算IOU，偏移量（截取框相对label框的偏移位置），是否为人脸（one-hot表示），landmark face(5个五官的位置)。所以训练集的label为 16 x 1。通过P_Net输出的偏移量来算出在原图的label框，不同的训练会算出不同的label框。然后通过定位截取框，并算出IOU值。
  * R_Net: 将P_Net 截取的图片转换成24*24的图片大小，重复P_Net的过程
  * O_Net:将R_Net 截取的图片转换成48*48的图片大小，重复R_Net的过程。精准输出。
  训练的时候不用根据IOU进行NMS(非极大值抑制)

train的时候：不做金字塔，根据预先设定的偏移量，直接在原图上随机截取长宽不一的图，然后统一缩放到12*12送入Pnet训练。


现在回到train的时候，在原图上随机截取长宽不一的图片，然后缩放到12*12,就是为了模拟test的时候，那个滑动窗口内12*12的小图。至于为什么把图片数据分成pos part nag ，因为滑动窗口内的小图存在这三种情况，所以train的时候，需要制作相应的数据，去训练Pnet网络。

**Test:**
Test时候，将原图做成金字塔，把图片金字塔送入Pnet做预测，金字塔的图片肯定大于12*12,输入Pnet之后，Pnet网络可以相当于一个12*12的滑动窗口在图片上滑动，每次滑动都会得到一个1*1*16的预测结果，这个思路非常类似于yolo，把图片划分成小格，对每个小格预测人脸框。相当于可以进行多人头的检测。

图片经过Pnet，会得到feature map，注意这里的feature map不是（1，1，16） 因为输入不是12*12的图片，现在的输出是（m,n,16) 代表有 m x n 个输出。通过分类、NMS筛选掉大部分假的候选；然后剩余候选去原图crop图片输入Rnet，再对Rnet的输出筛选掉False、NMS去掉众多的候选；剩余候选再去原图crop出图片再输入到Onet，这个时候就能够输出准确的bbox、landmark坐标了

**损失函数**
每个网络都会有三个损失函数：分类使用的是交叉熵损失函数，bbox回归使用的是平方差损失函数、landmark回归使用的也是平方差损失函数

其实就是coarse to fine 的过程


**NMS（非极大值抑制）**
顾名思义，非极大值抑制就是抑制不是极大值的元素。在目标检测领域里面，可以使用该方法快速去掉重合度很高且标定相对不准确的预测框，但是这种方法对于重合的目标检测不友好。

**Soft-NMS**
对于优化重合目标检测的一种改进方法。核心在于在进行NMS的时候不直接删除被抑制的对象，而是降低其置信度。处理之后在最后统一一个置信度进行统一删除

对于Soft-NMS来说，在进行非极大值抑制的时候，可以结合实际应用场景将部分重合的窗口的置信度与其重合度进行线性或者非线性变换来进行更富有逻辑意义的滤除，而不是简单的降低置信度之后进行统一滤除。
在进行卷积的时候，是否能够使用不同大小的卷积核使得卷积网络更加敏捷，同时保留、突出的特征更多，从而进一步提高算法的精度与效率。

**Prelu**
在MTCNN中，卷积网络采用的激活函数是PRelu，带有参数的带有参数的Relu，相对于Relu滤除负值的做法，PRule对负值进行了添加参数而不是直接滤除，这种做法会给算法带来更多的计算量和更多的过拟合的可能性，但是由于保留了更多的信息，也可能是训练结果拟合性能更好。

训练数据的选择：
在与训练模型上，加入了自己的训练图片。图片是基于课堂现场拍摄的数千张画面进行训练。因为教室大，人脸小，还有光线等问题，原预训练模型的训练集都是没考虑的。

改进: 将Soft——NMS替换 NMS。提高在复杂，重合的人脸检测的成功率。



1. 从监控器截取5K张图片，作为训练集。

2. 对训练集进行label划分

3. 使用MTCNN网络进行训练，MTCNN多层级神经网络，有三个网络，P-net，R-net，O-net 。三个网络是连接的，功能也是差不多的，只不过每个网络所要拟合的信息的权重不太一样。P-net主要专注于判别一张图片是否为人脸，R-net主要专注于一张图片的bbox信息，O-net，主要专注于人脸的五官位置。所以它是一个从简但精的一个逐级拟合的过程。

4. 可以用一个网络来代替吗？

   可以，但效果没那么好。一个网络经过卷积后，所提取的信息还是有限的，他很难同时准确的表达出一个信息 是否是人脸，位置回归信息，人脸五官信息。

5. 网络结构：3-4个隐藏层。P- net无全连接层。R-net和O-net有

6. 图像金字塔：从原图像缩放到12*12

7. 当训练完后，将截取到的图像送入到facenet网络中

8. 网络结构：

9. <img src="/Users/kanghaoyu/Library/Mobile Documents/com~apple~CloudDocs/上岸宝典/ML/截屏2021-08-16 下午2.06.45.png" alt="截屏2021-08-16 下午2.06.45" style="zoom:50%;" />

   新一代的FaceNet采用Inception-ResNet-v2网络，在原有的Google的Inception系列网络的基础上结合了微软的残差网络ResNet思想

10. <img src="/Users/kanghaoyu/Library/Mobile Documents/com~apple~CloudDocs/上岸宝典/DL/截屏2021-08-16 下午12.02.08.png" alt="截屏2021-08-16 下午12.02.08" style="zoom:50%;" />

11.  损失函数

    构造三元组$(x_i^a,x_i^p,x_i^N)$，使得内类距离尽可能小，类内距离$(x_i^a,x_i^p)$尽可能小，类间距离尽可能拉大$(x_i^a,x_i^N)$
    $$
    L=\sum_i^N[||f(x_i^a)-f(x_i^p)||_2^2-||f(x_i^a)-f(x_i^\N)||_2^2+\alpha]_+
    $$
    
