## Numba ##

Numba是一个库，可以在运行时将Python代码编译为本地机器指令，不会大幅度的改变普通的Python代码。

可以通过给函数定义添加一个装饰器实现
```
from numba import jit

from numpy import arange

# jit decorator tells Numba to compile this function
#the argument types will be in ferred by Numba when function is called
@jit

def sum2d(arr):
M,N=arr.shape
result=0.0
for i in range(M):
    for j in range(N):
        result+=arr[i,j]
return result

a= arrange(9).reshape(3,3)

print(sum2d(a))
```