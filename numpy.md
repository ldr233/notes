# NumPy as np
[TOC]
## 必记：
![image](https://upload-images.jianshu.io/upload_images/2233157-b77105789e36c847.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/652)
## 2018.08.02
### np.repeat
属于ndarray对象的方法，使用它可以通过两个方法
- np.repeat(a,repeats,axis=None)
- object(ndarray).repeat(repeats,axis=None)

其中
1. axis=None时就会flatten当前矩阵，实际上就是变成了一个行向量
2. axis=0,沿着y轴复制，实际上增加了行数
3. axis=1,沿着x轴复制，实际上增加列数              
4. repeats可以为一个数，也可以为一个矩阵

实例：
```
a = np.array(([1, 2], [3, 4]))
b = np.repeat(a, 3)
axis0 = np.repeat(a, 3, axis=0)
axis1 = np.repeat(a, 2, axis=1)
mat_axis0 = np.repeat(a, [2, 3], axis=0)
mat_axis1 = np.repeat(a, [2, 3], axis=1)
```
结果如下：
```
a: 
[[1 2]
 [3 4]]
b: 
[1 1 1 2 2 2 3 3 3 4 4 4]
axis0: 
[[1 2]
 [1 2]
 [1 2]
 [3 4]
 [3 4]
 [3 4]]
axis1: 
[[1 1 2 2]
 [3 3 4 4]]
mat_axis0: 
[[1 2]
 [1 2]
 [3 4]
 [3 4]
 [3 4]]
mat_axis1: 
[[1 1 2 2 2]
 [3 3 4 4 4]]
```
### np.tile
也是复制的功能，不需要 axis 关键字参数，仅通过第二个参数便可指定在各个轴上的复制倍数。
```
a = np.array(([1, 2], [3, 4]))
b = np.tile(a, 3)
c = np.tile(a, [2, 4])
```
结果如下：
```
[[1 2 1 2 1 2]
 [3 4 3 4 3 4]]
____________________________
[[1 2 1 2 1 2 1 2]
 [3 4 3 4 3 4 3 4]
 [1 2 1 2 1 2 1 2]
 [3 4 3 4 3 4 3 4]]
```
### np.zeros_like
参数是一个mat矩阵，生成一个相同大小的全零矩阵
### np.diag
参数是一个mat矩阵和k，以array的形式返回特定对焦线上的元素
```
a = np.arange(9).reshape((3,3))
print(a)
print(np.diag(a))
print(np.diag(a, k=-1))
print(np.diag(a, k=2))
print(np.diag(np.diag(a)))
print(np.diag(np.diag(a, k=1), k=2))
print(np.diag(np.diag(a, k=-2), k=1))
```
结果如下：
```
[[0 1 2]
 [3 4 5]
 [6 7 8]]
[0 4 8]
[3 7]
[2]
[[0 0 0]
 [0 4 0]
 [0 0 8]]
[[0 0 1 0]
 [0 0 0 5]
 [0 0 0 0]
 [0 0 0 0]]
[[0 6]
 [0 0]]
```
### np.bincount
统计标签数目   
参数：
1. 标签array
2. weights权重
3. minlength最小长度


```
# 我们可以看到x中最大的数为7，因此bin的数量为8，那么它的索引值为0->7
x = np.array([0, 1, 1, 3, 2, 1, 7])
# 索引0出现了1次，索引1出现了3次......索引5出现了0次......
np.bincount(x)
#因此，输出结果为：array([1, 3, 1, 1, 0, 0, 0, 1])
```
权重解释
```
w = np.array([0.3, 0.5, 0.2, 0.7, 1., -0.6])
# 我们可以看到x中最大的数为4，因此bin的数量为5，那么它的索引值为0->4
x = np.array([2, 1, 3, 4, 4, 3])
# 索引0 -> 0
# 索引1 -> w[1] = 0.5
# 索引2 -> w[0] = 0.3
# 索引3 -> w[2] + w[5] = 0.2 - 0.6 = -0.4
# 索引4 -> w[3] + w[4] = 0.7 + 1 = 1.7
np.bincount(x,  weights=w)
# 因此，输出结果为：array([ 0. ,  0.5,  0.3, -0.4,  1.7])
```
最小长度解释
```
# 我们可以看到x中最大的数为3，因此bin的数量为4，那么它的索引值为0->3
x = np.array([3, 2, 1, 3, 1])
# 本来bin的数量为4，现在我们指定了参数为7，因此现在bin的数量为7，所以现在它的索引值为0->6
np.bincount(x, minlength=7)
# 因此，输出结果为：array([0, 2, 1, 2, 0, 0, 0])

# 我们可以看到x中最大的数为3，因此bin的数量为4，那么它的索引值为0->3
x = np.array([3, 2, 1, 3, 1])
# 本来bin的数量为4，现在我们指定了参数为1，那么它指定的数量小于原本的数量，因此这个参数失去了作用，索引值还是0->3
np.bincount(x, minlength=1)
# 因此，输出结果为：array([0, 2, 1, 2])
```
### ndim方法
对象可为数组或矩阵，返回维数
### np.nonzero
返回数组a中值不为零的元素的下标，它的返回值是一个长度为a.ndim(数组a的轴数)的元组，元组的每个元素都是一个整数数组，其值为非零元素的下标在对应轴上的值。
                     
```
>>> b2 = np.array([[True, False, True], [True, False, False]])
>>> np.nonzero(b2)
    (array([0, 0, 1]), array([0, 2, 0]))
>>> a = np.arange(3*4*5).reshape(3,4,5)
>>> a[b2]
array([[ 0,  1,  2,  3,  4],
       [10, 11, 12, 13, 14],
       [20, 21, 22, 23, 24]])
>>> a[np.nonzero(b2)]
array([[ 0,  1,  2,  3,  4],
       [10, 11, 12, 13, 14],
       [20, 21, 22, 23, 24]])
>>> a[1:3, b2]
array([[20, 22, 25],
       [40, 42, 45]])
>>> a[1:3, np.nonzero(b2)[0], np.nonzero(b2)[1]]
array([[20, 22, 25],
       [40, 42, 45]])
```
对于二维数组b2，nonzero(b2)所得到的是一个长度为2的元组。它的第0个元素是数组a中值不为0的元素的第0轴的下标，第1个元素则是第1轴的下标，因此从下面的结果可知b2[0,0]、b[0,2]和b2[1,0]的值不为0
### np.eye
通常用来生成单位矩阵
参数：
1. N, M矩阵大小
2. k: 全一对角线位置
3. 数据类型
```
a = np.eye(2)
print(a)
b = np.eye(3, k=1)
print(b)
c = np.eye(2, 3)
print(c)
d = np.eye(2, 3, k=1)
print(d)
```
结果如下
```
[[1. 0.]
 [0. 1.]]
[[0. 1. 0.]
 [0. 0. 1.]
 [0. 0. 0.]]
[[1. 0. 0.]
 [0. 1. 0.]]
[[0. 1. 0.]
 [0. 0. 1.]]
```
### np.linspace
numpy.linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None)
在指定的间隔内返回均匀间隔的数字。   
参数：
1. start：队列的开始值
2. stop：队列的结束值。当'endpoint = False'时，不包含该点。
3. num：要生成的样本数。默认是50。
4. endpoint(bool型)：如果是True，'stop'是最后样本。否则不包含'stop'。
5. retstep(bool型)：如果是True，返回('samples', 'step')
```
>>> print(np.linspace(1, 10, 5))
[ 1.    3.25  5.5   7.75 10.  ]
>>> print(np.linspace(1, stop=10, retstep=True))
(array([ 1.        ,  1.18367347,  1.36734694,  1.55102041,  1.73469388,
        1.91836735,  2.10204082,  2.28571429,  2.46938776,  2.65306122,
        2.83673469,  3.02040816,  3.20408163,  3.3877551 ,  3.57142857,
        3.75510204,  3.93877551,  4.12244898,  4.30612245,  4.48979592,
        4.67346939,  4.85714286,  5.04081633,  5.2244898 ,  5.40816327,
        5.59183673,  5.7755102 ,  5.95918367,  6.14285714,  6.32653061,
        6.51020408,  6.69387755,  6.87755102,  7.06122449,  7.24489796,
        7.42857143,  7.6122449 ,  7.79591837,  7.97959184,  8.16326531,
        8.34693878,  8.53061224,  8.71428571,  8.89795918,  9.08163265,
        9.26530612,  9.44897959,  9.63265306,  9.81632653, 10.        ]), 0.1836734693877551)
```
### np.allclose
判断两个array是否在误差范围内相等   
参数：
1. arr1, arr2
2. atol
3. rtol

