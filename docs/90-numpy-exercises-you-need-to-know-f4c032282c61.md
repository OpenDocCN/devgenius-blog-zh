# 你需要知道的 90 个数字练习

> 原文：<https://blog.devgenius.io/90-numpy-exercises-you-need-to-know-f4c032282c61?source=collection_archive---------2----------------------->

## 你将需要定期进行 Numpy 练习

![](img/ac0ed58e61c331821c5f3defe9cc6234.png)

莎伦·麦卡琴在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

日常使用中需要的有用的 numpy 练习。请看一看。

1.  在名称 np 下导入 numpy 包

```
import numpy as np
```

2.打印 numpy 版本和配置

```
print(np.__version__)
np.show_config()
```

3.创建一个大小为 10 的空向量

```
Z = np.zeros(10)
print(Z)
```

4.如何从命令行获取 numpy add 函数的文档？

```
python -c "import numpy; numpy.info(numpy.add)"
```

5.创建一个大小为 10 的空向量，但第五个值为 1

```
Z = np.zeros(10)
Z[4] = 1
print(Z)
```

6.创建一个值在 10 到 49 之间的向量

```
Z = np.arange(10,50)
print(Z)
```

7.反转向量(第一个元素成为最后一个)

```
Z = np.arange(50)
Z = Z[::-1]
```

8.创建一个 3x3 的矩阵，取值范围从 0 到 8

```
Z = np.arange(9).reshape(3,3)
print(Z)
```

9.从[1，2，0，0，4，0]中寻找非零元素的索引

```
nz = np.nonzero([1,2,0,0,4,0])
print(nz)
```

10.创建一个 3x3 的身份矩阵

```
Z = np.eye(3)
print(Z)
```

11.用随机值创建一个 3x3x3 数组

```
Z = np.random.random((3,3,3))
print(Z)
```

12.用随机值创建一个 10x10 的数组，并找出最小值和最大值

```
Z = np.random.random((10,10))
Zmin, Zmax = Z.min(), Z.max()
print(Zmin, Zmax)
```

13.创建一个大小为 30 的随机向量，并找出平均值

```
Z = np.random.random(30)
m = Z.mean()
print(m)
```

14.创建一个边界为 1，内部为 0 的二维数组

```
Z = np.ones((10,10))
Z[1:-1,1:-1] = 0
```

15.下面这个表达式的结果是什么？

```
0 * np.nan
np.nan == np.nan
np.inf > np.nan
np.nan - np.nan
0.3 == 3 * 0.1
```

16.在对角线正下方创建一个值为 1，2，3，4 的 5x5 矩阵

```
Z = np.diag(1+np.arange(4),k=-1)
print(Z)
```

17.创建一个 8x8 的矩阵，用棋盘图案填充

```
Z = np.zeros((8,8),dtype=int)
Z[1::2,::2] = 1
Z[::2,1::2] = 1
print(Z)
```

18.考虑一个(6，7，8)形状的数组，第 100 个元素的索引(x，y，z)是多少？

```
print(np.unravel_index(100,(6,7,8)))
```

19.使用平铺功能创建一个 8×8 的棋盘矩阵

```
Z = np.tile( np.array([[0,1],[1,0]]), (4,4))
print(Z)
```

20.归一化 5x5 随机矩阵

```
Z = np.random.random((5,5))
Zmax, Zmin = Z.max(), Z.min()
Z = (Z - Zmin)/(Zmax - Zmin)
print(Z)
```

21.创建将颜色描述为四个无符号字节的自定义数据类型(RGBA)

```
color = np.dtype([("r", np.ubyte, 1),
("g", np.ubyte, 1),
("b", np.ubyte, 1),
("a", np.ubyte, 1)])
```

22.将一个 5x3 矩阵乘以一个 3x2 矩阵(实数矩阵乘积)

```
Z = np.dot(np.ones((5,3)), np.ones((3,2)))
print(Z)
```

23.给定一个 1D 数组，就地取 3 到 8 之间的所有元素的反。

```
# Author: Evgeni Burovski
Z = np.arange(11)
Z[(3 < Z) & (Z <= 8)] *= -1
```

24.以下脚本的输出是什么？

```
# Author: Jake VanderPlas
print(sum(range(5),-1))
from numpy import *
print(sum(range(5),-1))
```

25.考虑一个整数向量 Z，这些表达式中哪些是合法的？

```
Z**Z
2 << Z >> 2
Z <- Z
1j*Z
Z/1/1
Z<Z>Z
```

