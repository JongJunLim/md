---
title: Numpy
layout: default
parent: Python
has_children: true
nav_order: 2
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


## 1. Numpy란?

 - Numerical python의 줄임말로써 고성능의 수치 계산을 하기 위해 만들어진 파이썬 package

 - Numpy는 과학 계산을 위한 수치해석용 라이브러리로서 다차원 배열을 처리하는데 필요한 여러 유용한 기능을 제공하고 있음

 - 기본적으로 array라는 자료 구조를 제공하며 선형대수용 행렬, 벡터 수학 계산을 위한 자료구조와 계산 함수를 제공

 - 보통 과학용 일반 함수 목록으로 SciPy, 차트용 라이브러리인 Matplotlib, 고수준 DataFrame 제공 모듈인 Pandas와 함께 사용


### 1.1 Numpy 특징

 - 특징, 메트릭스, 고수준의 배열은 과학계산 컴퓨팅에 있어 필수 도구라 할 수 있음

  - 입력 값 세트를 통해 계산이 반복될 때, 배열로 데이터를 나타내는 것이 자연스럽고 장점이 많음

  - 파이썬 과학계산 환경에서, 배열을 다루기 좋은 구조를 제공하는 라이브러리가 Numpy임

 

 - Numpy의 핵심 기능은 C로 구현되어 있어 배열을 계산하고 처리하는데 효율적인 기능들을 제공하고, 속도 또한 빠름

  - 파이썬 프로그램이 일반적으로 C에 비해 느린 이유는 for 루프문 때문인데, Numpy의 벡터 연산을 이용하면 for 루프면 없이 빠르게 동작하는 코드를 작성할 수 있음

 

 - 첫눈에 보기에 Numpy 배열은 파이썬 리스트 데이터 구조와 유사해보임

  - 하지만 파이썬 리스트는 다양한 객체를 담을 수 있는 컨테이너인데 반해 Numpy 배열은 동일한 자료형만 담을 수 있다는 점이 중요한 차이점

  

 - Numpy에는 고수준의 데이터 분석 기능을 제공하지는 않음

  - 그러므로 나중에 Scipy나 Pandas 같은 도구를 사용해 데이터 분석을 수행하는 것이 효과적임



```python
import numpy as np
```


```python
list1 = [7,2,8,10]

print(list1)
print(type(list1))
```

<pre>
[7, 2, 8, 10]
<class 'list'>
</pre>

```python
array1 = np.array(list1)
print(array1)
print(type(array1))
```

<pre>
[ 7  2  8 10]
<class 'numpy.ndarray'>
</pre>

```python
list2 = [[5.2,3.0,4.5],[9.1,0.1,0.3]]
print(list2)
print(type(list2))
```

<pre>
[[5.2, 3.0, 4.5], [9.1, 0.1, 0.3]]
<class 'list'>
</pre>

```python
array2 = np.array(list2)

print(array2)
print(type(array2))
```

<pre>
[[5.2 3.  4.5]
 [9.1 0.1 0.3]]
<class 'numpy.ndarray'>
</pre>

```python
array1.shape
```

<pre>
(4,)
</pre>

```python
array2.shape
```

<pre>
(2, 3)
</pre>

```python
a = np.array([1,2,3,4])

print(a)
print(type(a))
print(a.shape)
```

<pre>
[1 2 3 4]
<class 'numpy.ndarray'>
(4,)
</pre>

```python
b = np.array([[1,2,3,4]])

print(b)
print(type(b))
print(b.shape)
```

<pre>
[[1 2 3 4]]
<class 'numpy.ndarray'>
(1, 4)
</pre>
### Transpose

 - np.transpose()

 - .T



```python
print(a)
print(a.T)
```

<pre>
[1 2 3 4]
[1 2 3 4]
</pre>

```python
print(b)
print(b.T)
```

<pre>
[[1 2 3 4]]
[[1]
 [2]
 [3]
 [4]]
</pre>

```python
array3 = np.array([[[0,1],[2,3],[4,5]],
                  [[6,7],[8,9],[10,11]],
                  [[12,13],[14,15],[16,17]],
                  [[18,19],[20,21],[22,23]]])

print(array3)
print(array3.shape)
```