若满足absolute(a - b) <= (atol + rtol * absolute(b))断定相等
```
>>> np.allclose([1e10,1e-7], [1.00001e10,1e-8])
False
>>> np.allclose([1e10,1e-8], [1.00001e10,1e-9])
True
>>> np.allclose([1e10,1e-8], [1.0001e10,1e-9])
False
>>> np.allclose([1.0, np.nan], [1.0, np.nan])
False
>>> np.allclose([1.0, np.nan], [1.0, np.nan], equal_nan=True)
True
```
### 各种sort
#### array.sort()
参数：
1. a：所需排序的数组，若想降序，则输入-a(元素为数) 
2. axis：数组排序时的基准，axis=0，按行排列；axis=1，按列排列 
3. kind：数组排序时使用的方法，其中： 
       kind=′quicksort′为快排；kind=′mergesort′为归并；kind=′heapsort′为堆排； 
4. order：一个字符串或列表，可以设置按照某个属性进行排序
```
array([('Duan', 1.7, 38), ('Li', 1.8, 41),('Wang', 1.9, 38)],
      dtype=[('Name', '|S10'), ('Height', '<f8'), ('Age', '<i4')])
>>> np.sort(a, order=['Age', 'Height']) 
# 先按照属性Age排序,如果Age相等，再按照Height排序，此时参数为列表        
array([('Duan', 1.7, 38), ('Wang', 1.9, 38),('Li', 1.8, 41)],
      dtype=[('Name', '|S10'), ('Height', '<f8'), ('Age', '<i4')])
```
#### np.argsort()
返回排序后的元素值的索引
参数：
1. a：所需排序的数组，若想降序，则输入-a(元素为数) 
2. axis：数组排序时的基准，axis=0，按行排列；axis=1，按列排列 
3. kind：数组排序时使用的方法，其中： 
       kind=′quicksort′为快排；kind=′mergesort′为归并；kind=′heapsort′为堆排； 
