## Pandas
### 利用Pandas，我們可以分析time series，Pandas有兩種主要的資料結構：
1. Series：一條時間序列
1. DataFrame：很多的時間序列
> 假如您會使用Series，相信離精通DataFrame也不遠了！

> 所以接下來我們以Series開始講起
```python
import pandas as pd

s=pd.Series([1,2,3,4])

date = pd.date_range('20180101', periods=6)
s = pd.Series([1,2,3,4,5,6], index=date)
```
### 查找
```python
s.loc['20180104']
s.loc['20180102':'2018-01-04']

s.iloc[1]
s.iloc[0:2]
```
### 修改
```python
# s.max() 最大值
# s.min() 最小值
# s.mean() 平均值
# s.std() 長度平均值
# s.cumsun() values值累加
# s.cumprod() values值累乘
# s.rolling(2).sum() 移動窗格2 再把所有數字加起來
```
### 繪圖
```python
%matplotlib inline
s.plot()
```
## DataFrame 介紹！
DataFrame 基本上，可以想像是很多的time series，
我們剛剛所學，都是修改其中一條 Series，但我們藉由dataframe，就能以相同的function，直接修改多個Series喔！
```python
s1 = pd.Series([1,2,3,4,5,6], index=date)
s2 = pd.Series([5,6,7,8,9,10], index=date)
s3 = pd.Series([11,12,5,7,8,2], index=date)

dictionary = {
    'c1': s1,
    'c2': s2,
    'c3': s3,
}

df = pd.DataFrame(dictionary)
df

df.loc['2018-01-02']
df.iloc[1]
df.loc['2018-01-02':'2018-01-05', ['c1', 'c2']]
df['c3']
df.cumsum(axis=1) # axis=0 由上往下累加 # axio=1 由左往右累加
```