<pre>
[[[ 0  1]
  [ 2  3]
  [ 4  5]]

 [[ 6  7]
  [ 8  9]
  [10 11]]

 [[12 13]
  [14 15]
  [16 17]]

 [[18 19]
  [20 21]
  [22 23]]]
(4, 3, 2)
</pre>
### 2.2 Numpy array 변형하기

 - np.arange()



```python
np.arange(10)
```

<pre>
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
</pre>

```python
np.arange(1,10)
```

<pre>
array([1, 2, 3, 4, 5, 6, 7, 8, 9])
</pre>

```python
np.arange(1,10,2)
```

<pre>
array([1, 3, 5, 7, 9])
</pre>
- reshape



```python
    a = np.arange(1,11)
    print(a)
    print(a.shape)
```

<pre>
[ 1  2  3  4  5  6  7  8  9 10]
(10,)
</pre>

```python
b = a.reshape(2,5)
print(b)
print(b.shape)
```

<pre>
[[ 1  2  3  4  5]
 [ 6  7  8  9 10]]
(2, 5)
</pre>

```python
c = a.reshape(5,2)
print(c)
print(c.shape)
```

<pre>
[[ 1  2]
 [ 3  4]
 [ 5  6]
 [ 7  8]
 [ 9 10]]
(5, 2)
</pre>

```python
d = c.reshape(10,)
print(d)
print(d.shape)
```

<pre>
[ 1  2  3  4  5  6  7  8  9 10]
(10,)
</pre>

```python
e = d.reshape(-1,2)
print(e)
print(e.shape)
```

<pre>
[[ 1  2]
 [ 3  4]
 [ 5  6]
 [ 7  8]
 [ 9 10]]
(5, 2)
</pre>

```python
f = d.reshape(2,-1)
print(f)
print(f.shape)
```

<pre>
[[ 1  2  3  4  5]
 [ 6  7  8  9 10]]
(2, 5)
</pre>
### 2.3 numpy에서 제공하는 함수를 사용하여 numpy array 만들기




```python
np.zeros((2,2)) #모든 값 0
```

<pre>
array([[0., 0.],
       [0., 0.]])
</pre>

```python
np.ones((2,3)) #모든 값 1
```

<pre>
array([[1., 1., 1.],
       [1., 1., 1.]])
</pre>

```python
np.full((2,3),5) #사용자가 지정한 특정 값을 모든 값으로 하는 array 생성
```

<pre>
array([[5, 5, 5],
       [5, 5, 5]])
</pre>

```python
np.eye(3) #단위 행렬
```

<pre>
array([[1., 0., 0.],
       [0., 1., 0.],
       [0., 0., 1.]])
</pre>

```python
x = np.array([[0,1,2],[3,4,5]])

print(x)
x
```

<pre>
[[0 1 2]
 [3 4 5]]
</pre>
<pre>
array([[0, 1, 2],
       [3, 4, 5]])
</pre>

```python
np.zeros_like(x) #모든 값을 0으로 대체하고 싶을때 사용
```

<pre>
array([[0, 0, 0],
       [0, 0, 0]])
</pre>
### random 서브모듈 함수를 통해 난수 생성



 - rand 함수

  - 0,1 사이의 균일 분포에서 난수 matrix array 생성



```python
np.random.rand(2,3,3)
```

<pre>
array([[[0.30531114, 0.50297347, 0.02134391],
        [0.91955662, 0.81454713, 0.40047712],
        [0.88202009, 0.13273599, 0.81733349]],

       [[0.26466386, 0.05006526, 0.5808811 ],
        [0.35161347, 0.85831735, 0.86023872],
        [0.97791561, 0.07154844, 0.38946964]]])
</pre>
- randn 함수

 - 평균 0, 분산 1의 표준 정규분포 난수 matrix array 생성




```python
np.random.randn(3,3)
```

<pre>
array([[ 0.04997603, -0.95190023,  0.44591015],
       [ 0.41734562, -0.91598104,  0.55968764],
       [ 1.06786417, -1.08018472,  0.14578862]])
</pre>
- seed 함수

 - 랜덤한 값을 동일하게 다시 생성하고자 할때 사용



```python
np.random.seed(1234)
np.random.rand(2,3,3)
```

