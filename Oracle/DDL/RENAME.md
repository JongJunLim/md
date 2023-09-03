---
title: RENAME TABLE
layout: default
parent: DDL
grand_parent: Oracle
nav_order: 3
---
<div style="text-align: right;">
</div>


<div style="text-align: right;">
작성일자 : 2023-09-03<br>
</div>


## <span style="background-color:#FFF5b1">1.RENAME TABLE</span> <a id="chapter-1"></a>
{: .d-inline-block }

Ver 0.1.1
{: .label .label-green }

- [1. 테이블명 변경](#chapter-1)<br>

---
### **1. 테이블명 변경** <a id="chapter-1"></a>
기존 테이블의 이름을 변경할 때는 <u><b>RENAME 문</b></u>을 사용한다.

  - Syntax

```sql
RENAME old_table TO new_table;
```

- 구문 설명
  - <i>old_table</i> : 기존 테이블의 변경 전 이름
  - <i>new_table</i> : 테이블의 변경 후 이름

<br>


EX)

```sql
RENAME t1 TO table1;
```


```sql
DESC table1
```


|COLUMN|Nullable|Type|
|:-----|:------:|:---|
|EMPNO||NUMBER(4)|
|ENAME|NOT NULL|VARCHAR2(10)|
|SAL||NUMBER(7,2)|
|DEPTNO|NOT NULL|NUMBER(2)|



|EMPNO|ENAME|SAL|DEPTNO|
|----:|:----|--:|-----:|
|7782|CLARK||10|
|7369|SMITH||20|
|7566|JONES||20|
|7499|ALLEN||50|
|7902|JONES||90|


