---
title: DROP TABLE
layout: default
parent: DDL
grand_parent: Oracle
nav_order: 4
---
<div style="text-align: right;">
</div>


<div style="text-align: right;">
작성일자 : 2023-09-03<br>
</div>


## <span style="background-color:#FFF5b1">1.DROP TABLE</span> <a id="chapter-1"></a>
{: .d-inline-block }

Ver 0.1.1
{: .label .label-green }

- [1. 테이블 삭제 ](#chapter-1)<br>

---
### **1. 테이블 삭제** <a id="chapter-1"></a>
- 기존 테이블을 삭제할 때는 <u><b>DROP TABLE 문</b></u>을 사용한다.
- 테이블의 구조와 데이터가 함께 삭제된다.

  - Syntax

```sql
DROP TABLE [schema.]table [CASCADE CONSTRAINTS] [PURGE];
```

- 구문 설명
  - <i>table</i> : 삭제하려는 테이블의 이름
  - <i>CASCADE CONSTRAINTS</i> : 테이블을 참조하고 있는 FK 제약 조건을 삭제
  - <i>PURGE</i> : recyle bin을 사용하지 않고 테이블을 즉시 삭제 (= 윈도우의 ctrl + delete)

<br>


EX)

```sql
DROP TABLE t2 PURGE;

SELECT a.*
  FROM t2 a;
```

ORA-00942 : 테이블 또는 뷰가 존재하지 않습니다.

```sql
DESC t2;
```

ORA-00943 : t2 객체는 존재하지 않습니다.