<pre>
array([[[0.19151945, 0.62210877, 0.43772774],
        [0.78535858, 0.77997581, 0.27259261],
        [0.27646426, 0.80187218, 0.95813935]],

       [[0.87593263, 0.35781727, 0.50099513],
        [0.68346294, 0.71270203, 0.37025075],
        [0.56119619, 0.50308317, 0.01376845]]])
</pre>
- choice 함수

 - 주어진 1차원 array부터 랜덤으로 샘플링

 - replace = True 복원추출(기본값)

 - replace = False 비복원추출



```python
x = np.arange(20)
y = np.random.choice(x, size=(2,3,3), replace = False)

print(x)
print(y)
```

<pre>
[ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19]
[[[12 18 13]
  [16  5  8]
  [ 1  4 10]]

 [[19 17 14]
  [15  9  0]
  [11  7  3]]]
</pre>
### 3. 부분 추출하기

 - 3.1 슬라이싱

  - numpy array는 파이썬 리스트와 마찬가지로 슬라이스(slice)를 지원함

  - numpy array를 슬라이싱하기 위해서는 각 차원별로 슬라이스 범위를 지정해야 함



```python
x = np.arange(1,10).reshape(3,3)
x
```

<pre>
array([[1, 2, 3],
       [4, 5, 6],
       [7, 8, 9]])
</pre>

```python
x[0,2]
```

<pre>
3
</pre>

```python
x[2]
```

<pre>
array([7, 8, 9])
</pre>

```python
x[1:]
```

<pre>
array([[4, 5, 6],
       [7, 8, 9]])
</pre>

```python
x[1:2]
```

<pre>
array([[4, 5, 6]])
</pre>

```python
x[:,1:3]
```

<pre>
array([[2, 3],
       [5, 6],
       [8, 9]])
</pre>

```python
x[1:,1:]
```

<pre>
array([[5, 6],
       [8, 9]])
</pre>

```python
x[1:,:2]
```

<pre>
array([[4, 5],
       [7, 8]])
</pre>
### 3.2 인덱싱



### 정수 인덱싱



- numpy 슬라이싱이 각 array 차원별 최소-최대의 범위를 정하여 부분 집합을 구하는 것이라면, 정수 인덱싱은 각 차원별로 선택되어지는 array 요소의 인덱스들을 일렬로 나열하여 부분집합을 구하는 방식임

- 즉, 임의의 numpy array a에 대해 a[[row1, row2], [col1, col2]] 와 같이 표현하는 것인데, 이는 a[row1, col1] 과 a[row2, col2] 라는 두 개의 array 요소의 집합을 의미함

- 예를 들어, 아래 예제에서 a[[0, 2], [1, 3]] 은 정수 인덱싱으로서 이는 a[0, 1] 과 a[2, 3] 등 2개의 array 요소를 가리킴



```python
lst = [[1,2,3,4],
       [5,6,7,8],
       [9,10,11,12]]
a = np.array(lst)
a
```

<pre>
array([[ 1,  2,  3,  4],
       [ 5,  6,  7,  8],
       [ 9, 10, 11, 12]])
</pre>

```python
s = a[[0,2],[1,3]]
s
```

<pre>
array([ 2, 12])
</pre>

```python
f = a[[1,2],[2,1]]
f
```

<pre>
array([ 7, 10])
</pre>
### 다중조건 사용하기

 - 파이썬은 논리 연산자인 and, or, not 사용불가

 - &, | 사용



```python
a = np.array([[1,2,3],
             [4,5,6],
             [7,8,9]])
a
```

<pre>
array([[1, 2, 3],
       [4, 5, 6],
       [7, 8, 9]])
</pre>

```python
a[(a%2 == 0) & (a>5) ]
```

<pre>
array([6, 8])
</pre>

```python
a[(a%2 == 0) | (a>5) ]
```

<pre>
array([2, 4, 6, 7, 8, 9])
</pre>
## 4. 연산

 ### 4.1 기본 연산

- numpy를 사용하면 array간 연산을 쉽게 실행할 수 있음

- 연산은 +, -, *, / 등의 연산자를 사용할 수도 있고, add(), substract(), multiply(), divide() 등의 함수를 사용할 수도 있음 

- 예를 들어, 아래 예제와 같이 array a 와 b 가 있을때, a + b를 하면 각 array 요소의 합을 구하게 됨

- 즉 a[0]+b[0], a[1]+b[1], ... 과 같은 방식으로 결과를 리턴하게 됨