26.下列表达式的结果是什么？

```
np.array(0) // np.array(0)
np.array(0) // np.array(0.)
np.array(0) / np.array(0)
np.array(0) / np.array(0.)
```

27.如何从零开始舍入一个浮点数组？

```
# Author: Charles R Harris
Z = np.random.uniform(-10,+10,10)
print (np.trunc(Z + np.copysign(0.5, Z)))
```

28.使用 5 种不同的方法提取随机数组的整数部分

```
Z = np.random.uniform(0,10,10)
print (Z - Z%1)
print (np.floor(Z))
print (np.ceil(Z)-1)
print (Z.astype(int))
print (np.trunc(Z))
```

29.创建一个行值范围从 0 到 4 的 5x5 矩阵

```
Z = np.zeros((5,5))
Z += np.arange(5)
print(Z)
```

30.考虑一个生成 10 个整数的生成器函数，并用它来构建一个数组

```
def generate():
for x in xrange(10):
yield x
Z = np.fromiter(generate(),dtype=float,count=-1)
print(Z)
```

31.创建一个大小为 10 的向量，值的范围从 0 到 1，两者都不包括

```
Z = np.linspace(0,1,12,endpoint=True)[1:-1]
print(Z)
```

32.创建一个大小为 10 的随机向量并排序

```
Z = np.random.random(10)
Z.sort()
print(Z)
```

33.如何比 np.sum 更快的对小数组求和？

```
# Author: Evgeni Burovski
Z = np.arange(10)
np.add.reduce(Z)
```

34.考虑两个随机数组 A 和 B，检查它们是否相等

```
A = np.random.randint(0,2,5)
B = np.random.randint(0,2,5)
equal = np.allclose(A,B)
print(equal)
```

35.使数组不可变(只读)

```
Z = np.zeros(10)
Z.flags.writeable = False
Z[0] = 1
```

36.考虑表示笛卡尔坐标的随机 10×2 矩阵，将它们转换成极坐标

```
X,Y = Z[:,0], Z[:,1]
R = np.sqrt(X**2+Y**2)
T = np.arctan2(Y,X)
print(R)
print(T)
```

37.创建大小为 10 的随机向量，并将最大值替换为 0

```
Z = np.random.random(10)
Z[Z.argmax()] = 0
print(Z)
```

38.用覆盖[0，1]x[0，1]区域的 x 和 y 坐标创建一个结构化数组

```
Z = np.zeros((10,10), [('x',float),('y',float)])
Z['x'], Z['y'] = np.meshgrid(np.linspace(0,1,10),
np.linspace(0,1,10))print(Z)
```

39.给定两个数组 X 和 Y，构造柯西矩阵 C (Cij = 1/(xi yj))

```
# Author: Evgeni Burovski
X = np.arange(8)
Y = X + 0.5
C = 1.0 / np.subtract.outer(X, Y)
print(np.linalg.det(C))
```

40.打印每个 numpy 标量类型的最小和最大可表示值

```
for dtype in [np.int8, np.int32, np.int64]:
print(np.iinfo(dtype).min)
print(np.iinfo(dtype).max)
for dtype in [np.float32, np.float64]:
print(np.finfo(dtype).min)
print(np.finfo(dtype).max)
print(np.finfo(dtype).eps)
```

41.如何打印一个数组的所有值？

```
np.set_printoptions(threshold=np.nan)
Z = np.zeros((25,25))
print(Z)
```

42.如何在数组中找到最接近的值(给定标量)？

```
Z = np.arange(100)
v = np.random.uniform(0,100)
index = (np.abs(Z-v)).argmin()
print(Z[index])
```

43.创建一个表示位置(x，y)和颜色(r，g，b)的结构化数组

```
Z = np.zeros(10, [ ('position', [ ('x', float, 1),
('y', float, 1)]),
('color', [ ('r', float, 1),
('g', float, 1),
('b', float, 1)])])print(Z)
```

44.假设一个形状为(100，2)的随机向量表示坐标，求逐点距离

```
Z = np.random.random((10,2))
X,Y = np.atleast_2d(Z[:,0]), np.atleast_2d(Z[:,1])
D = np.sqrt( (X-X.T)**2 + (Y-Y.T)**2)
print(D)
# Much faster with scipy
import scipy
# Thanks Gavin Heverly-Coulson (#issue 1)
import scipy.spatial
Z = np.random.random((10,2))
D = scipy.spatial.distance.cdist(Z,Z)
print(D)
```

45.如何将一个浮点数(32 位)数组就地转换成整数(32 位)？

