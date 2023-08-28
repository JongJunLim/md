---
title: Pandas
layout: default
parent: Python
has_children: true
nav_order: 3
---


<head>
  <style>
    table.dataframe {
      white-space: normal;
      width: 100%;
      height: 240px;
      display: block;
      overflow: auto;
      font-family: Arial, sans-serif;
      font-size: 0.9rem;
      line-height: 20px;
      text-align: center;
      border: 0px !important;
    }

    table.dataframe th {
      text-align: center;
      font-weight: bold;
      padding: 8px;
    }

    table.dataframe td {
      text-align: center;
      padding: 8px;
    }

    table.dataframe tr:hover {
      background: #b8d1f3; 
    }

    .output_prompt {
      overflow: auto;
      font-size: 0.9rem;
      line-height: 1.45;
      border-radius: 0.3rem;
      -webkit-overflow-scrolling: touch;
      padding: 0.8rem;
      margin-top: 0;
      margin-bottom: 15px;
      font: 1rem Consolas, "Liberation Mono", Menlo, Courier, monospace;
      color: $code-text-color;
      border: solid 1px $border-color;
      border-radius: 0.3rem;
      word-break: normal;
      white-space: pre;
    }

  .dataframe tbody tr th:only-of-type {
      vertical-align: middle;
  }

  .dataframe tbody tr th {
      vertical-align: top;
  }

  .dataframe thead th {
      text-align: center !important;
      padding: 8px;
  }

  .page__content p {
      margin: 0 0 0px !important;
  }

  .page__content p > strong {
    font-size: 0.8rem !important;
  }

  </style>
</head>

<div style="text-align: right;">
작성일자 : 2023-08-28<br>
</div>


## 0. Pandas

- Pandas 는 데이터 분석을 위한 자료구조로 데이터 분석 도구를 제공하는 파이썬 라이브러리이며, Pandas 의 특징은 다음과 같음

  - 각각의 행,열에 따라 데이터를 정렬할 수 있는 자료구조

  - 시계열, 비시계열 데이터를 함께 다룰 수 있는 통합 자료구조

  - 데이터의 결측치값을 유연하게 처리할 수 있는 기능

  - 데이터 핸들링 및 특정 행,열의 모든 값을 더하는 등의 데이터 연산 기능

- Pandas 의 자료구조로는 Series와 DataFrame이 있음 

  - Series는 1차원 데이터를 다루는 데 효과적인 자료구조임

  - DataFrame은 행과 열로 구성된 2차원 데이터를 다루는 데 효과적인 자료구조임



```python
import pandas as pd
import numpy as np
```

## 1. Series

- pandas의 series로 데이터를 선언할 때 따로 인덱스를 지정하지 않았다면 기본적으로 0부터 시작하는 정수값으로 인덱싱됨 

- 아래 예제 처럼 index를 사용자가 지정할 수 있음

  - 선언 후 데이터 값을 확인할 때는 values, index를 확인할 때는 index 함수를 이용해서 확인 할 수 있음

  - 기본 함수 사용법 : pd.Series(data, index=index)

  - Series객체 생성 시 인덱스값을 통해 데이터에 접근할 수 있음

  - 파이썬의 리스트와 달리 사용자가 index 값을 지정해 줄 수 있으며, 지정한 index 값으로 데이터에 접근 할 수 있음



```python
s1 = pd.Series([1,2,3])
s1
```

<pre>
0    1
1    2
2    3
dtype: int64
</pre>

```python
s2 = pd.Series(["a","b","c"])
s2
```

<pre>
0    a
1    b
2    c
dtype: object
</pre>

```python
s3 = pd.Series(["a",1,"b",2])
s3
```

<pre>
0    a
1    1
2    b
3    2
dtype: object
</pre>

```python
s4 = pd.Series(["a","b","c"], index=[1,2,3])
s4
```

<pre>
1    a
2    b
3    c
dtype: object
</pre>

```python
s5 = pd.Series([1,2,3],["a","b","c"])
s5
```

<pre>
a    1
b    2
c    3
dtype: int64
</pre>

```python
s5.values
```

<pre>
array([1, 2, 3])
</pre>

```python
s5.index
```

<pre>
Index(['a', 'b', 'c'], dtype='object')
</pre>

```python
s6 = pd.Series([1,2,3,4])
s6
```

<pre>
0    1
1    2
2    3
3    4
dtype: int64
</pre>

```python
s6.index = ("a", "b", "c", "d")
s6
```

<pre>
a    1
b    2
c    3
d    4
dtype: int64
</pre>
### 1.1 Series 기본 함수 



```python
s7 = pd.Series([1,1,1,4,5,6,7,8,9,9,np.NaN])
s7
```

<pre>
0     1.0
1     1.0
2     1.0
3     4.0
4     5.0
5     6.0
6     7.0
7     8.0
8     9.0
9     9.0
10    NaN
dtype: float64
</pre>

```python
s7.size #개수 반환
```

<pre>
11
</pre>

```python
len(s7)
```

<pre>
11
</pre>

```python
s7.count() #NaN을 제외한 개수를 반환
```

<pre>
10
</pre>

```python
s7.shape() #tuple 형태로 shape 반환
```


```python
a1 = np.array(s7)
a1
```

<pre>
array([ 1.,  1.,  1.,  4.,  5.,  6.,  7.,  8.,  9.,  9., nan])
</pre>

```python
print(a1.mean())
print(s7.mean()) #NaN을 제외한 평균 반환
```

<pre>
nan
5.1
</pre>

```python
s7.unique() #유일한 값만 array 형태로 반환
```

<pre>
array([ 1.,  4.,  5.,  6.,  7.,  8.,  9., nan])
</pre>

```python
s7.value_counts() #NaN을 제외하고 각 값들의 빈도를 반환
```

<pre>
1.0    3
9.0    2
4.0    1
5.0    1
6.0    1
7.0    1
8.0    1
dtype: int64
</pre>
### 1.2 Series 연산

 - Series와 스칼라의 연산은 각 원소별로 스칼라와의 연산이 적용

 - Series끼리의 사칙연산도 가능함. 단, index별로 계산이 되는 점을 유의하여야 함




```python
s1 = pd.Series([1,2,3,4,5],["a","b","c","d","e"])
s2 = pd.Series([2,2,2,2,2],["a","b","c","d","e"])

print(s1)
print(s2)
```

<pre>
a    1
b    2
c    3
d    4
e    5
dtype: int64
a    2
b    2
c    2
d    2
e    2
dtype: int64
</pre>

```python
s1*3
```

<pre>
a     3
b     6
c     9
d    12
e    15
dtype: int64
</pre>

```python
s1 + s2
```

<pre>
a    3
b    4
c    5
d    6
e    7
dtype: int64
</pre>

