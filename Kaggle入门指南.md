Kaggle入门指南
===============

## 1.pandas

```python
import pandas as pd
```

两个核心：DataFrame 和 Series

```python
pd.DataFrame({'Yes':[50,21], 'No':[131,2]})
```

|  | Yes | No |
| --- | --- | --- |
| 0 | 50 | 131 |
| 1 | 21 | 2 |

```python
pd.DataFrame({'Bob':['I like it','Its awful'],'Sue':['Pretty good','Bland']})
```

|  | Bob | Sue |
| --- | --- | --- |
| 0 | I like it | Pretty good |
| 1 | Its awful | Bland |

用DataFrame会生成列的标签和对应的键值，行标签会自动从0开始增加，有的时候想要自己定义，用index即可

```python
pd.DataFrame({'Bob':[50,21], 'Alice':[131,2]}, index=['2020 Sales', '2021 Sales'])
```

|  | Bob | Alice |
| --- | --- | --- |
| 2020 Sales | 50 | 131 |
| 2021 Sales | 21 | 2 |

series相比就是一sequence数据值，dataframe是一个table。

```python
pd.Series([1,2,3,4,5])
```

```text
0    1
1    2
2    3
3    4
4    5
dtype: int64
```

series就是一列dataframe，但就是没有列名，也可以使用index命名行

```python
pd.Series([30,31,32],index=['2015s','2016s','2017s'])
```

```text
2015s    30
2016s    31
2017s    32
dtype: int64
```

dataframe看做是多个series粘合是有益的

处理已经存在的数据比如csv等等

```python
wine_reviews=pd.read_csv("../data/wine-reviews/winemag-data-130k-v2.csv")
```

```python
wine_reviews.shape
wine_reviews.head() #展示前五行
df = pd.read_csv("../data/wine-reviews/winemag-data-130k-v2.csv",index_col=0) #将第一列作为索引
```
