# 基于单位扇环灰度的虹膜定位算法
提出了基于单位扇环灰度的虹膜定位算法。这种方法首先利用投影法分割出一个包含瞳孔的矩形区域，根据最大类间方差法确定矩形区域的阈值，分割瞳孔。为了确定某一方向上虹膜外边界点，需要根据瞳孔中心分割出该方向上的一个扇形区域。然后以扇宽为5，计算每个扇形半环的平均灰度值，计算灰度值变化最大的位置的横坐标作为该方向上的半径。再在其他方向上确定边界点，对这些点进行筛选并进行圆的拟合，最终实现虹膜的定位。
主函数main.m,其余的子函数都是在不同方向上对边界的定位。
##实验与结论


###实验 
    为了验证本文所给出的虹膜定位方法的有效性，同时对该方法的其他性能进行测试，主要包括定位的准确性和定位时间两个指标。用于仿真实验的电脑配置Intel(R) Core(TM) i5-4590 CPU @3.30GHz，安装内存4GB，Matlab2016b编程环境的PC机。从中国科学院自动化研究所中的CASIA-v1.0和CASIA-v3.0-Interval两个数据库中随机选出200张图像进行实验。同时与Daugman算法的定位结果进行了相关测量实验和比较。
####算法定位有效性的比较实验
    实验主要比较两种算法能否准确地定位出虹膜的内、外边缘。一个精确的算法要求标记的边界与内外边界吻合，既不存在边界的偏移也不出现过分割欠分割的情况，能准确定位出内外边界轮廓。由于篇幅所限，本文仅采用3组定位后的图像来进行说明。其结果如下图1所示。
图1表示采集的虹膜原始图像经过算法定位之后的几种典型的例图。这些图中是以高亮的方式显示定位的瞳孔和虹膜的边界情况。从中可以看出，原始图像中存在的干扰因素是多样的。图1(a)～图1(c)是利用Daugman算法进行定位的结果，而图1(d)～图1(f)则分别是利用本文算法对以上3幅原始图像进行定位的结果。
![image](https://github.com/1579477793/Iris-location-Algorithm/blob/master/picture/a.bmp)
(a)                 
![image](https://github.com/1579477793/Iris-location-Algorithm/blob/master/picture/b.bmp)
(b)               
![image](https://github.com/1579477793/Iris-location-Algorithm/blob/master/picture/c.bmp)
(c)
![image](https://github.com/1579477793/Iris-location-Algorithm/blob/master/picture/d.bmp)
(d)                
![image](https://github.com/1579477793/Iris-location-Algorithm/blob/master/picture/e.bmp)
(e)               
![image](https://github.com/1579477793/Iris-location-Algorithm/blob/master/picture/f.bmp)
(f)
图1 各种情况下的定位结果

对于大部分图像来说，两种算法在不同情况下均产生了不错的效果。但是需要注意的是，由于瞳孔图像不一定是标准的圆形，所以结果上有略微的差异。具体来说，Daugman算法是计算最大边缘梯度的定位结果，实际上是局部最大值的求解，所以主要看重在各个圆周上大多数边界像素点发挥的作用。而本文的算法在于质心的求解，是根据整个瞳孔区域定位的结果和整个边界上平均半径的计算。对于虹膜外边界的定位，本文根据大部分图像的灰度特征对特定范围的图像进行计算并筛选边缘点，有效的避免了眼睑、睫毛、光斑的干扰。图1(c)是Daugman算法定位的结果，其存在明显的定位偏差，定位失败。主要原因在于Daugman的算法准确性受到初始输入参数的影响，而本文算法可以在无人为设定经验值的情况下获得不错的效果。
准确率测试结果如表1所示，从整体的实验结果可以看出，本文提出的算法相对 Daugman算法分割虹膜区域，其准确性有很大提高。
2算法时间的比较实验
一般认为定位时间是从对原始图像的降噪处理结束到计算出虹膜内外边界的参数值所耗费的时间。计算图像的平均用时，结果如表1所示。
表1 算法准确率和定位时间比较
*算法	    内边界用时/s	整体用时/s	准确率
*Daugman	0.8354	  1.2311	89.5%
*本文算法	0.3926	  0.6261	 93%

本文速度提高主要有以下几个方面的原因:
(1)内边界的定位借助投影法分割和灰度直方图判断，避免了Daugman利用先验条件对圆心和半径进行粗略定位。同时利用一阶中心距确定的圆心和半径既不需要进行曲线拟合也不需要Hough变换。这一内边界定位的过程完全避免了对每个圆的遍历。
(2)在外边界定位时，分割扇形区域，设置合理的步长，与Daugman算法的同心圆周上的微积分计算相比，大大减少了计算量。

####结论
虹膜定位是图像处理研究的热点问题之一，本文提出了一种基于扇环平均灰度方法定位虹膜。文中提出的根据投影法和灰度直方图分割瞳孔的方法能够在不需要人为干预、不需多阈值测试的情况下，自动实现瞳孔的分割，阈值选取效果好。另外该方法不需要去除图像中的光斑，不需要对睫毛进行专门的处理，扇环灰度投影的方法抗干扰能力强。另外同时本文还分析对比了Daugman算法定位的效果，实验证明本文所提出基于扇环平均灰度的定位方法可行且可靠，定位结果精度较高时间短，而且鲁棒性较强，使用起来也更加灵活方便。