```python
s1["f"] = 100
s2["f"] = 200

print(s1)
print(s2)
```

<pre>
a      1
b      2
c      3
d      4
e      5
f    100
dtype: int64
a      2
b      2
c      2
d      2
e      2
f    200
dtype: int64
</pre>
### 1.3 Series update 



```python
s1 = pd.Series(np.arange(2,13,2),["a","b","c","d","e","f"])
s1
```

<pre>
a     2
b     4
c     6
d     8
e    10
f    12
dtype: int64
</pre>

```python
s1["a"] = 200
s1
```

<pre>
a    200
b      4
c      6
d      8
e     10
f     12
dtype: int64
</pre>

```python
s1.drop("a")
```

<pre>
b     4
c     6
d     8
e    10
f    12
dtype: int64
</pre>

```python
s1
```

<pre>
a    200
b      4
c      6
d      8
e     10
f     12
dtype: int64
</pre>

```python
s1.drop("a", inplace = True) #inplace를 통해 원 데이터 업데이트
s1
```

<pre>
b     4
c     6
d     8
e    10
f    12
dtype: int64
</pre>
### 1.4 Series selection

 - slicing

  - 리스트, array와 동일하게 적용



```python
s1 = pd.Series(np.arange(2,11,2),["a","b","c","d","e"])
s1
```

<pre>
a     2
b     4
c     6
d     8
e    10
dtype: int64
</pre>

```python
s1[1:3]
```

<pre>
b    4
c    6
dtype: int64
</pre>

```python
s1 = pd.Series(np.arange(2,21,2),np.arange(10))
s1
```

<pre>
0     2
1     4
2     6
3     8
4    10
5    12
6    14
7    16
8    18
9    20
dtype: int64
</pre>

```python
s1 > 10
```

<pre>
0    False
1    False
2    False
3    False
4    False
5     True
6     True
7     True
8     True
9     True
dtype: bool
</pre>

```python
s1[s1>10]
```

<pre>
5    12
6    14
7    16
8    18
9    20
dtype: int64
</pre>

```python
(s1 > 10).sum()
```

<pre>
5
</pre>

```python
s1[s1>10].sum()
```

<pre>
80
</pre>

```python
s1 = pd.Series([1,2,3,4,5,np.NaN])
s1
```

<pre>
0    1.0
1    2.0
2    3.0
3    4.0
4    5.0
5    NaN
dtype: float64
</pre>

```python
pd.isnull(s1)
```

<pre>
0    False
1    False
2    False
3    False
4    False
5     True
dtype: bool
</pre>

```python
s1[pd.isnull(s1)]
```

<pre>
5   NaN
dtype: float64
</pre>

```python
pd.notnull(s1)
```

<pre>
0     True
1     True
2     True
3     True
4     True
5    False
dtype: bool
</pre>

```python
s1[pd.notnull(s1)]
```

<pre>
0    1.0
1    2.0
2    3.0
3    4.0
4    5.0
dtype: float64
</pre>
## 2. DataFrame

- Pandas의 Series가 1차원 형태의 자료구조라면 DataFrame은 여러 개의 열로 구성된 2차원 형태의 자료구조임

- numpy array를 받아 만들 수 있으며, Series 처럼 변환 가능한 오브젝트들을 갖고 있는 dict 형태를 인자로 넣어주어 DataFrame을 만들 수 있음




```python
ex = pd.DataFrame({'A': 1.,
                   'B': pd.Timestamp('20130102'),
                   'C': pd.Series(1, index=list(range(5)), dtype='float32'),
                   'D': np.array(np.arange(3,8,1), dtype='int32'),
                   'E': pd.Categorical(['test', 'train', 'test', 'train','test']),
                   'F': 'foo'})
ex
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>E</th>
      <th>F</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.0</td>
      <td>2013-01-02</td>
      <td>1.0</td>
      <td>3</td>
      <td>test</td>
      <td>foo</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.0</td>
      <td>2013-01-02</td>
      <td>1.0</td>
      <td>4</td>
      <td>train</td>
      <td>foo</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1.0</td>
      <td>2013-01-02</td>
      <td>1.0</td>
      <td>5</td>
      <td>test</td>
      <td>foo</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1.0</td>
      <td>2013-01-02</td>
      <td>1.0</td>
      <td>6</td>
      <td>train</td>
      <td>foo</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.0</td>
      <td>2013-01-02</td>
      <td>1.0</td>
      <td>7</td>
      <td>test</td>
      <td>foo</td>
    </tr>
  </tbody>
</table>
</div>


- DataFrame의 컬럼들은 각기 특별한 자료형을 갖고 있음

- 이는 DataFrame 내에 있는 dtypes라는 속성을 통해 확인 가능함

- 파이썬의 기본적인 소수점은 float64로 잡히고, 기본적은 문자열은 str이 아니라 object라는 자료형으로 나타남



```python
ex.dtypes
```

<pre>
A           float64
B    datetime64[ns]
C           float32
D             int32
E          category
F            object
dtype: object
</pre>

```python
ex2 = pd.DataFrame(np.random.randn(5,2), columns = ['A','B'])
ex2
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.406282</td>
      <td>0.368642</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.140642</td>
      <td>-0.077410</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.812914</td>
      <td>-0.273615</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.448528</td>
      <td>0.854089</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-0.237988</td>
      <td>0.196074</td>
    </tr>
  </tbody>
</table>
</div>



```python
ex2.head() #기본값 = 5
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.406282</td>
      <td>0.368642</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.140642</td>
      <td>-0.077410</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.812914</td>
      <td>-0.273615</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.448528</td>
      <td>0.854089</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-0.237988</td>
      <td>0.196074</td>
    </tr>
  </tbody>
</table>
</div>



```python
ex2.tail(3)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>-0.812914</td>
      <td>-0.273615</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.448528</td>
      <td>0.854089</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-0.237988</td>
      <td>0.196074</td>
    </tr>
  </tbody>
</table>
</div>



```python
ex2.index #check index
```

<pre>
RangeIndex(start=0, stop=5, step=1)
</pre>

```python
ex2.columns #check columns
```

<pre>
Index(['A', 'B'], dtype='object')
</pre>

```python
ex2.values #check values
```

<pre>
array([[ 0.40628183,  0.36864168],
       [ 1.14064213, -0.07741036],
       [-0.81291384, -0.27361517],
       [-0.44852821,  0.85408865],
       [-0.23798752,  0.1960742 ]])
</pre>