```python
a = np.array([1,2,3])
b = np.array([6,5,9])

print(a)
print(b)
```

<pre>
[1 2 3]
[6 5 9]
</pre>

```python
print(a+b)
print(np.add(a,b))
```

<pre>
[ 7  7 12]
[ 7  7 12]
</pre>

```python
print(a-b)
print(np.subtract(a,b))
```

<pre>
[-5 -3 -6]
[-5 -3 -6]
</pre>

```python
print(a*b)
print(np.multiply(a,b))
```

<pre>
[ 6 10 27]
[ 6 10 27]
</pre>

```python
print(a/b)
print(np.divide(a,b))
```

<pre>
[0.16666667 0.4        0.33333333]
[0.16666667 0.4        0.33333333]
</pre>
### 4.2 행렬 곱 연산

 - numpy에서 matrix의 product를 구하기 위해서 dot() 함수를 사용함

 - 아래 예제는 두 matrix의 product를 구한 예시



```python
lst1 = [[1,2],
       [3,4]]

lst2 = [[5,6],
       [7,8]]

a = np.array(lst1)
b = np.array(lst2)

c = np.dot(a,b)
c
```

<pre>
array([[19, 22],
       [43, 50]])
</pre>
### 4.3 기준이 있는 연산

- numpy는 array간 연산을 위한 함수들을 제공하는데, 각 array 요소들을 더하는 sum() 함수, 각 array 요소들을 곱하는 prod() 함수, 평균을 구하는 mean() 함수 등이 있음

- 이들 함수에 선택옵션으로 axis를 지정할 수 있음

- 예를 들어 sum() 함수에서 axis가 0이면 행끼리 더하는 것(각 열의 합)이고, axis가 1이면 열끼리(각 행의 합) 더하는 것임


- np.sum()



```python
a = np.array([[1,2,3],
             [4,5,6],
             [7,8,9]])

a
```

<pre>
array([[1, 2, 3],
       [4, 5, 6],
       [7, 8, 9]])
</pre>

```python
np.sum(a)
```

<pre>
45
</pre>

```python
np.sum(a, axis =0)
```

<pre>
array([12, 15, 18])
</pre>

```python
np.sum(a, axis =1)
```

<pre>
array([ 6, 15, 24])
</pre>

```python
a.sum(axis=1)
```

<pre>
array([ 6, 15, 24])
</pre>
- np.prod() # 곱

- np.mean() # 평균


### 4.4 array에 nan이 있을 때 연산

 - sum() 함수를 사용할 때, array에 nan이 있는 경우 nan을 제외하고 합을 구하는 함수로 nansum()함수를 사용



```python
a = np.array([[1,2],[4,np.nan]])
a
```

<pre>
array([[ 1.,  2.],
       [ 4., nan]])
</pre>

```python
np.sum(a)
```

<pre>
nan
</pre>

```python
a.prod()
```

<pre>
nan
</pre>

```python
np.nansum(a)
```

<pre>
7.0
</pre>

```python
np.nanprod(a)
```

<pre>
8.0
</pre>

```python
np.nansum(a, axis=0) #열의 합
```

<pre>
array([5., 2.])
</pre>

```python
np.nansum(a, axis = 1) #행의 합
```

<pre>
array([3., 4.])
</pre>
## 5. 기타함수



### 5.1 where() 함수

 - 특정 조건을 만족할 때와 그렇지 않을 때 값을 각각 출력해주는 함수로 where() 함수를 사용

 - where(조건, 만족할때의 출력값, 만족하지 않을 때의 출력값)



```python
a = np.random.randn(4,4)
a
```

<pre>
array([[-0.99245009, -1.33743291,  1.21481458, -0.35214466],
       [ 1.36081077,  0.55253634,  1.22364139, -0.23659903],
       [ 0.95692038, -1.67469107,  0.26323924,  0.15985934],
       [ 1.58592928, -0.28960769, -0.64400765,  1.7661651 ]])
</pre>

```python
np.where(a > 0, "양수", "음수")
```

<pre>
array([['음수', '음수', '양수', '음수'],
       ['양수', '양수', '양수', '음수'],
       ['양수', '음수', '양수', '양수'],
       ['양수', '음수', '음수', '양수']], dtype='<U2')
</pre>
