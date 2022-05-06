# pandas 学习笔记


**pandas (Python Data Analysis Library) 是Numpy的一种工具，主要任务为解决数据分析任务而创建的**
**Nnmpy 是Python中的一种开源的数值计算扩展，可以用来存储和处理大型矩阵**

---

*Pands* 处理以下三个数据结构

*  系列(Series)
*  数据帧（DataFrame）
*  面板（Panel)

##  维数和描述
考虑数据结构最好的方法是，较高维数数据结构是其较低维数据结构的容器。例如，*DataFrame*是*Series*的容器，*Panel*是*DataFrame*的容器

|数据结构|	维数|	描述|
|----|----|  ----  |
|系列|	1	|1D标记均匀数组，大小不变。|
|数据帧|2	|一般2D标记，大小可变的表结构与潜在的异质类型的列。|
|面板|	3|	一般3D标记，大小可变数组。|

### 创建对象

    import pandas as pd 
    import numpy as np
    
    s=pd.Series([1,3,5,np.nan,6,8])
    print(s)