```python
ex2.describe() #simple statistic information of DataFrame
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>5.000000</td>
      <td>5.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.009499</td>
      <td>0.213556</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.772063</td>
      <td>0.434924</td>
    </tr>
    <tr>
      <th>min</th>
      <td>-0.812914</td>
      <td>-0.273615</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>-0.448528</td>
      <td>-0.077410</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>-0.237988</td>
      <td>0.196074</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.406282</td>
      <td>0.368642</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1.140642</td>
      <td>0.854089</td>
    </tr>
  </tbody>
</table>
</div>



```python
ex2.sort_index(axis =1 , ascending = False)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>B</th>
      <th>A</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.368642</td>
      <td>0.406282</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-0.077410</td>
      <td>1.140642</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.273615</td>
      <td>-0.812914</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.854089</td>
      <td>-0.448528</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.196074</td>
      <td>-0.237988</td>
    </tr>
  </tbody>
</table>
</div>



```python
ex2.sort_index(axis = 0 , ascending = False)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>-0.237988</td>
      <td>0.196074</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.448528</td>
      <td>0.854089</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.812914</td>
      <td>-0.273615</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.140642</td>
      <td>-0.077410</td>
    </tr>
    <tr>
      <th>0</th>
      <td>0.406282</td>
      <td>0.368642</td>
    </tr>
  </tbody>
</table>
</div>



```python
ex2.sort_index()
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.406282</td>
      <td>0.368642</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.140642</td>
      <td>-0.077410</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.812914</td>
      <td>-0.273615</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.448528</td>
      <td>0.854089</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-0.237988</td>
      <td>0.196074</td>
    </tr>
  </tbody>
</table>
</div>



```python
ex2.sort_values(by='B')
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>-0.812914</td>
      <td>-0.273615</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.140642</td>
      <td>-0.077410</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-0.237988</td>
      <td>0.196074</td>
    </tr>
    <tr>
      <th>0</th>
      <td>0.406282</td>
      <td>0.368642</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.448528</td>
      <td>0.854089</td>
    </tr>
  </tbody>
</table>
</div>


### Selection using pandas 



```python
ex2
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.406282</td>
      <td>0.368642</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.140642</td>
      <td>-0.077410</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.812914</td>
      <td>-0.273615</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.448528</td>
      <td>0.854089</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-0.237988</td>
      <td>0.196074</td>
    </tr>
  </tbody>
</table>
</div>



```python
ex2['A']
```

<pre>
0    0.406282
1    1.140642
2   -0.812914
3   -0.448528
4   -0.237988
Name: A, dtype: float64
</pre>

```python
ex2.A
```

<pre>
0    0.406282
1    1.140642
2   -0.812914
3   -0.448528
4   -0.237988
Name: A, dtype: float64
</pre>

```python
ex2[['A']]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.406282</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.140642</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.812914</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.448528</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-0.237988</td>
    </tr>
  </tbody>
</table>
</div>



```python
type(ex2['A'])
```

<pre>
pandas.core.series.Series
</pre>

```python
type(ex2[['A']])
```

<pre>
pandas.core.frame.DataFrame
</pre>

```python
ex2[0:3]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.406282</td>
      <td>0.368642</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.140642</td>
      <td>-0.077410</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.812914</td>
      <td>-0.273615</td>
    </tr>
  </tbody>
</table>
</div>


## Merge DataFrame  



```python
df1 = pd.DataFrame({'key' : list('ABCDE'),
                    'value' : np.random.randn(5)})
df1
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>key</th>
      <th>value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>-0.604176</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>-0.882808</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>-0.253994</td>
    </tr>
    <tr>
      <th>3</th>
      <td>D</td>
      <td>0.461608</td>
    </tr>
    <tr>
      <th>4</th>
      <td>E</td>
      <td>-0.507770</td>
    </tr>
  </tbody>
</table>
</div>



```python
df2 = pd.DataFrame({'key' : list('ABCXZ'),
                    'value' : np.random.randn(5)})
df2
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>key</th>
      <th>value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>0.071220</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>0.957223</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>0.622761</td>
    </tr>
    <tr>
      <th>3</th>
      <td>X</td>
      <td>1.802048</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Z</td>
      <td>-0.531795</td>
    </tr>
  </tbody>
</table>
</div>



```python
pd.concat([df1,df2]) # axis = 0 (Default), concat by rows 
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>key</th>
      <th>value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>-0.604176</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>-0.882808</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>-0.253994</td>
    </tr>
    <tr>
      <th>3</th>
      <td>D</td>
      <td>0.461608</td>
    </tr>
    <tr>
      <th>4</th>
      <td>E</td>
      <td>-0.507770</td>
    </tr>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>0.071220</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>0.957223</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>0.622761</td>
    </tr>
    <tr>
      <th>3</th>
      <td>X</td>
      <td>1.802048</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Z</td>
      <td>-0.531795</td>
    </tr>
  </tbody>
</table>
</div>



```python
pd.concat([df1, df2], axis = 0, ignore_index = True)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>key</th>
      <th>value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>-0.604176</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>-0.882808</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>-0.253994</td>
    </tr>
    <tr>
      <th>3</th>
      <td>D</td>
      <td>0.461608</td>
    </tr>
    <tr>
      <th>4</th>
      <td>E</td>
      <td>-0.507770</td>
    </tr>
    <tr>
      <th>5</th>
      <td>A</td>
      <td>0.071220</td>
    </tr>
    <tr>
      <th>6</th>
      <td>B</td>
      <td>0.957223</td>
    </tr>
    <tr>
      <th>7</th>
      <td>C</td>
      <td>0.622761</td>
    </tr>
    <tr>
      <th>8</th>
      <td>X</td>
      <td>1.802048</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Z</td>
      <td>-0.531795</td>
    </tr>
  </tbody>
</table>
</div>



```python
pd.concat([df1, df2]).reset_index()
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>index</th>
      <th>key</th>
      <th>value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>A</td>
      <td>-0.604176</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>B</td>
      <td>-0.882808</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>C</td>
      <td>-0.253994</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>D</td>
      <td>0.461608</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>E</td>
      <td>-0.507770</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0</td>
      <td>A</td>
      <td>0.071220</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1</td>
      <td>B</td>
      <td>0.957223</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2</td>
      <td>C</td>
      <td>0.622761</td>
    </tr>
    <tr>
      <th>8</th>
      <td>3</td>
      <td>X</td>
      <td>1.802048</td>
    </tr>
    <tr>
      <th>9</th>
      <td>4</td>
      <td>Z</td>
      <td>-0.531795</td>
    </tr>
  </tbody>
</table>
</div>



```python
pd.concat([df1,df2], axis = 1)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>key</th>
      <th>value</th>
      <th>key</th>
      <th>value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>-0.604176</td>
      <td>A</td>
      <td>0.071220</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>-0.882808</td>
      <td>B</td>
      <td>0.957223</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>-0.253994</td>
      <td>C</td>
      <td>0.622761</td>
    </tr>
    <tr>
      <th>3</th>
      <td>D</td>
      <td>0.461608</td>
      <td>X</td>
      <td>1.802048</td>
    </tr>
    <tr>
      <th>4</th>
      <td>E</td>
      <td>-0.507770</td>
      <td>Z</td>
      <td>-0.531795</td>
    </tr>
  </tbody>