```
Z = np.arange(10, dtype=np.int32)
Z = Z.astype(np.float32, copy=False)
```

46.如何阅读下面的文件？

```
# -------------
1,2,3,4,5
6,,,7,8
,,9,10,11
# -------------
Z = np.genfromtxt("missing.dat", delimiter=",")
```

47.numpy 数组的 enumerate 等价于什么？

```
Z = np.arange(9).reshape(3,3)
for index, value in np.ndenumerate(Z):
print(index, value)
for index in np.ndindex(Z.shape):
```

48.生成类似 2D 高斯的一般数组

```
X, Y = np.meshgrid(np.linspace(-1,1,10), np.linspace(-1,1,10))
D = np.sqrt(X*X+Y*Y)
sigma, mu = 1.0, 0.0
G = np.exp(-( (D-mu)**2 / ( 2.0 * sigma**2 ) ) )
print(G)
```

49.如何在 2D 数组中随机放置 p 个元素？

```
# Author: Divakar
n = 10
p = 3
Z = np.zeros((n,n))
np.put(Z, np.random.choice(range(n*n), p, replace=False),1)
```

50.减去矩阵每行的平均值

```
# Author: Warren Weckesser
X = np.random.rand(5, 10)
# Recent versions of numpy
Y = X - X.mean(axis=1, keepdims=True)
# Older versions of numpy
Y = X - X.mean(axis=1).reshape(-1, 1)
```

51.如何按第 n 列对数组排序？

```
# Author: Steve Tjoa
Z = np.random.randint(0,10,(3,3))
print(Z)
print(Z[Z[:,1].argsort()])
```

52.如何判断给定的 2D 数组是否有空列？

```
# Author: Warren Weckesser
Z = np.random.randint(0,3,(3,10))
print((~Z.any(axis=0)).any())
```

53.从数组中的给定值中找出最近的值

```
Z = np.random.uniform(0,1,10)
z = 0.5
m = Z.flat[np.abs(Z - z).argmin()]
print(m)
```

54.创建一个具有 name 属性的数组类

```
class NamedArray(np.ndarray):
def __new__(cls, array, name="no name"):
obj = np.asarray(array).view(cls)
obj.name = name
return obj
def __array_finalize__(self, obj):
if obj is None: return
self.info = getattr(obj, 'name', "no name")
Z = NamedArray(np.arange(10), "range_10")
print (Z.name)
```

55.考虑给定一个向量，如何给第二个向量索引的每个元素加 1(注意重复索引)？

```
# Author: Brett Olsen
Z = np.ones(10)
I = np.random.randint(0,len(Z),20)
Z += np.bincount(I, minlength=len(Z))
print(Z)
```

56.如何基于索引列表(I)将一个向量(X)的元素累加到一个数组(F)？

```
# Author: Alan G Isaac
X = [1,2,3,4,5,6]
I = [1,3,9,3,4,1]
F = np.bincount(I,X)
print(F)
```

57.考虑(dtype=ubyte)的(w，h，3)图像，计算唯一颜色的数量

```
# Author: Nadav Horesh
w,h = 16,16
I = np.random.randint(0,2,(h,w,3)).astype(np.ubyte)
F = I[...,0]*256*256 + I[...,1]*256 +I[...,2]
n = len(np.unique(F))
print(np.unique(I))
```

58.考虑一个四维数组，如何一次得到最后两个轴的和？

```
A = np.random.randint(0,10,(3,4,3,4))
sum = A.reshape(A.shape[:-2] + (-1,)).sum(axis=-1)
print(sum)
```

59.考虑一维向量 D，如何使用描述子集索引的相同大小的向量 S 来计算 D 的子集的平均值？

```
# Author: Jaime Fernández del Río
D = np.random.uniform(0,1,100)
S = np.random.randint(0,10,100)
D_sums = np.bincount(S, weights=D)
D_counts = np.bincount(S)
D_means = D_sums / D_counts
print(D_means)
```

60.如何求一个点积的对角线？

```
# Author: Mathieu Blondel
# Slow version
np.diag(np.dot(A, B))
# Fast version
np.sum(A * B.T, axis=1)
# Faster version
np.einsum("ij,ji->i", A, B).
```

61.考虑向量[1，2，3，4，5]，如何构建一个新的向量，每个值之间交错 3 个连续的零？

```
# Author: Warren Weckesser
Z = np.array([1,2,3,4,5])
nz = 3
Z0 = np.zeros(len(Z) + (len(Z)-1)*(nz))
Z0[::nz+1] = Z
print(Z0)
```

