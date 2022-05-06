## matplotlib
### 同时绘制多条曲线
```
import matplotlib.pyplot as plt
import numpy as np

# 从[-1,1]中等距去50个数作为x的取值
x = np.linspace(-1, 1, 50)
y1 = 2*x + 1
y2 = 2**x + 1
# num表示的是编号，figsize表示的是图表的长宽
plt.figure(num = 2,figsize=(8, 5))  
plt.plot(x, y2)
# 设置线条的样式
plt.plot(x, y1, 
         color='red',  # 线条的颜色
         linewidth=1.0,  # 线条的粗细
         linestyle='--'  # 线条的样式
        )

# 设置取值参数范围
plt.xlim((-1, 2))  # x参数范围
plt.ylim((1, 3))  # y参数范围

# 设置点的位置
new_ticks = np.linspace(-1, 2, 5)
plt.xticks(new_ticks)
# 为点的位置设置对应的文字。
# 第一个参数是点的位置，第二个参数是点的文字提示。‘\ ’表示一个空格
plt.yticks([-2, -1.8, -1, 1.22, 3],
          [r'$really\ bad$', r'$bad$', r'$normal$', r'$good$', r'$readly\ good$'])

# gca = 'get current axis'
ax = plt.gca()
# 将右边和上边的边框（脊）的颜色去掉
ax.spines['right'].set_color('none')
ax.spines['top'].set_color('none')
# 绑定x轴和y轴
ax.xaxis.set_ticks_position('bottom')
ax.yaxis.set_ticks_position('left')
# 定义x轴和y轴的位置
ax.spines['bottom'].set_position(('data',0))
ax.spines['left'].set_position(('data', 0))

plt.show()
```
输出结果为
![结果](/home/gao/Documents/Markdown/pictures/download1.png)
 
### 多条曲线之曲线说明

     import matplotlib.pyplot as plt
    import numpy as np
    
    # 从[-1,1]中等距去50个数作为x的取值
    x = np.linspace(-1, 1, 50)
    y1 = 2*x + 1
    y2 = 2**x + 1
    
    # 第一个参数表示的是编号，第二个表示的是图表的长宽
    plt.figure(num = 3, figsize=(8, 5))  
    plt.plot(x, y2)
    plt.plot(x, y1, color='red', linewidth=1.0, linestyle='--')
    
    # 设置取值参数
    plt.xlim((-1, 2))
    plt.ylim((1, 3))
    
    # 设置lable
    plt.xlabel("I am x")
    plt.ylabel("I am y")
    
    # 设置点的位置
    new_ticks = np.linspace(-1, 2, 5)
    plt.xticks(new_ticks)
    plt.yticks([-2, -1.8, -1, 1.22,3],
              [r'$really\ bad$', r'$bad$', r'$normal$', r'$good$', r'$readly\ good$'])
    
    
    l1, = plt.plot(x, y2, 
                   label='aaa'
                  )
    l2, = plt.plot(x, y1, 
                   color='red',  # 线条颜色
                   linewidth = 1.0,  # 线条宽度
                   linestyle='-.',  # 线条样式
                   label='bbb'  #标签
                  )
    
        # 使用ｌｅｇｅｎｄ绘制多条曲线
    plt.legend(handles=[l1, l2], 
               labels = ['aaa', 'bbb'], 
               loc = 'best'
              )
    
    plt.show()

```
 ![输出结果](/home/gao/Documents/Markdown/pictures/download2.png)
## 多个figure，并加上特殊点注释
```

 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 