</table>
</div>



```python
df2.columns = ['key','values2']
df2
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>key</th>
      <th>values2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>0.071220</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>0.957223</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>0.622761</td>
    </tr>
    <tr>
      <th>3</th>
      <td>X</td>
      <td>1.802048</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Z</td>
      <td>-0.531795</td>
    </tr>
  </tbody>
</table>
</div>



```python
pd.concat([df1,df2])
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>key</th>
      <th>value</th>
      <th>values2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>-0.604176</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>-0.882808</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>-0.253994</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>D</td>
      <td>0.461608</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>E</td>
      <td>-0.507770</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>NaN</td>
      <td>0.071220</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>NaN</td>
      <td>0.957223</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>NaN</td>
      <td>0.622761</td>
    </tr>
    <tr>
      <th>3</th>
      <td>X</td>
      <td>NaN</td>
      <td>1.802048</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Z</td>
      <td>NaN</td>
      <td>-0.531795</td>
    </tr>
  </tbody>
</table>
</div>


###  pd.merge()



```python
df1
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>key</th>
      <th>value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>-0.604176</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>-0.882808</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>-0.253994</td>
    </tr>
    <tr>
      <th>3</th>
      <td>D</td>
      <td>0.461608</td>
    </tr>
    <tr>
      <th>4</th>
      <td>E</td>
      <td>-0.507770</td>
    </tr>
  </tbody>
</table>
</div>



```python
df2
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>key</th>
      <th>values2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>0.071220</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>0.957223</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>0.622761</td>
    </tr>
    <tr>
      <th>3</th>
      <td>X</td>
      <td>1.802048</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Z</td>
      <td>-0.531795</td>
    </tr>
  </tbody>
</table>
</div>



```python
pd.merge(df1, df2, on = 'key', how = 'inner')
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>key</th>
      <th>value</th>
      <th>values2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>-0.604176</td>
      <td>0.071220</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>-0.882808</td>
      <td>0.957223</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>-0.253994</td>
      <td>0.622761</td>
    </tr>
  </tbody>
</table>
</div>



```python
pd.merge(df1, df2, on = 'key', how = 'left')
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>key</th>
      <th>value</th>
      <th>values2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>-0.604176</td>
      <td>0.071220</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>-0.882808</td>
      <td>0.957223</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>-0.253994</td>
      <td>0.622761</td>
    </tr>
    <tr>
      <th>3</th>
      <td>D</td>
      <td>0.461608</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>E</td>
      <td>-0.507770</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



```python
pd.merge(df1, df2, on = 'key', how = 'right')
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>key</th>
      <th>value</th>
      <th>values2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>-0.604176</td>
      <td>0.071220</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>-0.882808</td>
      <td>0.957223</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>-0.253994</td>
      <td>0.622761</td>
    </tr>
    <tr>
      <th>3</th>
      <td>X</td>
      <td>NaN</td>
      <td>1.802048</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Z</td>
      <td>NaN</td>
      <td>-0.531795</td>
    </tr>
  </tbody>
</table>
</div>



```python
pd.merge(df1, df2, on = 'key', how = 'outer')
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>key</th>
      <th>value</th>
      <th>values2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A</td>
      <td>-0.604176</td>
      <td>0.071220</td>
    </tr>
    <tr>
      <th>1</th>
      <td>B</td>
      <td>-0.882808</td>
      <td>0.957223</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C</td>
      <td>-0.253994</td>
      <td>0.622761</td>
    </tr>
    <tr>
      <th>3</th>
      <td>D</td>
      <td>0.461608</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>E</td>
      <td>-0.507770</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>X</td>
      <td>NaN</td>
      <td>1.802048</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Z</td>
      <td>NaN</td>
      <td>-0.531795</td>
    </tr>
  </tbody>
</table>
</div>


### 4.  Practice using data set - iris dataset



```python
from sklearn.datasets import load_iris
```


```python
print(iris) # 로드된 데이터가 속성-스타일 접근을 제공하는 딕셔너리와 번치 객체로 표현된 것을 확인
print(iris.DESCR) # Description 속성을 이용해서 데이터셋의 정보를 확인

# 각 key에 저장된 value 확인
# feature
print(iris.data)
print(iris.feature_names)

# label
print(iris.target)
print(iris.target_names)

# feature_names 와 target을 레코드로 갖는 데이터프레임 생성
df = pd.DataFrame(data=iris.data, columns=iris.feature_names)
```