62.考虑一个维数为(5，5，3)的数组，如何将其乘以维数为(5，5)的数组？

```
A = np.ones((5,5,3))
B = 2*np.ones((5,5))
print(A * B[:,:,None])
```

63.如何交换一个数组的两行？

```
# Author: Eelco Hoogendoorn
A = np.arange(25).reshape(5,5)
A[[0,1]] = A[[1,0]]
print(A)
```

64.考虑一组描述 10 个三角形(具有共享顶点)的 10 个三元组，找出组成所有三角形的唯一线段集

```
# Author: Nicolas P. Rougier
faces = np.random.randint(0,100,(10,3))
F = np.roll(faces.repeat(2,axis=1),-1,axis=1)
F = F.reshape(len(F)*3,2)
F = np.sort(F,axis=1)
G = F.view( dtype=[('p0',F.dtype),('p1',F.dtype)] )
G = np.unique(G)
print(G)
```

65.给定一个数组 C 是一个 bincount，如何产生一个数组 A 使得
np.bincount(A) == C？

```
# Author: Jaime Fernández del Río
C = np.bincount([1,1,2,3,4,4,6])
A = np.repeat(np.arange(len(C)), C)
print(A)
```

66.如何在数组上使用滑动窗口计算平均值？

```
# Author: Jaime Fernández del Río
def moving_average(a, n=3) :
ret = np.cumsum(a, dtype=float)
ret[n:] = ret[n:] - ret[:-n]
return ret[n - 1:] / n
Z = np.arange(20)
print(moving_average(Z, n=3))
```

