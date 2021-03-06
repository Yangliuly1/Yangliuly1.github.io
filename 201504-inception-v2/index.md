# Batch Normalization, Accelerating Deep Network Training by Reducing Internal Covariate Shift

摘要：BN归一化，加速收敛，防止梯度消失。
<!--more-->

# Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift[^01]

## 文献信息
| 信息 | 内容                                                         |
| ---- | ------------------------------------------------------------ |
| 日期 | 2015.04                                                  |
| 作者 | Sergey Ioffe et al. (sioffe@google.com)                                               |
| 机构 | Google Inc                                                      |
| 来源 | arXiv                                                   |
| 链接 | [Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift](https://arxiv.org/abs/1502.03167) |
| 代码 | [Code]()                                                     |

## 个人理解
><strong style="color:red;">问题:</strong> 神经网络训练困难，原因是初始化和内部协变量偏移，SGD参数不容易收敛，易饱和状态。
> 
><strong style="color:red;">方法:</strong> 批归一化；
> 
><strong style="color:red;">结论:</strong> ILSVRC 2012，4.9%top-5验证集错误率和4.8%测试集错误率；
> 
><strong style="color:red;">理解:</strong> 批归一化的作用，加速收敛和防止梯度消失。
> 
><strong style="color:red;">优化：</strong>按批次归一化，受限于每一批次的数据，当批次数据之间样本差异大，可能效果也会很差。
---

## 背景知识

网络收敛方式：1.较低的学习率，但减慢了收敛速度；2.谨慎的参数初始化；3.ReLU代替sigmoid 。

但是在要求低学习率以及比较好的参数初始化情况下，训练神经网络也是非常困难的。

因为训练深度神经网络时，会出现内部协变量偏移(Internal Covariate Shift)，即 **在训练过程中，每层的输入分布因为前层的参数变化而不断变化。** 具体来说，对于一个神经网络，第n层的输入就是第n-1层的输出，在训练过程中，每训练一轮参数就会发生变化，对于一个网络相同的输入，但n-1层的输出却不一样，这就导致第n层的输入也不一样，这个问题就叫做“Internal Covariate Shift”。

1. SGD训练多层网络：总损失是$l=F_{2}(F_{1}(u, \theta _{1}),\theta_{2})$，当$F_{1}(u, \theta _{1})=x$，损失转换为$l=F_{2}(x, \theta _{2})$,则梯度更新是$\theta_{2}=\theta_{2}-\frac{\alpha}{m}\sum_{i=1}^{m}\frac{\partial F_{2}(x_{i},\theta_{2})}{\partial \theta_{2}}$。 当x的分布固定时候，训练$\theta_{2}$是容易收敛的。而当x的分布不断变化时候，$\theta_{2}$需要不断调整去修正x分布的变化带来的影响.
2. 易进入饱和状态：考虑一个激活层$z=g(Wu+b)$，u代表输入层，W是权值矩阵，b是偏置矩阵，g(x)以sigmoid为例，$g(x)=\frac{1}{1+exp(-x)}$，当|x|增加，$g'(x)$趋向于0，称为饱和状态。（梯度消失，模型将缓慢训练）。x受权值W和偏置b以及之前所有层的参数的影响，训练期间前面层参数的变化可能会将x的许多维度移动到非线性的饱和状态并收敛减慢。且这个前层的影响随着网络深度的增加而放大。实际中，饱和问题和导致梯度减小通常利用ReLu、初始化以及小的学习率来解决。

## 原理方法

### 1、批归一化 Batch Normalization

减少内部协变量偏移，通过固定输入x的统计量（均值和方差），众所周知，如果输入白化\归一化，那么网络训练的收敛速度更快。所以在每一层的输入都进行白化处理，从而消除内部协变量带来的偏移问题。

**白化操作--whitened（LeCun et al，1998b）**，对输入进行白化即对输入数据变换成0均值、单位方差的正态分布，一是特征相关性较低；二是特征具有相同的方差。使用白化来进行标准化（normalization），以此来消除internal covariate shift的不良影响，白化操作可以加快收敛，对于深度网络，每个隐层的输出都是下一个隐层的输入，即每个隐层的输入都可以做白化操作。由于whitened需要计算协方差矩阵和它的平方根的逆，而且在每次参数更新后需要对整个训练集进行分析，代价昂贵。

Batch Normalization，在白化的基础上做简化：

- 简化1，单独标准化每个标量特征（每个通道），简单标准化可能改变该层的表达能力，以sigmoid层为例会把输入约束到线性状态。因此我们需要添加新的恒等变换去抵消这个（scale操作）。

- 简化2，mini-batch方式进行normalization，为了数值稳定，  是一个加到方差上的常量。

BN的主要作用就是：

- 加速网络训练
- 防止梯度消失

如果激活函数是sigmoid，对于每个神经元，可以把逐渐向非线性映射的两端饱和区靠拢的输入分布，强行拉回到0均值单位方差的标准正态分布，即激活函数的兴奋区，在sigmoid兴奋区梯度大，即加速网络训练，还防止了梯度消失。基于此，BN对于sigmoid函数作用大，sigmoid函数在区间[-1, 1]中，近似于线性函数。就会降低了模型的表达能力，使得网络近似于一个线性映射，因此加入了scale 和shift。它们的主要作用就是找到一个线性和非线性的平衡点，既能享受非线性较强的表达能力，有可以避免非线性饱和导致网络收敛变慢问题。

> BN优点：
>
> 1. 减少了人为选择参数。在某些情况下可以取消 dropout 和 L2 正则项参数,或者采取更小的 L2 正则项约束参数； 
> 2. 减少了对学习率的要求。现在我们可以使用初始很大的学习率或者选择了较小的学习率，算法也能够快速训练收敛； 
> 3. 破坏原来的数据分布，一定程度上缓解过拟合（防止每批训练中某一个样本经常被挑选到，文献说这个可以提高 1% 的精度）。 
> 4. 减少梯度消失，加快收敛速度，提高训练精度。
>
> BN训练、测试和推理时区别：
>
> - 在训练时：对每一批的训练数据进行归一化，也即用每一批数据的均值和方差，并用指数滑动平均来近似统计整个空间的均值和方差。BN一般要求将训练集完全打乱，并用一个较大的batch值，否则，一个batch的数据无法较好得代表训练集的分布，会影响模型训练的效果。
>
> - 在测试时：比如进行一个样本的预测，就并没有batch的概念，按照BN的公式，当测试样本batch为1的时候，大大降低了泛化能力，而当batch大于1的时候，样本的输出随所处batch变化，存在差异。因此，这个时候用的是指数滑动平均法（EMA）累计得到的“全局”**训练**数据的均值和方差。
>
> - 在推理时：bn看作1x1卷积，$\alpha$是卷积权值，$\beta$是偏差，把bn和前一个卷积融合为新的卷积，减少计算量并加速。
>
> BN参数量：1x1卷积的参数；
> BN与Dropout冲突：
>
> 1. BN和dropout搭配使用时，模型的性能不升反降，因此后续缺省Dropout。但Wide ResNet（WRN）的作者多了一个“心眼”，他发现在很宽的WRN网络里面，在每一个bottleneck的两个conv层之间加上那么一个Dropout，竟然能得到稳定的提升。
> 1. 原因是方差偏移，第一，训练时采用dropout，虽然通过除以(1-p)的方式来使得训练和测试时，每个神经元输入的期望大致相同，但是他们的方差却不一样。第二，BN是采用训练时得到的均值和方差对数据进行归一化的，现在dropout层的方差都不一样，后续变化也不一样。
> 1. 解决方案：针对方差偏移，[Understanding the Disharmony between Dropout and Batch Normalization by Variance Shift](https://arxiv.org/abs/1801.05134)给出了两种解决方案，（1）拒绝方差偏移，只在所有BN层的后面采用dropout层，现在大部分开源的模型，都在网络的中间加了BN，你也就只能在softmax的前一层加加dropout。还有另外一种方法是模型训练完后，固定参数，以测试模式对训练数据求BN的均值和方差，再对测试数据进行归一化，论文证明这种方法优于baseline；（2）dropout原文提出了一种高斯dropout，论文再进一步对高斯dropout进行扩展，提出了一个均匀分布Dropout，这样做带来了一个好处就是这个形式的Dropout（又称为“Uout”）对方差的偏移的敏感度降低了，总得来说就是整体方差偏地没有那么厉害了。可以看得出来实验性能整体上比第一个方案好，这个方法显得更加稳定。

### 2、Inception v2结构

V1到V2的改动：

- 2个3X3代替5X5，网络的最大深度增加9个权重层。同时，参数数目增加了25%，计算量增加了30%左右。目的是类似于VGG，保持相同感受野的同时减少参数和加强非线性的表达能力。
- 28X28modules从2个增加到3个。
- 在modules中，pooling有时average ，有时maximum。
- 没有across board pooling layers在任意两个inception modules。只在3c，4e里会有stride-2的卷积和pooling。

## 训练测试

略。

## 参考文献

[^01]: [李翔-大白话《Understanding the Disharmony between Dropout and Batch Normalization by Variance Shift》-知乎](https://zhuanlan.zhihu.com/p/33101420)


