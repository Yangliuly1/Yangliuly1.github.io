# 图像处理 0x 05：图像分割

摘要：图像分割是分离图像中前景和背景（含噪声干扰），提取需要的目标物体。常见的图像分割包括：基于阈值的图像分割（直方图、颜色），车牌识别就常利用颜色阈值提取车牌的ROI区域；基于机器学习的的图像分割（Logistic、K-means、GMM），K-means以像素相似度划分像素为K类；基于边缘的图像分割，识别像素的急剧变化或不连续的地方，如不同颜色的渐变区域。基于区域生长的分水岭算法。
具体问题需要具体分析，难点一是环境变化，光照变化，这种可能需要自适应的阈值分割；难点二是前景与背景的对比度小，暂时无法用一种有效的算法彻底解决该问题，目前在工业界的处理方式是硬件上更好的打光方案，突出前景与背景的差异，减小算法的难度，算法上采用图像增强方式突出前景与背景差异，然后采用高阶梯度差异去搜索最优的阈值变化范围边界。

灰度、颜色、纹理、形状
<!--more-->
# 图像处理 0x 05：图像分割

## 原理与特性

图像阈值分割，其主要思想是对像素进行分类。

图像阈值分割的适用性取决于前景和背景的像素差异程度，更学术点的说法是像素对比度，对比度差异越大，阈值分割效果越好，反之效果变差。

图像二值化：常常需要分离前景和背景，相当于像素二分类操作，因此也称其为图像二值化。

- 二值图像就是只有黑白两种颜色表示的图像，其中0表示黑色， 1表示白色(255)。
- 在数字图像处理中，二值图像占有非常重要的地位，图像的二值化使图像中数据量大为减少，从而能凸显出目标的轮廓。
- 二值图像分析包括轮廓分析、对象测量、轮廓匹配与识别、形态学处理与分割、各种形状检测与拟合、投影与逻辑操作、轮廓特征提取与编码等。

## 全局阈值分割

阈值分割，最直接的方式是设定一个阈值T，用T对图像的像素分类，大于T的像素为255，小于T的像素为0，从而对图像划分出两个区域。

阈值的选取可以分为两种：全局固定阈值分割和全局自适应阈值分割。

### 1、全局固定阈值分割

固定阈值分割，直接指定，往往需要根据个人经验来设置阈值T，常适用于固定不变的场景。

这里，举一个简单的例子，拍摄电影中的绿布，背景是绿布而前景是其他的颜色，通过忽略掉绿色的像素区间，那么就很容易提取到前景信息，故这个阈值可以根据绿色的像素范围来设定。

### 2、全局自适应阈值分割

人为指定存在偏差，可能不是最优阈值，细节上存在差异，那如何寻找全局最佳阈值呢？

因此，Nobuyuki Otsu (大津展之）[^1]1978年提出了Otsu法来选取最佳阈值，主要思想如下：

1. 假设一副图像由前景和背景组成；
2. 遍历不同的阈值，计算不同阈值下对应的前景和背景的类间方差(intra-class variance)。
3. 当类内方差取得极大值时，此时对应的阈值就是OTSU算法所求的阈值。

目标函数是类间方差最大：

$$
\max{ICV}_{T} = PA * (MA-M)^2 + PB * (MB - M )^2
$$

其中，O表示原图像，M是图像所有像素的均值。A和B分别为前景$A = O[O > T]$和背景$B = O[O <= T]$，各自对应的平均值成为 MA 和 MB。A 部分里的像素数占图像的总像素数的比例记作 PA，B部分里的像素数占图像的总像素数的比例记作 PB。T是所求的最佳阈值。

Otsu方法的缺陷：当图像背景复杂，当前景与背景的大小比例悬殊时，类间方差准则函数可能呈现双峰或多峰，效果会大大降低。

因此，可以引进一个权值来优化前景和背景的比例，即 $\alpha$ 可以取一个 0-1之间的值（需要经验值）。

$$
\max{ICV}_{T} = PA^\alpha * (MA-M)^2 + PB^\alpha * (MB - M )^2
$$

```
1. 统计图像像素值0~255各灰阶的像素个数，即灰度直方图。
2. 计算图像的累积分布函数，计算图像平均像素。
3. 遍历循环0~255灰阶，循环迭代阈值T：
	a. 计算背景B（小于阈值T）所占的平均灰度、背景像素所占比例；（前缀和）
	b. 计算前景A所占的平均灰度、前景像素所占比例；
	c. 计算对应的类间方差，并更新类间方差最大值；
4. 重复步骤4，直到找到最大的类间方差为止，对应的阈值即为最佳阈值。
```

## 局部自适应阈值分割

当环境复杂，光照不均匀的时候，图像的灰度可能分布不均匀，往往单一的阈值不能很好的适应图片。**其根据图像不同区域的亮度分布，计算其局部阈值**，不同区域可以自适应计算不同的阈值，能更好地匹配复杂的环境。

简单的来说，利用区域核大小，对邻域内区域进行加权和，然后减去常量C，作为该区域的阈值，与领域中心像素比较，对像素分类。快速实现可以通过原图像减去滤波图像，再判断每个位置与0的大小，从而分割图像。

特别值得一提的，常见的加权方式：均值加权和高斯加权。如果存在特殊的点状，也可以用中值加权。

## 局部阈值分割 优化

由于光照的影响，图像的灰度可能是不均匀分布的，此时单一阈值的方法分割效果不好。1989年Yanowitz[^2][^3]提出了一种局部阈值分割方法。结合边缘和灰度信息找到阈值表面（treshhold surface），在阈值表面上的就是目标。

## 参考

[^1]: Otsu N. A threshold selection method from gray-level histogram. IEEE Trans,1979;SMC-9;62-66.
[^2]:S. D. Yanowitz and A. M. Bruckstein, "A new method for image segmentation," Comput. Graph. Image Process. 46, 82–95 ,1989.
[^3]: https://github.com/radishgiant/ThresholdAndSegment



# 基于机器学习的的图像分割