67.考虑一个一维数组 Z，构建一个二维数组，其第一行是(Z[0]，Z[ 1]，Z[ 2])，随后的每一行移动 1(最后一行应该是(Z[ 3]，Z[2]，Z[1])

```
# Author: Joe Kington / Erik Rigtorp
from numpy.lib import stride_tricks
def rolling(a, window):
shape = (a.size - window + 1, window)
strides = (a.itemsize, a.itemsize)
return stride_tricks.as_strided(a, shape=shape, strides=strides)
Z = rolling(np.arange(10), 3)
print(Z)
```

68.如何否定一个布尔值，或者改变一个浮点数的符号？

```
# Author: Nathaniel J. Smith
Z = np.random.randint(0,2,100)
np.logical_not(arr, out=arr)
Z = np.random.uniform(-1.0,1.0,100)
np.negative(arr, out=arr)
```

69.考虑 2 组点 P0，P1 描述直线(2d)和一个点 p，如何
计算 p 到每条直线 I 的距离(P0[i]，P1[i])？

```
def distance(P0, P1, p):
T = P1 - P0
L = (T**2).sum(axis=1)
U = -((P0[:,0]-p[...,0])*T[:,0] + (P0[:,1]-p[...,1])*T[:,1]) / L
U = U.reshape(len(U),1)
D = P0 + U*T - p
return np.sqrt((D**2).sum(axis=1))
P0 = np.random.uniform(-10,10,(10,2))
P1 = np.random.uniform(-10,10,(10,2))
p = np.random.uniform(-10,10,( 1,2))
print(distance(P0, P1, p))
```

70.考虑 2 组点 P0，P1 描述直线(2d)和一组点 P，如何计算每个点 j (P[j])到每条直线 i (P0[i]，P1[i])的距离？

```
# Author: Italmassov Kuanysh
# based on distance function from previous question
P0 = np.random.uniform(-10, 10, (10,2))
P1 = np.random.uniform(-10,10,(10,2))
p = np.random.uniform(-10, 10, (10,2))
print np.array([distance(P0,P1,p_i) for p_i in p])
```

71.考虑一个任意数组，编写一个函数，提取一个形状固定的子部分，并以给定的元素为中心(必要时用 fillvalue 填充)

```
# Author: Nicolas Rougier
Z = np.random.randint(0,10,(10,10))
shape = (5,5)
fill = 0
position = (1,1)
R = np.ones(shape, dtype=Z.dtype)*fill
P = np.array(list(position)).astype(int)
Rs = np.array(list(R.shape)).astype(int)
Zs = np.array(list(Z.shape)).astype(int)
R_start = np.zeros((len(shape),)).astype(int)
R_stop = np.array(list(shape)).astype(int)
Z_start = (P-Rs//2)
Z_stop = (P+Rs//2)+Rs%2
R_start = (R_start - np.minimum(Z_start,0)).tolist()
Z_start = (np.maximum(Z_start,0)).tolist()
R_stop = np.maximum(R_start, (R_stop - np.maximum(Z_stop-Zs,0))).tolist()
Z_stop = (np.minimum(Z_stop,Zs)).tolist()
r = [slice(start,stop) for start,stop in zip(R_start,R_stop)]
z = [slice(start,stop) for start,stop in zip(Z_start,Z_stop)]
R[r] = Z[z]
print(Z)
print(R)
```

72.考虑一个数组 Z = [1，2，3，4，5，6，7，8，9，10，11，12，13，14]，如何生成数组 R = [[1，2，3，3，4，5]，[3，4，5，6]，…，[11，12，13，14]]？

```
# Author: Stefan van der Walt
Z = np.arange(1,15,dtype=uint32)
R = stride_tricks.as_strided(Z,(11,4),(4,4))
print(R)
```

73.计算矩阵秩

```
# Author: Stefan van der Walt
Z = np.random.uniform(0,1,(10,10))
U, S, V = np.linalg.svd(Z) # Singular Value Decomposition
rank = np.sum(S > 1e-10)
```

74.如何找到一个数组中最频繁出现的值？

```
Z = np.random.randint(0,10,50)
print(np.bincount(Z).argmax())
```

75.从随机的 10x10 矩阵中提取所有连续的 3x3 块

```
# Author: Chris Barker
Z = np.random.randint(0,5,(10,10))
n = 3
i = 1 + (Z.shape[0]-3)
j = 1 + (Z.shape[1]-3)
C = stride_tricks.as_strided(Z, shape=(i, j, n, n), strides=Z.strides + Z.strides)
print(C)
```

76.创建一个 2D 数组子类，使得 Z[i，j] == Z[j，i]

```
# Author: Eric O. Lebigot
# Note: only works for 2d array and value setting using indices
class Symetric(np.ndarray):
def __setitem__(self, (i,j), value):
super(Symetric, self).__setitem__((i,j), value)
super(Symetric, self).__setitem__((j,i), value)
def symetric(Z):
return np.asarray(Z + Z.T - np.diag(Z.diagonal())).view(Symetric)
S = symetric(np.random.randint(0,10,(5,5)))
S[2,3] = 42
print(S)
```

77.考虑一组形状为(n，n)的 p 个矩阵和一组形状为(n，1)的 p 个向量。如何一次性计算 p 个矩阵乘积的和？(结果具有形状(n，1))

```
# Author: Stefan van der Walt
p, n = 10, 20
M = np.ones((p,n,n))
V = np.ones((p,n,1))
S = np.tensordot(M, V, axes=[[0, 2], [0, 1]])
print(S)
# It works, because:
# M is (p,n,n)
# V is (p,n,1)
# Thus, summing over the paired axes 0 and 0 (of M and V independently),
# and 2 and 1, to remain with a (n,1) vector.
```

78.考虑一个 16x16 的数组，如何求块和(块大小为 4x4)？

```
# Author: Robert Kern
Z = np.ones(16,16)
k = 4
S = np.add.reduceat(np.add.reduceat(Z, np.arange(0, Z.shape[0], k), axis=0),
np.arange(0, Z.shape[1], k), axis=1)
```

79.如何用 numpy 数组实现生命的游戏？

```
# Author: Nicolas Rougier
def iterate(Z):
# Count neighbours
N = (Z[0:-2,0:-2] + Z[0:-2,1:-1] + Z[0:-2,2:] +
Z[1:-1,0:-2] + Z[1:-1,2:] +
Z[2: ,0:-2] + Z[2: ,1:-1] + Z[2: ,2:])
# Apply rules
birth = (N==3) & (Z[1:-1,1:-1]==0)
survive = ((N==2) | (N==3)) & (Z[1:-1,1:-1]==1)
Z[...] = 0
Z[1:-1,1:-1][birth | survive] = 1
return Z
Z = np.random.randint(0,2,(50,50))
for i in range(100): Z = iterate(Z)
```

80.如何获得数组的 n 个最大值

```
Z = np.arange(10000)
np.random.shuffle(Z)
n = 5
# Slow
print (Z[np.argsort(Z)[-n:]])
# Fast
print (Z[np.argpartition(-Z,n)[:n]])
```

81.给定任意数量的向量，构建笛卡儿积(每个项目的每个
组合)

```
# Author: Stefan Van der Walt
def cartesian(arrays):
arrays = [np.asarray(a) for a in arrays]
shape = (len(x) for x in arrays)
ix = np.indices(shape, dtype=int)
ix = ix.reshape(len(arrays), -1).T
for n, arr in enumerate(arrays):
ix[:, n] = arrays[n][ix[:, n]]
return ix
print (cartesian(([1, 2, 3], [4, 5], [6, 7])))
```

82.如何从常规数组创建记录数组？

```
Z = np.array([("Hello", 2.5, 3),
("World", 3.6, 2)])
R = np.core.records.fromarrays(Z.T,names='col1, col2, col3',
formats = 'S8, f8, i8')
```

83.考虑一个大向量 Z，用 3 种不同的方法计算 Z 的 3 次方

```
Author: Ryan G.
x = np.random.rand(5e7)
%timeit np.power(x,3)
1 loops, best of 3: 574 ms per loop
%timeit x*x*x
1 loops, best of 3: 429 ms per loop
%timeit np.einsum('i,i,i->i',x,x,x)
1 loops, best of 3: 244 ms per loop
```

84.考虑形状为(8，3)和(2，2)的两个数组 A 和 B。如何找到 A 中包含 B 中每行元素的行，而不考虑 B 中元素的顺序？

```
# Author: Gabe Schwartz
A = np.random.randint(0,5,(8,3))
B = np.random.randint(0,5,(2,2))
C = (A[..., np.newaxis, np.newaxis] == B)
rows = (C.sum(axis=(1,2,3)) >= B.shape[1]).nonzero()[0]
print(rows)
```

85.考虑 10×3 矩阵，提取具有不相等值的行(例如[2，2，3])

```
# Author: Robert Kern
Z = np.random.randint(0,5,(10,3))
E = np.logical_and.reduce(Z[:,1:] == Z[:,:-1], axis=1)
U = Z[~E]
print(Z)
print(U)
```

86.将整数向量转换为矩阵二进制表示形式

```
# Author: Warren Weckesser
I = np.array([0, 1, 2, 3, 15, 16, 32, 64, 128])
B = ((I.reshape(-1,1) & (2**np.arange(8))) != 0).astype(int)
print(B[:,::-1])
# Author: Daniel T. McDonald
I = np.array([0, 1, 2, 3, 15, 16, 32, 64, 128], dtype=np.uint8)
print(np.unpackbits(I[:, np.newaxis], axis=1))
```

87.给定一个二维数组，如何提取唯一的行？

```
# Author: Jaime Fernández del Río
Z = np.random.randint(0,2,(6,3))
T = np.ascontiguousarray(Z).view(np.dtype((np.void, Z.dtype.itemsize * Z.shape[1])))
_, idx = np.unique(T, return_index=True)
uZ = Z[idx]
print(uZ)
```

88.考虑 2 个向量的 A & B，写出内、外、和、乘函数的等和

```
# Author: Alex Riley
# Make sure to read: [http://ajcr.net/Basic-guide-to-einsum/](http://ajcr.net/Basic-guide-to-einsum/)
np.einsum('i->', A) # np.sum(A)
np.einsum('i,i->i', A, B) # A * B
np.einsum('i,i', A, B) # np.inner(A, B)
np.einsum('i,j', A, B) # np.outer(A, B)
```

89.考虑一条由两个向量(X，Y)描述的路径，如何使用
等距样本对其进行采样？

```
# Author: Bas Swinckels
phi = np.arange(0, 10*np.pi, 0.1)
a = 1
x = a*phi*np.cos(phi)
y = a*phi*np.sin(phi)
dr = (np.diff(x)**2 + np.diff(y)**2)**.5 # segment lengths
r = np.zeros_like(x)
r[1:] = np.cumsum(dr) # integrate path
r_int = np.linspace(0, r.max(), 200) # regular spaced path
x_int = np.interp(r_int, r, x) # integrate path
y_int = np.interp(r_int, r, y)
```

90.给定一个整数 n 和一个 2D 数组 X，从 X 中选择可以被
解释为从 n 次多项式分布中抽取的行，即只包含整数且总和为 n 的行

```
# Author: Evgeni Burovski
X = np.asarray([[1.0, 0.0, 3.0, 8.0],
[2.0, 0.0, 1.0, 1.0],
[1.5, 2.5, 1.0, 0.0]])n = 4
M = np.logical_and.reduce(np.mod(X, 1) == 0, axis=-1)
M &= (X.sum(axis=-1) == n)
print(X[M])
```

参考:[https://github.com/rougier/numpy-100](https://github.com/rougier/numpy-100)