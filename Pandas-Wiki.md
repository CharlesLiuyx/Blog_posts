---
title: Pandas-Wiki
date: 2017-09-30 10:44:25
categories:
- Tools
tags:
- Library
- Wiki
- Machine Learning
---

【阅读时间】百科类
【内容简介】pandas函数库相关操作手册，供查阅使用。笔记对象来自[集智](https://jizhi.im/blog/post/10min2pandas01)
<!-- more -->

必备库的导入

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

# 创建对象

创建一个`Series`对象：传递一个**数值列表**作为参数，令Pandas使用默认索引。

```python
s = pd.Series([1,3,5,np.nan,6,8])
print(s)
>
0    1.0
1    3.0
2    5.0
3    NaN
4    6.0
5    8.0
dtype: float64
```

创建一个`DataFrame`对象：传递待datetime索引和列标签的Numpy数组作为参数。

```python
# 首先创建一个时间序列
dates = pd.date_range('20130101', periods=6)
print(dates)

# 创建DataFrame对象，指定index和columns标签
df = pd.DataFrame(np.random.randn(6,4), index=dates, columns=list('ABCD'))
print(df)
> 
DatetimeIndex(['2013-01-01', '2013-01-02', '2013-01-03', '2013-01-04',
               '2013-01-05', '2013-01-06'],
              dtype='datetime64[ns]', freq='D')
                   A         B         C         D
2013-01-01  0.194508 -0.897507  0.224745  0.090260
2013-01-02  2.412146 -1.191852 -1.644737  0.190971
2013-01-03 -0.674645  0.395960  1.425822 -0.718231
2013-01-04  0.349046  0.315026  2.058357 -0.882345
2013-01-05  1.467093  0.146932 -0.680309 -0.519155
2013-01-06  2.223284 -0.305247 -0.559043  1.017710
```

也可以传递词典来构建`DataFrame`，该词典的元素是形如`Series`的对象。

```python
df2 = pd.DataFrame({ 'A' : 1., 'B' : pd.Timestamp('20130102'), 'C' : pd.Series(1,index=list(range(4)),dtype='float32'), 'D' : np.array([3] * 4,dtype='int32'), 'E' : pd.Categorical(["test","train","test","train"]), 'F' : 'foo' })

print(df2)

# 查看df2对象各列的数据类型
print(df2.dtypes)
>
     A          B    C  D      E    F
0  1.0 2013-01-02  1.0  3   test  foo
1  1.0 2013-01-02  1.0  3  train  foo
2  1.0 2013-01-02  1.0  3   test  foo
3  1.0 2013-01-02  1.0  3  train  foo
A           float64
B    datetime64[ns]
C           float32
D             int32
E          category
F            object
dtype: object
```

# 观察数据

查看一个`DataFrame`对象的**前几行和最后几行**。

```python
print(df.head())
print(df.tail(3))
# 默认情况下，.head()和.tail()输出首尾的前5行，也可以手动指定输出行数。
>
                   A         B         C         D
2013-01-01  0.194508 -0.897507  0.224745  0.090260
2013-01-02  2.412146 -1.191852 -1.644737  0.190971
2013-01-03 -0.674645  0.395960  1.425822 -0.718231
2013-01-04  0.349046  0.315026  2.058357 -0.882345
2013-01-05  1.467093  0.146932 -0.680309 -0.519155
                   A         B         C         D
2013-01-04  0.349046  0.315026  2.058357 -0.882345
2013-01-05  1.467093  0.146932 -0.680309 -0.519155
2013-01-06  2.223284 -0.305247 -0.559043  1.017710
```

查看索引(index)，列标签(columns)和Numpy数组形式的内容。 `.describe()`返回**简约的统计信息，**在工程中非常实用。

```python
# 索引
print(df.index)
>
DatetimeIndex(['2013-01-01', '2013-01-02', '2013-01-03', '2013-01-04','2013-01-05', '2013-01-06'],dtype='datetime64[ns]', freq='D')

# 列标签
print(df.columns)
>
Index(['A', 'B', 'C', 'D'], dtype='object')

# 数值
print(df.values)
>
[[ 0.19450849 -0.89750748  0.22474509  0.09025968]
 [ 2.41214612 -1.19185153 -1.64473716  0.19097097]
 [-0.67464536  0.39595956  1.42582221 -0.71823105]
 [ 0.3490457   0.31502599  2.05835699 -0.88234524]
 [ 1.46709286  0.14693219 -0.68030862 -0.51915519]
 [ 2.2232837  -0.30524727 -0.55904285  1.01771006]]

# 统计
print(df.describe())
>
              A         B         C         D
count  6.000000  6.000000  6.000000  6.000000
mean   0.995239 -0.256115  0.137473 -0.136798
std    1.231715  0.663815  1.391936  0.711615
min   -0.674645 -1.191852 -1.644737 -0.882345
25%    0.233143 -0.749442 -0.649992 -0.668462
50%    0.908069 -0.079158 -0.167149 -0.214448
75%    2.034236  0.273003  1.125553  0.165793
max    2.412146  0.395960  2.058357  1.017710
```

灵活调教你的DataFrame：转置与排序

```python
# 转置
print(df.T)

# 按轴排序，逐列递减
print(df.sort_index(axis=1, ascending=False))

# 按值排序，'B'列逐行递增
print(df.sort_values(by='B'))
```

# 选中

尽管标准Python/Numpy的选中表达式很直观也很适合互动，但在生产代码中还是推荐使用Pandas里的方法如`.at`,`.iat`,`.loc`,`.iloc`,`.ix`等。

## 获取

`DataFrame`里选中的一列与`Series`等效。

```python
print(df["A"]) # 与df.A相同
>
2013-01-01    0.194508
2013-01-02    2.412146
2013-01-03   -0.674645
2013-01-04    0.349046
2013-01-05    1.467093
2013-01-06    2.223284
Freq: D, Name: A, dtype: float64

# 用[]分割DataFrame
print(df[0:3])
print(df['20130102':'20130104'])
>
                   A         B         C         D
2013-01-01  0.194508 -0.897507  0.224745  0.090260
2013-01-02  2.412146 -1.191852 -1.644737  0.190971
2013-01-03 -0.674645  0.395960  1.425822 -0.718231
                   A         B         C         D
2013-01-02  2.412146 -1.191852 -1.644737  0.190971
2013-01-03 -0.674645  0.395960  1.425822 -0.718231
2013-01-04  0.349046  0.315026  2.058357 -0.882345
```

## 按标签选择

```python
# 选中一整行
print(df.loc[dates[0]])
>
A    0.194508
B   -0.897507
C    0.224745
D    0.090260
Name: 2013-01-01 00:00:00, dtype: float64
        
# 按标签选中复数列（所有行，输出只显示前5行）
print(df.loc[:,['A','B']])
>
                  A         B
2013-01-01  0.194508 -0.897507
2013-01-02  2.412146 -1.191852
2013-01-03 -0.674645  0.395960
2013-01-04  0.349046  0.315026
2013-01-05  1.467093  0.146932
2013-01-06  2.223284 -0.305247

# 行/列同时划分（包括起止点）
print(df.loc['20130102':'20130104',['A','B']])
>
                   A         B
2013-01-02  2.412146 -1.191852
2013-01-03 -0.674645  0.395960
2013-01-04  0.349046  0.315026

# 返回一个元素（两个方法等效）
print(df.loc[dates[0],'A'])
print(df.at[dates[0],'A'])
>
0.194508494402
0.194508494402
```

## 按位置选择

```python
# 位置索引为3的行（从0开始，所以其实是第4行）
print(df.iloc[3])
>
A    0.349046
B    0.315026
C    2.058357
D   -0.882345
Name: 2013-01-04 00:00:00, dtype: float64
        
# 按位置索引分割DataFrame
print(df.iloc[3:5,0:2])
print(df.iloc[[1,2,4],[0,2]])
>
                   A         B
2013-01-04  0.349046  0.315026
2013-01-05  1.467093  0.146932

# 直接定位一个特定元素
df.iloc[1,1]
df.iat[1,1]
>
                   A         C
2013-01-02  2.412146 -1.644737
2013-01-03 -0.674645  1.425822
2013-01-05  1.467093 -0.680309
```

## 布尔值索引

```python
# 用一列的值来选择数据
print(df.A > 0)
>
2013-01-01     True
2013-01-02     True
2013-01-03    False
2013-01-04     True
2013-01-05     True
2013-01-06     True
Freq: D, Name: A, dtype: bool
      
# 使用.isin()函数过滤数据
df2 = df.copy()
df2['E'] = ['one', 'one','two','three','four','three']

# 提取df2中'E'值属于['two', 'four']的行
print(df2[df2['E'].isin(['two','four'])])
>
                   A         B         C         D     E
2013-01-03 -0.674645  0.395960  1.425822 -0.718231   two
2013-01-05  1.467093  0.146932 -0.680309 -0.519155  four
```

## 设置

赋值

```python
# 为DataFrame创建一个新的列，其值为时间顺序（与df相同）的索引值
s1 = pd.Series([1,2,3,4,5,6], index=pd.date_range('20130102', periods=6))
print(s1)

df['F'] = s1

# 按标签赋值
df.at[dates[0],'A'] = 0

# 按索引赋值
df.iat[0,1] = 0

# 用Numpy数组赋值
df.loc[:,'D'] = np.array([5] * len(df))

# 最终结果
print(df)
```

# 缺失数据

Pandas默认使用`np.nan`来代表缺失数据。`Reindexing`允许用户对某一轴上的索引改/增/删，并返回数据的副本。

```python
# 创建DataFrame对象df1，以dates[0:4]为索引，在df的基础上再加一个新的列'E'（初始均为NaN）
df1 = df.reindex(index=dates[0:4], columns=list(df.columns) + ['E'])

# 将'E'列的前两个行设为1
df1.loc[dates[0]:dates[1],'E'] = 1

print(df1)
>
                   A         B         C         D    E
2013-01-01  0.194508 -0.897507  0.224745  0.090260  1.0
2013-01-02  2.412146 -1.191852 -1.644737  0.190971  1.0
2013-01-03 -0.674645  0.395960  1.425822 -0.718231  NaN
2013-01-04  0.349046  0.315026  2.058357 -0.882345  NaN
```

## 处理缺失数据

```python
# 剔除df1中含NaN的行（只要任一一列为NaN就算）
df1.dropna(how='any')

# 用5填充df1里的缺失值
df1.fillna(value=5)

# 判断df1中的值是否为缺失数据，返回True/False
pd.isnull(df1)
```

# 操作

## 统计

此类操作**默认排除缺失数据**

```python
# 求平均值
print(df.mean())
>
A   -0.190821
B   -0.050040
C   -0.203207
D    5.000000
F    3.000000
dtype: float64
  
# 指定求平均值的轴
print(df.mean(1))
>
2013-01-01    1.264749
2013-01-02    1.049748
2013-01-03    1.578067
2013-01-04    1.035639
2013-01-05    1.855754
2013-01-06    1.936110
Freq: D, dtype: float64
    
# 创建Series对象s，以dates为索引并平移2个位置
s = pd.Series([1,3,5,np.nan,6,8], index=dates).shift(2)
print(s)
>
2013-01-01    NaN
2013-01-02    NaN
2013-01-03    1.0
2013-01-04    3.0
2013-01-05    5.0
2013-01-06    NaN
Freq: D, dtype: float64

# 从df中逐列减去s（若有NaN则得NaN）
print(df.sub(s, axis='index'))
>
                   A         B         C    D    F
2013-01-01       NaN       NaN       NaN  NaN  NaN
2013-01-02       NaN       NaN       NaN  NaN  NaN
2013-01-03 -1.572312  0.570952 -1.108305  4.0  1.0
2013-01-04 -3.401496 -4.304842 -4.115467  2.0  0.0
2013-01-05 -4.955735 -5.576715 -4.188780  0.0 -1.0
2013-01-06       NaN       NaN       NaN  NaN  NaN
```

## 应用

对`DataFrame`中的数据**应用函数**。

```python
# 逐行累加
print(df.apply(np.cumsum))
>
                   A         B         C   D     F
2013-01-01  0.000000  0.000000  0.058997   5   NaN
2013-01-02  0.277465 -0.161767 -0.807960  10   1.0
2013-01-03 -0.294847  1.409186 -0.916265  15   3.0
2013-01-04 -0.696343  0.104344 -2.031732  20   6.0
2013-01-05 -0.652078 -0.472372 -1.220512  25  10.0
2013-01-06 -1.144929 -0.300242 -1.219241  30  15.0

# 每列的最大值减最小值
print(df.apply(lambda x: x.max() - x.min()))
>
A    0.849776
B    2.875794
C    1.926687
D    0.000000
F    4.000000
dtype: float64
```

## 字符

`Series`对象的`str`属性具有一系列字符处理方法，**可以很轻松地操作数组的每个元素**。

```python
# 字符串变为小写
s = pd.Series(['A', 'B', 'C', 'Aaba', 'Baca', np.nan, 'CABA', 'dog', 'cat'])
print(s.str.lower())
>
0       a
1       b
2       c
3    aaba
4    baca
5     NaN
6    caba
7     dog
8     cat
dtype: object
```

# 合并

## 连接

Pandas在join/merge两中情境下提供了支持多种方式，基于逻辑/集合运算和代数运算来连接Series，DataFrame和Panel对象。

concat()方法连接数组

```python
df = pd.DataFrame(np.random.randn(10, 4))
print(df)
print("------")

# 拆分成块
pieces = [df[:3], df[3:7], df[7:]]

# 重新连接，可得初始数组
print(pd.concat(pieces))
```

## 增补

给DataFrame增补行

```python
df = pd.DataFrame(np.random.randn(8, 4), columns=['A','B','C','D'])
print(df)
print("------")

# 将索引为3的行增补到整个DataFrame最后
s = df.iloc[3]
print(df.append(s, ignore_index=True))
```

# 组合

“组合”包含以下步骤：

- 基于一定标准将数据分组
- 对每个部分分别应用函数
- 把结果汇合到数据结构中

```python
# 新建DataFrame对象df
df = pd.DataFrame({'A' : ['foo', 'bar', 'foo', 'bar', 'foo', 'bar', 'foo', 'foo'], 'B' : ['one', 'one', 'two', 'three', 'two', 'two', 'one', 'three'], 'C' : np.random.randn(8), 'D' : np.random.randn(8)})

print(df)
>
     A      B         C         D
0  foo    one  0.298545 -0.101893
1  bar    one  1.080680 -0.717276
2  foo    two  1.365395  0.939482
3  bar  three  0.783108 -0.575995
4  foo    two -1.089990 -0.033826
5  bar    two  0.442084  1.533146
6  foo    one  0.041715  0.190613
7  foo  three  0.529231  0.380723

# 对'A'列进行合并并应用.sum()函数
print(df.groupby('A').sum())
>
            C         D
A                      
bar  2.305871  0.239875
foo  1.144897  1.375099

# 对'A', 'B'两列分别合并形成层级结构，再应用.sum()函数
print(df.groupby(['A','B']).sum())
>
                  C         D
A   B                        
bar one    1.080680 -0.717276
    three  0.783108 -0.575995
    two    0.442084  1.533146
foo one    0.340260  0.088720
    three  0.529231  0.380723
    two    0.275406  0.905656
```

# 重塑

## 层次化

```python
tuples = list(zip(*[['bar', 'bar', 'baz', 'baz', 'foo', 'foo', 'qux', 'qux'], ['one', 'two', 'one', 'two', 'one', 'two', 'one', 'two']]))

# 多重索引
index = pd.MultiIndex.from_tuples(tuples, names=['first', 'second'])

df = pd.DataFrame(np.random.randn(8, 2), index=index, columns=['A', 'B'])
df2 = df[:4]

print(df2)
>
                     A         B
first second                    
bar   one    -1.144920 -0.823033
      two     0.250615 -1.423107
baz   one     0.291435 -1.580619
      two    -0.574831 -0.742291

# .stack()方法将DataFrame的列“压缩”了一级
stacked = df2.stack()
print(stacked)
>
first  second   
bar    one     A   -1.144920
               B   -0.823033
       two     A    0.250615
               B   -1.423107
baz    one     A    0.291435
               B   -1.580619
       two     A   -0.574831
               B   -0.742291
dtype: float64
```

对于已经层次化的，具有多重索引的DataFrame或Series，`stack()`的逆操作是`unstack()`——默认将最后一级“去层次化”。

```python
print(stacked.unstack())
>
                     A         B
first second                    
bar   one    -1.144920 -0.823033
      two     0.250615 -1.423107
baz   one     0.291435 -1.580619
      two    -0.574831 -0.742291

print(stacked.unstack(1))
>
second        one       two
first                      
bar   A -1.144920  0.250615
      B -0.823033 -1.423107
baz   A  0.291435 -0.574831
      B -1.580619 -0.742291
  
print(stacked.unstack(0))
>
first          bar       baz
second                      
one    A -1.144920  0.291435
       B -0.823033 -1.580619
two    A  0.250615 -0.574831
       B -1.423107 -0.742291
```

## 数据透视表

```python
df = pd.DataFrame({'A' : ['one', 'one', 'two', 'three'] * 3, 'B' : ['A', 'B', 'C'] * 4, 'C' : ['foo', 'foo', 'foo', 'bar', 'bar', 'bar'] * 2, 'D' : np.random.randn(12), 'E' : np.random.randn(12)})

print(df)
>
        A  B    C         D         E
0     one  A  foo -0.411674  0.284523
1     one  B  foo -1.217944  1.519293
2     two  C  foo  0.502824 -0.167898
3   three  A  bar  0.565186  0.226860
4     one  B  bar  0.626023  0.401529
5     one  C  bar -0.437217  0.832881
6     two  A  foo -0.825128  0.346303
7   three  B  foo  0.069236  0.728729
8     one  C  foo  1.647690 -0.531091
9     one  A  bar -0.881553  0.070718
10    two  B  bar  0.203672  1.601761
11  three  C  bar  1.334214 -0.778639

# 生成数据透视表
print(pd.pivot_table(df, values='D', index=['A', 'B'], columns=['C']))
>
C             bar       foo
A     B                    
one   A -0.881553 -0.411674
      B  0.626023 -1.217944
      C -0.437217  1.647690
three A  0.565186       NaN
      B       NaN  0.069236
      C  1.334214       NaN
two   A       NaN -0.825128
      B  0.203672       NaN
      C       NaN  0.502824
```

# 时间序列

Pandas提供了简单、强力且有效的工具，可以在频率转换中进行重采样（比如从秒级数据中提取5分钟一更新的数据）。这在金融工程中应用甚广，当然也不仅限于金融领域。

时区表示

```python
rng = pd.date_range('3/6/2012 00:00', periods=5, freq='D')
ts = pd.Series(np.random.randn(len(rng)), rng)
print(ts)
>
2012-03-06   -0.605179
2012-03-07    0.961252
2012-03-08    1.309406
2012-03-09    1.184313
2012-03-10    0.249745
Freq: D, dtype: float64

# 设定国际时区标准
ts_utc = ts.tz_localize('UTC')
print(ts_utc)
>
2012-03-06 00:00:00+00:00   -0.605179
2012-03-07 00:00:00+00:00    0.961252
2012-03-08 00:00:00+00:00    1.309406
2012-03-09 00:00:00+00:00    1.184313
2012-03-10 00:00:00+00:00    0.249745
Freq: D, dtype: float64

# 切换时区
print(ts_utc.tz_convert('US/Eastern'))
>
2012-03-05 19:00:00-05:00   -0.605179
2012-03-06 19:00:00-05:00    0.961252
2012-03-07 19:00:00-05:00    1.309406
2012-03-08 19:00:00-05:00    1.184313
2012-03-09 19:00:00-05:00    0.249745
Freq: D, dtype: float64
```

周期和时间戳之间的转换让我们可以方便的使用一些算术运算。比如下面的例子，我们把一个**以季度为单位的时间序列转换为按日期表示**。

```python
prng = pd.period_range('1990Q1', '2000Q4', freq='Q-NOV')

ts = pd.Series(np.random.randn(len(prng)), prng)
print(ts.head())

ts.index = (prng.asfreq('M', 'e') + 1).asfreq('H', 's') + 9
print(ts.head())
```

# 分类

```python
df = pd.DataFrame({"id":[1,2,3,4,5,6], "raw_grade":['a', 'b', 'b', 'a', 'a', 'e']})
# 将原始记录转换为分类类型
df["grade"] = df["raw_grade"].astype("category")

print(df["grade"])
```

将类别重命名为更有意义的字样

```python
df["grade"].cat.categories = ["very good", "good", "very bad"]

# 重排类别同时添加上缺失的类别名称
df["grade"] = df["grade"].cat.set_categories(["very bad", "bad", "medium", "good", "very good"])

print(df["grade"])

# 排序在各分类中分别进行
print(df.sort_values(by="grade"))

# 对类别列分组可以显示空类
print(df.groupby("grade").size())
```

# 绘图

随机累加序列

```python
# 生成一串随机序列
ts = pd.Series(np.random.randn(1000), index=pd.date_range('1/1/2000', periods=1000))

# 求累加和
ts = ts.cumsum()

# 输出图象
ts.plot()
```

绘制带标签的列

```python
# 生成一个4列的DataFrame，每列1000行，并逐列累加
df = pd.DataFrame(np.random.randn(1000, 4), index=ts.index, columns=['A', 'B', 'C', 'D']) 
df = df.cumsum()

df.plot(); plt.legend(loc='best')
```

# 获取数据输入/输出

## CSV

df是一个DataFrame

```python
# 输出至.csv文件
df.to_csv('haha.csv')

# 从.csv文件中读取数据
pd.read_csv('haha.csv')
```

## Excel

df是一个DataFrame

```python
# 输出至.xlsx文件
df.to_excel('haha.xlsx', sheet_name='Sheet1')

# 从.xlsx文件中读取数据
pd.read_excel('foo.xlsx', 'Sheet1', index_col=None, na_values=['NA'])
('haha.csv')
```

