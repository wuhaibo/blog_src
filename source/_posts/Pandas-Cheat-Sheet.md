---
title: Pandas Cheat Sheet
description: Pandas Cheat Sheet
date: 2021-05-27 11:41:00
updated: 2021-05-27 11:41:00
tags:
  - Pandas
categories:
  - 技術
---
> Pandas Cheat Sheet

## 创建dataframe

```python
# 按列创建
df = pd.DataFrame(
    {"a" : [4 ,5, 6],
    "b" : [7, 8, 9],
    "c" : [10, 11, 12]},
    index = [1, 2, 3])

# 按行创建
df = pd.DataFrame(
  [[4, 7, 10],
  [5, 8, 11],
  [6, 9, 12]],
  index=[1, 2, 3],
  columns=['a', 'b', 'c'])
```

## 改变dataframe

```python
# 按照某列的值进行行排序
df.sort_values('mpg')
df.sort_values('mpg',ascending=False) 

# 列名重命名
# y -> year
df.rename(columns = {'y':'year'})

# 按索引排序
df.sort_index()

# 重置DataFrame的索引为行号，原索引变为普通列
df.reset_index()

# 移除特定列
df.drop(columns=['Length','Height'])
```

## 添加新列或者新行

```python
in [1]: import pandas as pd
	import numpy as np
in [2]: data = pd.DataFrame(np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]]), columns=['a', 'b', 'c'])
in [3]: data
out[3]:
	 	a 	b 	c
	0 	1 	2 	3
	1 	4 	5 	6
	2 	7 	8 	9

# 添加新列
# simple
data['d'] = 0 # or give a new serie like [3,2,1]

# use loc 
data.loc[:, 'd'] = 0 

# use concat 添加多个列
newColDf = pd.DataFrame({"d":[1,1,1],"e":[2,2,2]})
data = pd.concat([data, newColDf], axis=1,ignore_index=True)




# 添加新行
# use loc 
data.loc[4] = 0 # or give a new serie like [3,2,1]

# use append 一次多行
data.append(pd.DataFrame([[2,3,4]],columns=['a','b','c']), ignore_index = True)
```

## 按行或者列取得dataframe子集

```python
# 筛选满足逻辑表达式的数据
df[df.Length > 7]

# 移除重复行 (仅考虑列)
df.drop_duplicates()

# 选择前n行 或 后n行
df.head(n)
df.tail(n)

# 按行选取行
df.iloc[10:20]

# 选取指定的多列
df[['width','length','species']]

# 选取指定的单列
df['width'] or df.width
```

## dataframe 拼接

```python
# 行拼接
pd.concat([df1,df2])

# 列拼接
# DataFrames的列拼接
pd.concat([df1,df2], axis=1)
```

## 两个dataframe join，类似table join

```python
# -*- coding: utf-8 -*-
"""
Spyder Editor

Dies ist eine temporäre Skriptdatei.
"""
import pandas as pd

left = pd.DataFrame(
        {
            "key1": ["K0", "K0", "K1", "K2"],
            "key2": ["K0", "K1", "K0", "K1"],
            "A": ["A0", "A1", "A2", "A3"],
            "B": ["B0", "B1", "B2", "B3"],
        }
    )
    
right = pd.DataFrame(
        {
            "key1": ["K0", "K1", "K1", "K2"],
            "key2": ["K0", "K0", "K0", "K0"],
            "C": ["C0", "C1", "C2", "C3"],
            "D": ["D0", "D1", "D2", "D3"],
        }
    )

# inner join, default is inner
result = pd.merge(left, right, how="inner", on=["key1", "key2"])

# above is same as 
result = pd.merge(left, right, on=["key1", "key2"])

# left join 
result = pd.merge(left, right, how="left", on=["key1", "key2"])


# right join
result = pd.merge(left, right, how="right", on=["key1", "key2"])


# outer join
result = pd.merge(left, right, how="outer", on=["key1", "key2"])
```

## 汇总函数

```python
sum()
count() # 统计non-na的个数
median() 
quantile() # 计算p分位数
apply(function) # 将传入函数作用于每个对象
min()
max()
mean()
var() # 方差
std() # 标准差
```

## 处理缺失

```python
# 去掉含有NA值的行
df.dropna()

# 将所有的NA/null值替换为value
df.fillna(value)
```