<pre>
{'data': array([[5.1, 3.5, 1.4, 0.2],
       [4.9, 3. , 1.4, 0.2],
       [4.7, 3.2, 1.3, 0.2],
       [4.6, 3.1, 1.5, 0.2],
       [5. , 3.6, 1.4, 0.2],
       [5.4, 3.9, 1.7, 0.4],
       [4.6, 3.4, 1.4, 0.3],
       [5. , 3.4, 1.5, 0.2],
       [4.4, 2.9, 1.4, 0.2],
       [4.9, 3.1, 1.5, 0.1],
       [5.4, 3.7, 1.5, 0.2],
       [4.8, 3.4, 1.6, 0.2],
       [4.8, 3. , 1.4, 0.1],
       [4.3, 3. , 1.1, 0.1],
       [5.8, 4. , 1.2, 0.2],
       [5.7, 4.4, 1.5, 0.4],
       [5.4, 3.9, 1.3, 0.4],
       [5.1, 3.5, 1.4, 0.3],
       [5.7, 3.8, 1.7, 0.3],
       [5.1, 3.8, 1.5, 0.3],
       [5.4, 3.4, 1.7, 0.2],
       [5.1, 3.7, 1.5, 0.4],
       [4.6, 3.6, 1. , 0.2],
       [5.1, 3.3, 1.7, 0.5],
       [4.8, 3.4, 1.9, 0.2],
       [5. , 3. , 1.6, 0.2],
       [5. , 3.4, 1.6, 0.4],
       [5.2, 3.5, 1.5, 0.2],
       [5.2, 3.4, 1.4, 0.2],
       [4.7, 3.2, 1.6, 0.2],
       [4.8, 3.1, 1.6, 0.2],
       [5.4, 3.4, 1.5, 0.4],
       [5.2, 4.1, 1.5, 0.1],
       [5.5, 4.2, 1.4, 0.2],
       [4.9, 3.1, 1.5, 0.2],
       [5. , 3.2, 1.2, 0.2],
       [5.5, 3.5, 1.3, 0.2],
       [4.9, 3.6, 1.4, 0.1],
       [4.4, 3. , 1.3, 0.2],
       [5.1, 3.4, 1.5, 0.2],
       [5. , 3.5, 1.3, 0.3],
       [4.5, 2.3, 1.3, 0.3],
       [4.4, 3.2, 1.3, 0.2],
       [5. , 3.5, 1.6, 0.6],
       [5.1, 3.8, 1.9, 0.4],
       [4.8, 3. , 1.4, 0.3],
       [5.1, 3.8, 1.6, 0.2],
       [4.6, 3.2, 1.4, 0.2],
       [5.3, 3.7, 1.5, 0.2],
       [5. , 3.3, 1.4, 0.2],
       [7. , 3.2, 4.7, 1.4],
       [6.4, 3.2, 4.5, 1.5],
       [6.9, 3.1, 4.9, 1.5],
       [5.5, 2.3, 4. , 1.3],
       [6.5, 2.8, 4.6, 1.5],
       [5.7, 2.8, 4.5, 1.3],
       [6.3, 3.3, 4.7, 1.6],
       [4.9, 2.4, 3.3, 1. ],
       [6.6, 2.9, 4.6, 1.3],
       [5.2, 2.7, 3.9, 1.4],
       [5. , 2. , 3.5, 1. ],
       [5.9, 3. , 4.2, 1.5],
       [6. , 2.2, 4. , 1. ],
       [6.1, 2.9, 4.7, 1.4],
       [5.6, 2.9, 3.6, 1.3],
       [6.7, 3.1, 4.4, 1.4],
       [5.6, 3. , 4.5, 1.5],
       [5.8, 2.7, 4.1, 1. ],
       [6.2, 2.2, 4.5, 1.5],
       [5.6, 2.5, 3.9, 1.1],
       [5.9, 3.2, 4.8, 1.8],
       [6.1, 2.8, 4. , 1.3],
       [6.3, 2.5, 4.9, 1.5],
       [6.1, 2.8, 4.7, 1.2],
       [6.4, 2.9, 4.3, 1.3],
       [6.6, 3. , 4.4, 1.4],
       [6.8, 2.8, 4.8, 1.4],
       [6.7, 3. , 5. , 1.7],
       [6. , 2.9, 4.5, 1.5],
       [5.7, 2.6, 3.5, 1. ],
       [5.5, 2.4, 3.8, 1.1],
       [5.5, 2.4, 3.7, 1. ],
       [5.8, 2.7, 3.9, 1.2],
       [6. , 2.7, 5.1, 1.6],
       [5.4, 3. , 4.5, 1.5],
       [6. , 3.4, 4.5, 1.6],
       [6.7, 3.1, 4.7, 1.5],
       [6.3, 2.3, 4.4, 1.3],
       [5.6, 3. , 4.1, 1.3],
       [5.5, 2.5, 4. , 1.3],
       [5.5, 2.6, 4.4, 1.2],
       [6.1, 3. , 4.6, 1.4],
       [5.8, 2.6, 4. , 1.2],
       [5. , 2.3, 3.3, 1. ],
       [5.6, 2.7, 4.2, 1.3],
       [5.7, 3. , 4.2, 1.2],
       [5.7, 2.9, 4.2, 1.3],
       [6.2, 2.9, 4.3, 1.3],
       [5.1, 2.5, 3. , 1.1],
       [5.7, 2.8, 4.1, 1.3],
       [6.3, 3.3, 6. , 2.5],
       [5.8, 2.7, 5.1, 1.9],
       [7.1, 3. , 5.9, 2.1],
       [6.3, 2.9, 5.6, 1.8],
       [6.5, 3. , 5.8, 2.2],
       [7.6, 3. , 6.6, 2.1],
       [4.9, 2.5, 4.5, 1.7],
       [7.3, 2.9, 6.3, 1.8],
       [6.7, 2.5, 5.8, 1.8],
       [7.2, 3.6, 6.1, 2.5],
       [6.5, 3.2, 5.1, 2. ],
       [6.4, 2.7, 5.3, 1.9],
       [6.8, 3. , 5.5, 2.1],
       [5.7, 2.5, 5. , 2. ],
       [5.8, 2.8, 5.1, 2.4],
       [6.4, 3.2, 5.3, 2.3],
       [6.5, 3. , 5.5, 1.8],
       [7.7, 3.8, 6.7, 2.2],
       [7.7, 2.6, 6.9, 2.3],
       [6. , 2.2, 5. , 1.5],
       [6.9, 3.2, 5.7, 2.3],
       [5.6, 2.8, 4.9, 2. ],
       [7.7, 2.8, 6.7, 2. ],
       [6.3, 2.7, 4.9, 1.8],
       [6.7, 3.3, 5.7, 2.1],
       [7.2, 3.2, 6. , 1.8],
       [6.2, 2.8, 4.8, 1.8],
       [6.1, 3. , 4.9, 1.8],
       [6.4, 2.8, 5.6, 2.1],
       [7.2, 3. , 5.8, 1.6],
       [7.4, 2.8, 6.1, 1.9],
       [7.9, 3.8, 6.4, 2. ],
       [6.4, 2.8, 5.6, 2.2],
       [6.3, 2.8, 5.1, 1.5],
       [6.1, 2.6, 5.6, 1.4],
       [7.7, 3. , 6.1, 2.3],
       [6.3, 3.4, 5.6, 2.4],
       [6.4, 3.1, 5.5, 1.8],
       [6. , 3. , 4.8, 1.8],
       [6.9, 3.1, 5.4, 2.1],
       [6.7, 3.1, 5.6, 2.4],
       [6.9, 3.1, 5.1, 2.3],
       [5.8, 2.7, 5.1, 1.9],
       [6.8, 3.2, 5.9, 2.3],
       [6.7, 3.3, 5.7, 2.5],
       [6.7, 3. , 5.2, 2.3],
       [6.3, 2.5, 5. , 1.9],
       [6.5, 3. , 5.2, 2. ],
       [6.2, 3.4, 5.4, 2.3],
       [5.9, 3. , 5.1, 1.8]]), 'target': array([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
       0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
       0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
       1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,
       2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,
       2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2]), 'frame': None, 'target_names': array(['setosa', 'versicolor', 'virginica'], dtype='<U10'), 'DESCR': '.. _iris_dataset:\n\nIris plants dataset\n--------------------\n\n**Data Set Characteristics:**\n\n    :Number of Instances: 150 (50 in each of three classes)\n    :Number of Attributes: 4 numeric, predictive attributes and the class\n    :Attribute Information:\n        - sepal length in cm\n        - sepal width in cm\n        - petal length in cm\n        - petal width in cm\n        - class:\n                - Iris-Setosa\n                - Iris-Versicolour\n                - Iris-Virginica\n                \n    :Summary Statistics:\n\n    ============== ==== ==== ======= ===== ====================\n                    Min  Max   Mean    SD   Class Correlation\n    ============== ==== ==== ======= ===== ====================\n    sepal length:   4.3  7.9   5.84   0.83    0.7826\n    sepal width:    2.0  4.4   3.05   0.43   -0.4194\n    petal length:   1.0  6.9   3.76   1.76    0.9490  (high!)\n    petal width:    0.1  2.5   1.20   0.76    0.9565  (high!)\n    ============== ==== ==== ======= ===== ====================\n\n    :Missing Attribute Values: None\n    :Class Distribution: 33.3% for each of 3 classes.\n    :Creator: R.A. Fisher\n    :Donor: Michael Marshall (MARSHALL%PLU@io.arc.nasa.gov)\n    :Date: July, 1988\n\nThe famous Iris database, first used by Sir R.A. Fisher. The dataset is taken\nfrom Fisher\'s paper. Note that it\'s the same as in R, but not as in the UCI\nMachine Learning Repository, which has two wrong data points.\n\nThis is perhaps the best known database to be found in the\npattern recognition literature.  Fisher\'s paper is a classic in the field and\nis referenced frequently to this day.  (See Duda & Hart, for example.)  The\ndata set contains 3 classes of 50 instances each, where each class refers to a\ntype of iris plant.  One class is linearly separable from the other 2; the\nlatter are NOT linearly separable from each other.\n\n.. topic:: References\n\n   - Fisher, R.A. "The use of multiple measurements in taxonomic problems"\n     Annual Eugenics, 7, Part II, 179-188 (1936); also in "Contributions to\n     Mathematical Statistics" (John Wiley, NY, 1950).\n   - Duda, R.O., & Hart, P.E. (1973) Pattern Classification and Scene Analysis.\n     (Q327.D83) John Wiley & Sons.  ISBN 0-471-22361-1.  See page 218.\n   - Dasarathy, B.V. (1980) "Nosing Around the Neighborhood: A New System\n     Structure and Classification Rule for Recognition in Partially Exposed\n     Environments".  IEEE Transactions on Pattern Analysis and Machine\n     Intelligence, Vol. PAMI-2, No. 1, 67-71.\n   - Gates, G.W. (1972) "The Reduced Nearest Neighbor Rule".  IEEE Transactions\n     on Information Theory, May 1972, 431-433.\n   - See also: 1988 MLC Proceedings, 54-64.  Cheeseman et al"s AUTOCLASS II\n     conceptual clustering system finds 3 classes in the data.\n   - Many, many more ...', 'feature_names': ['sepal length (cm)', 'sepal width (cm)', 'petal length (cm)', 'petal width (cm)'], 'filename': 'iris.csv', 'data_module': 'sklearn.datasets.data'}
