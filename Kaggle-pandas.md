# Kaggle 入门指南 · Pandas

## 1. 创建、读取与保存数据

```python
import pandas as pd
```

pandas 的两个核心对象：**DataFrame**（二维表格）和 **Series**（一维数据序列）。

### DataFrame

字典的键作为列名；行索引默认从 `0` 开始。

```python
pd.DataFrame({'Yes': [50, 21], 'No': [131, 2]})
```

|   | Yes | No |
|---|---:|---:|
| 0 | 50 | 131 |
| 1 | 21 | 2 |

```python
pd.DataFrame({'Bob': ['I like it', 'Its awful'], 'Sue': ['Pretty good', 'Bland']})
```

|   | Bob | Sue |
|---|---|---|
| 0 | I like it | Pretty good |
| 1 | Its awful | Bland |

用 `index` 自定义行标签：

```python
pd.DataFrame(
    {'Bob': [50, 21], 'Alice': [131, 2]},
    index=['2020 Sales', '2021 Sales']
)
```

|   | Bob | Alice |
|---|---:|---:|
| 2020 Sales | 50 | 131 |
| 2021 Sales | 21 | 2 |

### Series

Series 可以理解为 DataFrame 中的一列；反过来，DataFrame 可以理解为多个 Series 组合而成。

```python
pd.Series([1, 2, 3, 4, 5])
```

```text
0    1
1    2
2    3
3    4
4    5
dtype: int64
```

Series 同样可以指定 `index`，也可以用 `name` 命名：

```python
pd.Series([30, 31, 32], index=['2015s', '2016s', '2017s'])
```

```text
2015s    30
2016s    31
2017s    32
dtype: int64
```

```python
pd.Series([30, 31, 32], name='Sales')
```

### CSV 读写

```python
wine_reviews = pd.read_csv('../data/wine-reviews/winemag-data-130k-v2.csv')
wine_reviews.shape       # (行数, 列数)
wine_reviews.head()      # 前 5 行

# 将 CSV 的第一列作为行索引

df = pd.read_csv('../data/wine-reviews/winemag-data-130k-v2.csv', index_col=0)
```

`shape` 是属性，不加 `()`；`head(3)` 查看前 3 行。

保存 CSV：

```python
animal.to_csv('地址')
animal.to_csv('animals.csv', index=False)  # 不保存行索引
```

文件路径需要实际存在；`animal` 需要是已经创建的 DataFrame。

---

## 2. 索引、选择与赋值

以下使用 `reviews` 表，若沿用上面的数据，可先执行 `reviews = wine_reviews`。

### 访问列

```python
reviews.country
reviews['country']
reviews['country'][0]
```

前两种写法都返回 `country` 列；`[]` 也适用于含空格的列名，例如 `reviews['country one']`（该列存在时）。`reviews['country'][0]` 中的 `0` 涉及索引标签，不一定是第一行。

### `iloc`：按位置选择

`iloc` 和 `loc` 都是**先行、后列**。只传一个参数时取**行**；单取一整行通常返回 Series。`:` 表示该维度的全部。

```python
reviews.iloc[0]             # 第一行
reviews.iloc[:, 0]          # 第一列（所有行）
reviews.iloc[0, 0]          # 第一行第一列的值
reviews.iloc[:3, 0]         # 第 0、1、2 行，第 0 列
reviews.iloc[1:3, 0]        # 第 1、2 行，第 0 列
reviews.iloc[[0, 1, 2], 0]  # 指定行位置 0、1、2
reviews.iloc[-5:]           # 最后 5 行
```

`iloc` 切片**不包含结束位置**，如 `1:3` 只取位置 `1`、`2`。

### `loc`：按标签选择

`loc` 按**行标签、列标签**选择，而不是按第几行、第几列。

```python
reviews.loc[:, ['taster_name', 'taster_twitter_handle', 'points']]
reviews.loc[0, 'country']      # 行标签 0、列标签 country
reviews.loc[0:3, 'country']    # 标签 0 到 3（包含 3，若标签存在）
```

| 对比 | `iloc` | `loc` |
|---|---|---|
| 依据 | 整数位置 | 行、列标签 |
| 切片结尾 | 不包含 | 通常包含结束标签 |

### `set_index()`：设置行索引

```python
reviews.set_index('title')
reviews_by_title = reviews.set_index('title')
```

默认返回新 DataFrame，不直接修改原表；要保存结果，需赋给变量。

### 条件筛选

```python
reviews.country == 'Italy'  # 返回 True / False
reviews.loc[reviews.country == 'Italy']
reviews.loc[(reviews.country == 'Italy') & (reviews.points >= 90)]
reviews.loc[(reviews.country == 'Italy') | (reviews.points >= 90)]
```

`&` 表示“并且”，`|` 表示“或者”；组合条件时各自加括号。

`isin()`：值在指定列表内。

```python
reviews.country.isin(['Italy', 'France'])
reviews.loc[reviews.country.isin(['Italy', 'France'])]
```

`isnull()` / `notnull()`：判断值是否缺失。

```python
reviews.loc[reviews.price.isnull()]   # price 为空
reviews.loc[reviews.price.notnull()]  # price 不为空
```

### 新增或修改列

```python
reviews['critic'] = 'everyone'  # 整列赋值
reviews['critic']

reviews['index_backwards'] = range(len(reviews), 0, -1)
reviews['index_backwards']     # 行数、行数-1、……、1
```

仅修改符合条件的行：

```python
reviews.loc[reviews.country == 'Italy', 'critic'] = 'Italy reviewer'
```

### 按列表选择多行

```python
rows = list(range(1, 7)) + list(range(8, 11))
reviews.loc[rows]   # 按行标签选：1～6、8～10
reviews.iloc[rows]  # 按行位置选
```

`loc[rows]` 要求这些行标签存在；`iloc[rows]` 按位置选择。
