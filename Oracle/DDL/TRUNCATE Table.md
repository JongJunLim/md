---
title: TRUNCATE TABLE
layout: default
parent: DDL
grand_parent: Oracle
nav_order: 5
---
<div style="text-align: right;">
</div>


<div style="text-align: right;">
작성일자 : 2023-09-03<br>
</div>


## <span style="background-color:#FFF5b1">1.TRUNCATE TABLE</span> <a id="chapter-1"></a>
{: .d-inline-block }

Ver 0.1.1
{: .label .label-green }

- [1. 테이블 초기화](#chapter-1)<br>

---
### **1. 테이블 초기화(TRUNCATE)** <a id="chapter-1"></a>
- 테이블 초기화
  - 테이블 구조는 유지한 채 <u><b>데이터를 전체 삭제</b></u>할 때, <u><b>TRUNCATE TABLE 문</b></u>을 사용한다.
  - 내부 처리 방식이나 <u><b>Auto Commit</b></u>의 특성으로 인해 DDL로 분류된다.
  - 위험성이 있으나, DELETE 문보다 속도가 빨라 효율성 측면에서 좋다.
<br>
  - Syntax

```sql
TRUNCATE TABLE [schema.]table;
```

- 구문 설명
  - <i>table</i> : 초기화 하려는 테이블의 이름

<br>


EX)

```sql
TRUNCATE TABLE table1;

DESC t2;
```


|COLUMN|Nullable|Type|
|:-----|:------:|:---|
|EMPNO||NUMBER(4)|
|ENAME|NOT NULL|VARCHAR2(10)|
|SAL||NUMBER(7,2)|
|DEPTNO|NOT NULL|NUMBER(2)|



```sql
SELECT a.*
  FROM table1 a;
```




|EMPNO|ENAME|SAL|DEPTNO|
|----:|:----|--:|-----:|




no rows selected