.. _iris_dataset:

Iris plants dataset
--------------------

**Data Set Characteristics:**

    :Number of Instances: 150 (50 in each of three classes)
    :Number of Attributes: 4 numeric, predictive attributes and the class
    :Attribute Information:
        - sepal length in cm
        - sepal width in cm
        - petal length in cm
        - petal width in cm
        - class:
                - Iris-Setosa
                - Iris-Versicolour
                - Iris-Virginica
                
    :Summary Statistics:

    ============== ==== ==== ======= ===== ====================
                    Min  Max   Mean    SD   Class Correlation
    ============== ==== ==== ======= ===== ====================
    sepal length:   4.3  7.9   5.84   0.83    0.7826
    sepal width:    2.0  4.4   3.05   0.43   -0.4194
    petal length:   1.0  6.9   3.76   1.76    0.9490  (high!)
    petal width:    0.1  2.5   1.20   0.76    0.9565  (high!)
    ============== ==== ==== ======= ===== ====================

    :Missing Attribute Values: None
    :Class Distribution: 33.3% for each of 3 classes.
    :Creator: R.A. Fisher
    :Donor: Michael Marshall (MARSHALL%PLU@io.arc.nasa.gov)
    :Date: July, 1988

The famous Iris database, first used by Sir R.A. Fisher. The dataset is taken
from Fisher's paper. Note that it's the same as in R, but not as in the UCI
Machine Learning Repository, which has two wrong data points.

This is perhaps the best known database to be found in the
pattern recognition literature.  Fisher's paper is a classic in the field and
is referenced frequently to this day.  (See Duda & Hart, for example.)  The
data set contains 3 classes of 50 instances each, where each class refers to a
type of iris plant.  One class is linearly separable from the other 2; the
latter are NOT linearly separable from each other.

.. topic:: References

   - Fisher, R.A. "The use of multiple measurements in taxonomic problems"
     Annual Eugenics, 7, Part II, 179-188 (1936); also in "Contributions to
     Mathematical Statistics" (John Wiley, NY, 1950).
   - Duda, R.O., & Hart, P.E. (1973) Pattern Classification and Scene Analysis.
     (Q327.D83) John Wiley & Sons.  ISBN 0-471-22361-1.  See page 218.
   - Dasarathy, B.V. (1980) "Nosing Around the Neighborhood: A New System
     Structure and Classification Rule for Recognition in Partially Exposed
     Environments".  IEEE Transactions on Pattern Analysis and Machine
     Intelligence, Vol. PAMI-2, No. 1, 67-71.
   - Gates, G.W. (1972) "The Reduced Nearest Neighbor Rule".  IEEE Transactions
     on Information Theory, May 1972, 431-433.
   - See also: 1988 MLC Proceedings, 54-64.  Cheeseman et al"s AUTOCLASS II
     conceptual clustering system finds 3 classes in the data.
   - Many, many more ...