4. order：一个字符串或列表，可以设置按照某个属性进行排序
#### np.lexsort()
使用一系列键执行间接稳定排序
参数：
1. a：所需排序的数组，若想降序，则输入-a(元素为数) 
2. axis：数组排序时的基准，axis=0，按行排列；axis=1，按列排列 
3. kind：数组排序时使用的方法，其中： 
       kind=′quicksort′为快排；kind=′mergesort′为归并；kind=′heapsort′为堆排； 
4. order：一个字符串或列表，可以设置按照某个属性进行排序
```
>>> a=[1,5,1,4,3,4,4]
>>> b=[9,4,0,4,0,2,1]
>>> np.lexsort((b,a))
# b在前，a在后，即是先按照a的元素进行比较
# 如a中的最小值为两个1，其索引分别为0,2，再计较b中相应索引上的值，即9,0
# 对应的最小应是：1,0，而其对应的索引为2，所以排序后返回的结果第一个值为索引2
# 下一个最小应是：1,9，而其对应的索引为0，所以排序后返回的结果第一个值为索引0
# 以此类推...
array([2, 0, 4, 6, 5, 3, 1], dtype=int64)
>>> np.lexsort((a,b))
# a在前，b在后，即是先按照b的元素进行比较
# 如b中的最小值为两个0，其索引分别为0,4，再计较a中相应索引上的值，即1,3
# 对应的最小应是：0,1，而其对应的索引为2，所以排序后返回的结果第一个值为索引2
# 下一个最小应是：0,3，而其对应的索引为4，所以排序后返回的结果第一个值为索引4
# 以此类推...
array([2, 4, 6, 5, 3, 1, 0], dtype=int64)
>>> c=[[1,5,1,4,3,4,4],[9,4,0,4,0,2,1]]
>>> c
[[1, 5, 1, 4, 3, 4, 4], [9, 4, 0, 4, 0, 2, 1]]
>>> np.lexsort(c)
# 此种情况与先b后a的情况一致
array([2, 4, 6, 5, 3, 1, 0], dtype=int64)
```
#### np.searchsorted()
参数：
1. a：所需排序的数组 
2. v：待查询索引的元素值 
3. side：查询索引时的方向，其中： 
       kind=′left′为从左至右；kind=′right′为从右至左 
4. sorder：一个字符串或列表，可以设置按照某个属性进行排序
```
>>> list3=[1,2,3,4,5]
>>> np.searchsorted(list3,2)
1
# 如若要在list3中插入元素2，则应当将其插在原列表索引为1的地方，即是插在元素1的后面
>>> np.searchsorted(list3,[-5,7,4,9])
array([0, 5, 3, 5], dtype=int64)
# 如若要在list3中插入元素-5，则应当将其插在原列表索引为0的地方，即是插在元素1的前面
# 其他以此类推...
>>> np.searchsorted([1,2,3,4,5], 3, side='right')
3
```
#### np.partition(与此对应的还有argpartition)
返回数组的分区副本。

创建数组的副本，其元素重新排列，使得第k个位置的元素值位于排序数组中的位置。小于第k个元素的所有元素都在此元素之前移动，并且所有元素都等于或大于它。两个分区中元素的排序是未定义的。   
参数：
1. a：所需排序的数组 
2. {'introselect'}，可选
选择算法。默认为'introselect'。
3. kth ： int或int的序列
要分区的元素索引。元素的第k个值将处于其最终排序位置，所有较小元素将在它之前移动，并且所有较小元素将在其后面移动。分区中所有元素的顺序未定义。如果提供了第k个序列，它将立即将由第k个索引的所有元素分成它们的排序位置。
4. order：一个字符串或列表，可以设置按照某个属性进行排序
```
>>>list=[3,4,5,2,1]
>>>np.partition(list,3)
array([2, 1, 3, 4, 5])
# 以排序后的第3个数，即3进行分区，分区后的结果即是：
# 小于3的元素2,1位于3的前面，大于等于3的元素4,5位于3的后面
```

