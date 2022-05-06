#OpenCV python 
---
**高通滤波器**：根据像素与临近像素的亮度差值来提升该像素的亮度。
**低通滤波器**：在像素与周围像素的亮度的差值小于一个特定值时，平滑该像素的亮度

---
##opencv之GaussianBlur函数
**概述**
`GaussianBlur()`函数用高斯滤波器（GaussianFilter）对图像进行平滑处理。
该函数将云图像与指定的高斯内核进行卷积，同时也支持in-palace滤波

    cv2.GaussianBlur(src, ksize, sigmaX[, dst[, sigmaY[, borderType]]])
关于参数ksize：

- ksize.width和ksize.height可以不同
  - 取值有2种情况：
  - 可以是正的奇数
  - 也可以是0，此时它们的值会自动由sigma进行计算
  
- 关于参数sigmaX和sigmaY：
 - sigmaY=0时，其值自动由sigmaX确定（sigmaY=sigmaX）；
 - sigmaY=sigmaX=0时，它们的值将由ksize.width和ksize.height自动确定；

##opecv提供的边缘检测函数

将非边缘区域转换为黑色，边缘区域转换为白色或其他饱和的颜色
1. `Laplacian()`
（作为边缘检测函数，会产生明显的线条，灰度图像更是如此，在使用`mediumBlur()`函数之后，使用`Laplacian()`之前需要将图像从BGR色彩空间装换为灰度色彩空间,得到`Laplacianan()`数结果后，将其转换为黑色边缘和白色背景的图像，将其归一化，并乘以原图像以便将边缘变黑 ）
2. `Sobel`
3. `Scharr`
上述函数容易将噪声错识别为边缘，解决办法为找到边缘前对图像进行模糊处理
##opencv提供的模糊滤波函数
1. 简单算数平均`blur`
2. `mediaBlur`（数字化视频的噪声，特别是去除彩色图像的噪声）
3. `GuassianBlur`

##canny边缘检测算法

## 支持向量机(SVM)
支持向量机(support vector machines) 是一种二分类模型，其目的是寻找一个超平面来对样本进行分割，分割的时间原则是时间间隔最大化，最终转化为一个凸二次规划问题来求解，由简至繁的模型包括：

 - 当训练样本线性可分时，通过硬间隔最大化，学习一个线性可分支持向量机；
 - 当训练样本近似线性可分时，通过软间隔最大化，学习一个线性支持向量机；
 - 当训练样本线性不可分时，通过核技巧和软件间隔最大化，学习一个非线性支持向量机；

支持向量机SVM模型对数据分布的要求低，如果在事先对数据的分布没有任何的先验信息，即不知道是什么分布，那么SVM无疑是最好的选择。

## K-means聚类
K-means聚类是用于数据分析的向量量化方法。对于给定的数据集，K表示要分割的数据中的簇数。术语“means”指的是数学中的均值，从可视化表示的角度来看，簇的均值其实是这个簇中点的几何中心。

## 背景分割器： KNN、MOG2和GMG
KNN(K-Nearest）
MOG2(Mixture of Gaussians)
GMG(Geometric Multigid)

BackgroundSubtractor：
常用于对不同的帧进行比较，并存贮以前的帧，可按照时间推移的方法来提高运动分析的结果。
另一个基本特征是可以计算阴影，通过检测阴影可排除检测图像的阴影区域（采用阈值的方式），从而能关注实际特征。

## 均值漂移(Meanshift)
均值漂移是一种目标跟踪算法，该算法寻找概率函数离散样本的最大密度（例如感兴趣的图像区域），并重新计算在下一帧中的最大密度，该算法给出了目标的移动方向。重复进行该计算，直到与原始中心匹配，或者在连续迭代计算后中心保持不变。（窗口是固定的）(**对比Camshift**)
![meanshift][1]
meanshift截取的三幅图像
![meanshift][2]
## calcHist函数
计算感兴趣区域的直方图

    calcHist(...)
        calcHist(image,channel,mask,histSize,range[,hist[,accumulate]]) ->hist
![calcHist.png-255.4kB][3]

    roi_hist = cv2.calcHist([hsv_roi],[0],mask,[18],[0,180])
可解释为计算图像数组的彩色直方图，该图像数组只包含有HSV空间中感兴趣的区域，这里只计算掩码值为0所对应的图像值，有18列直方图，每列直方图以0为左边界以180为右边界。

## calcBackProject函数（直方图反向投影））
该函数可以得到直方图并将其投影到一幅图像上，其结果是概率，即每个像素属于起初那副生成直方图的图像的概率。因此，`calcBackproject`给出的是一个概率估计：一副图像等于或类似与模型图像（产生原始直方图的图像）的概率。

**综上，**`calcHist`函数从图像提取彩色直方图，对图像的颜色进行统计并进行展示，`calcBackProject`可用来计算图像的每个像素属于原始图像的概率。

## Camshift
meanshift得到的结果可以看出，其窗口的大小是固定的逐渐变大，固定窗口是不合适的。
Camshift算法首先使用meanshift，meanshift找到（并覆盖）目标之后，再去调整窗口的大小。还会计算目标对象的最佳外接椭圆的角度，并以此调节窗口角度，然后使用更新后的窗口大小和角度来在位置继续进行meanshift。重复这个过程达到需要的精度。
![camshift][4]
![camshift][5]

##kalman滤波

 1. 预测：在这一阶段，kalman滤波器使用由当前点计算的协方差来估计目标的新位置
 2. 更新：在这一阶段，kalmanl滤波器记录目标的位置，并为下一次循环计算修正协方差

## HSV (Hue,Satura)tion,Value):
是根据颜色的直观特性创建的一种颜色空间，模型中颜色的参数分别是：色调(H),饱和度(s),明度(V)
色调H：用角度度量，取值范围为$0^o到360^o$
饱和度S:颜色接近光谱色的程度
明度V:颜色明亮的程度0%为黑100%为白

## 人工神经网络学习算法
![人工神经网络学习算法.png-281.2kB][6]



  [1]: http://opencv-python-tutroals.readthedocs.io/en/latest/_images/meanshift_face.gif
  [2]: http://opencv-python-tutroals.readthedocs.io/en/latest/_images/meanshift_result.jpg
  [3]: http://static.zybuluo.com/GaoSHF/4zkjz4d4ku7a0dc0krrtu3r2/calcHist.png
  [4]: http://opencv-python-tutroals.readthedocs.io/en/latest/_images/camshift_face.gif
  [5]: http://opencv-python-tutroals.readthedocs.io/en/latest/_images/camshift_result.jpg
  [6]: http://static.zybuluo.com/GaoSHF/i2ijumobqrdhec2uxnijkn1z/%E4%BA%BA%E5%B7%A5%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C%E5%AD%A6%E4%B9%A0%E7%AE%97%E6%B3%95.png