[[5.1 3.5 1.4 0.2]
 [4.9 3.  1.4 0.2]
 [4.7 3.2 1.3 0.2]
 [4.6 3.1 1.5 0.2]
 [5.  3.6 1.4 0.2]
 [5.4 3.9 1.7 0.4]
 [4.6 3.4 1.4 0.3]
 [5.  3.4 1.5 0.2]
 [4.4 2.9 1.4 0.2]
 [4.9 3.1 1.5 0.1]
 [5.4 3.7 1.5 0.2]
 [4.8 3.4 1.6 0.2]
 [4.8 3.  1.4 0.1]
 [4.3 3.  1.1 0.1]
 [5.8 4.  1.2 0.2]
 [5.7 4.4 1.5 0.4]
 [5.4 3.9 1.3 0.4]
 [5.1 3.5 1.4 0.3]
 [5.7 3.8 1.7 0.3]
 [5.1 3.8 1.5 0.3]
 [5.4 3.4 1.7 0.2]
 [5.1 3.7 1.5 0.4]
 [4.6 3.6 1.  0.2]
 [5.1 3.3 1.7 0.5]
 [4.8 3.4 1.9 0.2]
 [5.  3.  1.6 0.2]
 [5.  3.4 1.6 0.4]
 [5.2 3.5 1.5 0.2]
 [5.2 3.4 1.4 0.2]
 [4.7 3.2 1.6 0.2]
 [4.8 3.1 1.6 0.2]
 [5.4 3.4 1.5 0.4]
 [5.2 4.1 1.5 0.1]
 [5.5 4.2 1.4 0.2]
 [4.9 3.1 1.5 0.2]
 [5.  3.2 1.2 0.2]
 [5.5 3.5 1.3 0.2]
 [4.9 3.6 1.4 0.1]
 [4.4 3.  1.3 0.2]
 [5.1 3.4 1.5 0.2]
 [5.  3.5 1.3 0.3]
 [4.5 2.3 1.3 0.3]
 [4.4 3.2 1.3 0.2]
 [5.  3.5 1.6 0.6]
 [5.1 3.8 1.9 0.4]
 [4.8 3.  1.4 0.3]
 [5.1 3.8 1.6 0.2]
 [4.6 3.2 1.4 0.2]
 [5.3 3.7 1.5 0.2]
 [5.  3.3 1.4 0.2]
 [7.  3.2 4.7 1.4]
 [6.4 3.2 4.5 1.5]
 [6.9 3.1 4.9 1.5]
 [5.5 2.3 4.  1.3]
 [6.5 2.8 4.6 1.5]
 [5.7 2.8 4.5 1.3]
 [6.3 3.3 4.7 1.6]
 [4.9 2.4 3.3 1. ]
 [6.6 2.9 4.6 1.3]
 [5.2 2.7 3.9 1.4]
 [5.  2.  3.5 1. ]
 [5.9 3.  4.2 1.5]
 [6.  2.2 4.  1. ]
 [6.1 2.9 4.7 1.4]
 [5.6 2.9 3.6 1.3]
 [6.7 3.1 4.4 1.4]
 [5.6 3.  4.5 1.5]
 [5.8 2.7 4.1 1. ]
 [6.2 2.2 4.5 1.5]
 [5.6 2.5 3.9 1.1]
 [5.9 3.2 4.8 1.8]
 [6.1 2.8 4.  1.3]
 [6.3 2.5 4.9 1.5]
 [6.1 2.8 4.7 1.2]
 [6.4 2.9 4.3 1.3]
 [6.6 3.  4.4 1.4]
 [6.8 2.8 4.8 1.4]
 [6.7 3.  5.  1.7]
 [6.  2.9 4.5 1.5]
 [5.7 2.6 3.5 1. ]
 [5.5 2.4 3.8 1.1]
 [5.5 2.4 3.7 1. ]
 [5.8 2.7 3.9 1.2]
 [6.  2.7 5.1 1.6]
 [5.4 3.  4.5 1.5]
 [6.  3.4 4.5 1.6]
 [6.7 3.1 4.7 1.5]
 [6.3 2.3 4.4 1.3]
 [5.6 3.  4.1 1.3]
 [5.5 2.5 4.  1.3]
 [5.5 2.6 4.4 1.2]
 [6.1 3.  4.6 1.4]
 [5.8 2.6 4.  1.2]
 [5.  2.3 3.3 1. ]
 [5.6 2.7 4.2 1.3]
 [5.7 3.  4.2 1.2]
 [5.7 2.9 4.2 1.3]
 [6.2 2.9 4.3 1.3]
 [5.1 2.5 3.  1.1]
 [5.7 2.8 4.1 1.3]
 [6.3 3.3 6.  2.5]
 [5.8 2.7 5.1 1.9]
 [7.1 3.  5.9 2.1]
 [6.3 2.9 5.6 1.8]
 [6.5 3.  5.8 2.2]
 [7.6 3.  6.6 2.1]
 [4.9 2.5 4.5 1.7]
 [7.3 2.9 6.3 1.8]
 [6.7 2.5 5.8 1.8]
 [7.2 3.6 6.1 2.5]
 [6.5 3.2 5.1 2. ]
 [6.4 2.7 5.3 1.9]
 [6.8 3.  5.5 2.1]
 [5.7 2.5 5.  2. ]
 [5.8 2.8 5.1 2.4]
 [6.4 3.2 5.3 2.3]
 [6.5 3.  5.5 1.8]
 [7.7 3.8 6.7 2.2]
 [7.7 2.6 6.9 2.3]
 [6.  2.2 5.  1.5]
 [6.9 3.2 5.7 2.3]
 [5.6 2.8 4.9 2. ]
 [7.7 2.8 6.7 2. ]
 [6.3 2.7 4.9 1.8]
 [6.7 3.3 5.7 2.1]
 [7.2 3.2 6.  1.8]
 [6.2 2.8 4.8 1.8]
 [6.1 3.  4.9 1.8]
 [6.4 2.8 5.6 2.1]
 [7.2 3.  5.8 1.6]
 [7.4 2.8 6.1 1.9]
 [7.9 3.8 6.4 2. ]
 [6.4 2.8 5.6 2.2]
 [6.3 2.8 5.1 1.5]
 [6.1 2.6 5.6 1.4]
 [7.7 3.  6.1 2.3]
 [6.3 3.4 5.6 2.4]
 [6.4 3.1 5.5 1.8]
 [6.  3.  4.8 1.8]
 [6.9 3.1 5.4 2.1]
 [6.7 3.1 5.6 2.4]
 [6.9 3.1 5.1 2.3]
 [5.8 2.7 5.1 1.9]
 [6.8 3.2 5.9 2.3]
 [6.7 3.3 5.7 2.5]
 [6.7 3.  5.2 2.3]
 [6.3 2.5 5.  1.9]
 [6.5 3.  5.2 2. ]
 [6.2 3.4 5.4 2.3]
 [5.9 3.  5.1 1.8]]
['sepal length (cm)', 'sepal width (cm)', 'petal length (cm)', 'petal width (cm)']
[0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
 0 0 0 0 0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 2
 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
 2 2]
['setosa' 'versicolor' 'virginica']
</pre>

```python
df.head()
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal length (cm)</th>
      <th>sepal width (cm)</th>
      <th>petal length (cm)</th>
      <th>petal width (cm)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
  </tbody>
</table>
</div>



```python
df.tail()
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal length (cm)</th>
      <th>sepal width (cm)</th>
      <th>petal length (cm)</th>
      <th>petal width (cm)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>145</th>
      <td>6.7</td>
      <td>3.0</td>
      <td>5.2</td>
      <td>2.3</td>
    </tr>
    <tr>
      <th>146</th>
      <td>6.3</td>
      <td>2.5</td>
      <td>5.0</td>
      <td>1.9</td>
    </tr>
    <tr>
      <th>147</th>
      <td>6.5</td>
      <td>3.0</td>
      <td>5.2</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>148</th>
      <td>6.2</td>
      <td>3.4</td>
      <td>5.4</td>
      <td>2.3</td>
    </tr>
    <tr>
      <th>149</th>
      <td>5.9</td>
      <td>3.0</td>
      <td>5.1</td>
      <td>1.8</td>
    </tr>
  </tbody>
</table>
</div>



```python
df.index #인덱스 확인
```

<pre>
RangeIndex(start=0, stop=150, step=1)
</pre>

```python
df.columns #컬럼 확인
```

<pre>
Index(['sepal length (cm)', 'sepal width (cm)', 'petal length (cm)',
       'petal width (cm)'],
      dtype='object')
</pre>

```python
df.dtypes #형식 확인
```

<pre>
sepal length (cm)    float64
sepal width (cm)     float64
petal length (cm)    float64
petal width (cm)     float64
dtype: object
</pre>

```python
df[['sepal length (cm)', 'sepal width (cm)']] = df[['sepal length (cm)', 'sepal width (cm)']].astype(object)
df.dtypes #형식 변경
```

<pre>
sepal length (cm)     object
sepal width (cm)      object
petal length (cm)    float64
petal width (cm)     float64
dtype: object
</pre>

```python
df[['sepal length (cm)', 'sepal width (cm)']] = df[['sepal length (cm)', 'sepal width (cm)']].astype(float)
df.dtypes
```

<pre>
sepal length (cm)    float64
sepal width (cm)     float64
petal length (cm)    float64
petal width (cm)     float64
dtype: object
</pre>

```python
df.info() #데이터 타입, 각 아이템 개수 확인
```

<pre>
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 150 entries, 0 to 149
Data columns (total 4 columns):
 #   Column             Non-Null Count  Dtype  
---  ------             --------------  -----  
 0   sepal length (cm)  150 non-null    float64
 1   sepal width (cm)   150 non-null    float64
 2   petal length (cm)  150 non-null    float64
 3   petal width (cm)   150 non-null    float64
dtypes: float64(4)
memory usage: 4.8 KB
</pre>
### 4.2 데이터 전처리 



```python
df = df.rename(columns={'sepal length (cm)': 'sepal length', 'sepal width (cm)': 'sepal width',
                        'petal length (cm)' : 'petal length', 'petal width (cm)': 'petal width',
                        'variety' : 'species'}) #변수 이름 변경
df.head()
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal length</th>
      <th>sepal width</th>
      <th>petal length</th>
      <th>petal width</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
  </tbody>
</table>
</div>


Dataframe column 선택

 - dataframe[ ] 으로 컬럼 추출

 - [] -> Series로 변환

 - [[]] -> dataframe으로 반환



```python
df.columns
```

<pre>
Index(['sepal length', 'sepal width', 'petal length', 'petal width'], dtype='object')
</pre>

```python
df['sepal length']
```

<pre>
0      5.1
1      4.9
2      4.7
3      4.6
4      5.0
      ... 
145    6.7
146    6.3
147    6.5
148    6.2
149    5.9
Name: sepal length, Length: 150, dtype: float64
</pre>

```python
df[['sepal length']]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal length</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.7</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.6</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>145</th>
      <td>6.7</td>
    </tr>
    <tr>
      <th>146</th>
      <td>6.3</td>
    </tr>
    <tr>
      <th>147</th>
      <td>6.5</td>
    </tr>
    <tr>
      <th>148</th>
      <td>6.2</td>
    </tr>
    <tr>
      <th>149</th>
      <td>5.9</td>
    </tr>
  </tbody>
</table>
<p>150 rows × 1 columns</p>
</div>



```python
df[['sepal length', 'sepal width']]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal length</th>
      <th>sepal width</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
      <td>3.5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.7</td>
      <td>3.2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.6</td>
      <td>3.1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
      <td>3.6</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>145</th>
      <td>6.7</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>146</th>
      <td>6.3</td>
      <td>2.5</td>
    </tr>
    <tr>
      <th>147</th>
      <td>6.5</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>148</th>
      <td>6.2</td>
      <td>3.4</td>
    </tr>
    <tr>
      <th>149</th>
      <td>5.9</td>
      <td>3.0</td>
    </tr>
  </tbody>
</table>
<p>150 rows × 2 columns</p>
</div>


dataframe row 선택

- dataframe의 경우 []연산자는 컬럼(column) 선택, 하지만 슬라이싱(slicing)은 행(row) 선택

- .loc(),iloc()로 행 선택 가능

 - .loc() : 인덱스 자체를 사용

 - .iloc() : 0 based 인덱스 사용



```python
df.head(10)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal length</th>
      <th>sepal width</th>
      <th>petal length</th>
      <th>petal width</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5.4</td>
      <td>3.9</td>
      <td>1.7</td>
      <td>0.4</td>
    </tr>
    <tr>
      <th>6</th>
      <td>4.6</td>
      <td>3.4</td>
      <td>1.4</td>
      <td>0.3</td>
    </tr>
    <tr>
      <th>7</th>
      <td>5.0</td>
      <td>3.4</td>
      <td>1.5</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>8</th>
      <td>4.4</td>
      <td>2.9</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>9</th>
      <td>4.9</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.1</td>
    </tr>
  </tbody>
</table>
</div>



```python
df[0:5]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal length</th>
      <th>sepal width</th>
      <th>petal length</th>
      <th>petal width</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
  </tbody>
</table>
</div>



```python
df.index = df.index + 100
df.head()
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal length</th>
      <th>sepal width</th>
      <th>petal length</th>
      <th>petal width</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>100</th>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>101</th>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>102</th>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>103</th>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>104</th>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
  </tbody>
</table>
</div>



```python
df.loc[[100]]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal length</th>
      <th>sepal width</th>
      <th>petal length</th>
      <th>petal width</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>100</th>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
  </tbody>
</table>
</div>



```python
df.iloc[[30]]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal length</th>
      <th>sepal width</th>
      <th>petal length</th>
      <th>petal width</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>130</th>
      <td>4.8</td>
      <td>3.1</td>
      <td>1.6</td>
      <td>0.2</td>
    </tr>
  </tbody>
</table>
</div>



```python
df.iloc[[0]]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal length</th>
      <th>sepal width</th>
      <th>petal length</th>
      <th>petal width</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>100</th>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
  </tbody>
</table>
</div>



```python
df.loc[[100,101,102],["sepal length", "sepal width"]]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal length</th>
      <th>sepal width</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>100</th>
      <td>5.1</td>
      <td>3.5</td>
    </tr>
    <tr>
      <th>101</th>
      <td>4.9</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>102</th>
      <td>4.7</td>
      <td>3.2</td>
    </tr>
  </tbody>
</table>
</div>



```python
df.iloc[[0,1,2],[0,3]]
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sepal length</th>
      <th>petal width</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>100</th>
      <td>5.1</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>101</th>
      <td>4.9</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>102</th>
      <td>4.7</td>
      <td>0.2</td>
    </tr>
  </tbody>
</table>
</div>

