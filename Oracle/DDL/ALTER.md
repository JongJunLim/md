---
title: ALTER Table
layout: default
parent: DDL
grand_parent: Oracle
nav_order: 2
---
<div style="text-align: right;">
</div>


<div style="text-align: right;">
작성일자 : 2023-08-18<br>
</div>


## <span style="background-color:#FFF5b1">1. Create Table</span> <a id="chapter-1"></a>
{: .d-inline-block }

Ver 0.1.1
{: .label .label-green }


- [1.칼럼 추가](#chapter-1)<br>
- [2.칼럼 수정](#chapter-2)<br>
- [3.칼럼명 변경](#chapter-3)<br>
- [4.제약조건(Constraints) 추가](#chapter-4)<br>
- [5.제약조건(Constraints) 삭제](#chapter-5)<br>


---

### **1. 칼럼 추가** <a id="chapter-1"></a>
- 테이블에 칼럼을 추가할 때는 ALTER TABLE 명령문의 <u><b>ADD 절</b></u>을 사용한다.
- 추가한 칼럼들은 기존 컬럼들 맨 뒤에 추가된다.

  - Syntax

```sql
ALTER TABLE [schema.]table
        ADD(column datatype [DEFAULT expr][NOT NULL]
          [,column datatype [DEFAULT expr][NOT NULL]]...
           );
```

- 구문 설명
  - column : 테이블에 추가할 칼럼의 이름
  - datatype : 해당 칼럼에 저장될 데이터의 타입(ex. varchar2, number, date, ..)
  - DEFAULT : 해당 칼럼에 값이 입력되지 않을 경우, 저장될 기본 값(expr)
  - NOT NULL : 해당 칼럼에 NOT NULL 제약 조건을 지정


EX)

```sql
ALTER TABLE t1
        ADD( HIREDATE DATE NOT NULL
            ,COMM     NUMBER DEFAULT 500
            );
```
     

```sql
DESC t1
```


|COLUMN|Nullable|Type|
|:-----|:------:|:---|
|EMPNO||NUMBER(4)|
|ENAME|NOT NULL|VARCHAR2(10)|
|SAL||NUMBER(7,2)|
|DEPTNO|NOT NULL|NUMBER(2)|
|**HIREDATE**|**NOT NULL**|**DATE**|
|**COMM**||**NUMBER**|



---

### **2. 칼럼 수정** <a id="chapter-2"></a>
- 기존 칼럼의 구조를 수정할 때는 ALTER TABLE 명령문의 <u><b>MODIFY 절</b></u>을 사용한다.
- 기존 칼럼에 값이 존재하면, 데이터 타입의 크기를 줄일 수 없다. (크기를 늘리는 것은 가능하다.)

  - Syntax

```sql
ALTER TABLE [schema.]table
     MODIFY(column datatype [DEFAULT expr][NOT NULL]
          [,column datatype [DEFAULT expr][NOT NULL]]...
           );
```

- 구문 설명
  - column : 수정하려는 칼럼의 이름
  - datatype : 해당 칼럼에 저장될 데이터의 타입(ex. varchar2, number, date, ..)
  - DEFAULT : 해당 칼럼에 값이 입력되지 않을 경우, 저장될 기본 값(expr)
  - NOT NULL : 해당 칼럼에 NOT NULL 제약 조건을 지정


EX)

```sql
ALTER TABLE t1
     MODIFY( ENAME     VARCHAR2(10)
            ,HIREDATE  VARCHAR2(8) NULL
            );
```
     

```sql
DESC t1
```


수정 후

|COLUMN|Nullable|Type|
|:-----|:------:|:---|
|EMPNO||NUMBER(4)|
|**ENAME**|**NOT NULL**|**VARCHAR2(10)**|
|SAL||NUMBER(7,2)|
|DEPTNO|NOT NULL|NUMBER(2)|
|**HIREDATE**||**VARCHAR2(8)**|
|COMM||NUMBER|



수정 전


|COLUMN|Nullable|Type|
|:-----|:------:|:---|
|EMPNO||NUMBER(4)|
|**ENAME**|**NOT NULL**|**VARCHAR2(20)**|
|SAL||NUMBER(7,2)|
|DEPTNO|NOT NULL|NUMBER(2)|
|**HIREDATE**|**NOT NULL**|**DATE**|
|COMM||NUMBER|



---

### **3. 칼럼명 변경** <a id="chapter-3"></a>
- 기존 칼럼의 이름을 변경할 때는 ALTER TABLE 명령문의 <u><b>RENAME COLUMN 절</b></u>을 사용한다.

  - Syntax

```sql
ALTER TABLE [schema.]table
RENAME COLUMN old_column TO new_column;
```

- 구문 설명
  - old_column : 기존 칼럼의 변경 전 이름
  - new_column : 칼럼의 변경 후 이름


EX)

```sql
ALTER TABLE t1
RENAME COLUMN COMM TO COMMISION;
```
     

```sql
DESC t1
```


|COLUMN|Nullable|Type|
|:-----|:------:|:---|
|EMPNO||NUMBER(4)|
|ENAME|NOT NULL|VARCHAR2(10)|
|SAL||NUMBER(7,2)|
|DEPTNO|NOT NULL|NUMBER(2)|
|HIREDATE||VARCHAR2(8)|
|**COMMISION**||NUMBER|



---

### **4. 제약조건(Constraints) 추가** <a id="chapter-4"></a>
- 특정 칼럼에 제약 조건을 추가할 때는 ALTER TABLE 명령문의 <u><b>ADD CONSTRAINT 절</b></u>을 사용한다.

  - Syntax

```sql
ALTER TABLE [schema.]table
ADD CONSTRAINT constraint_name constraint_type (column [, column, ...]);
```

- 구문 설명
  - constraint_name : 추가하려는 제약 조건의 이름
  - constraint_type : 추가하려는 제약 조건의 유형
  - column : 해당 제약 조건이 적용될 칼럼(들)

<br>

- 제약 조건(Constraints) : 오라클 데이터베이스는 데이터 무결성(Data Integrity)을 보장하기 위해 아래와 같은 제약 조건을 지정할 수 있다.

|제약조건|설명|
|:------:|:--|
|UNIQUE 제약|- UNIQUE 제약 조건이 걸린 칼럼(들)에 대해서 <u><b>동일한 값이 입력되지 못하게 제약한다.</u></b>|
|NOT NULL 제약|- NOT NULL 제약 조건이 걸린 칼럼에 대해서 <u><b>NULL 값이 입력되지 못하게 제약한다.</u></b>|
|PK(Primary Key) 제약|- PK 제약 조건 = <u><b>UNIQUE 제약 조건 + NOT NULL 제약 조건</u></b><br>- 식별자(개별 행을 고유하게 식별할 수 있는 구분자) 역할을 하는 칼럼에 대해 지정한다.|
|FK(Foreign Key) 제약|- FK 제약조건이 걸린 두 테이블의 칼럼(들)에 대해서 아래와 같은 제약을 한다.<br>- <u><b>부모 테이블에 존재하지 않는 칼럼 값은 자식 테이블에 입력할 수 없다.</u></b><br>-<u><b>자식 테이블에 존재하는 칼럼 값은 부모 테이블에서 삭제할 수 없다.</u></b>|
|CHECK 제약|- <b>CHECK 제약 조건이 걸린 칼럼에 대해</b><u><b>조건을 만족하지 않는 값이 입력되지 못하게 한다.<br>-TRUE or FALSE로 평가될 수 있는 조건을 지정한다.|




EX) PK 추가 예제

```sql
ALTER TABLE t2
ADD CONSTRAINT t2_pk PRIMARY KEY (DEPTNO);
```
     
```sql
INSERT INTO t2 (DEPTNO, DNAME, LOC)
     VALUES(10, 'ACCOUNTING', 'NEWYORK');

INSERT INTO t2 (DEPTNO, DNAME, LOC)
     VALUES(20, 'RESEARCH', 'DALLAS');

INSERT INTO t2 (DEPTNO, DNAME, LOC)
     VALUES(20, 'OPERATIONS', 'BOSTON');

ORA-00001 : 무결성 제약 조건(T2_PK)에 위배됩니다.

INSERT INTO t2 (DEPTNO, DNAME, LOC)
     VALUES(30, 'RESEARCH', 'DALLAS');

INSERT INTO t2 (DEPTNO, DNAME, LOC)
     VALUES(NULL, 'SALES', 'CHICAGO');

ORA-01400 : NULL을 ("T2","DEPTNO")안에 삽입할 수 없습니다.

COMMIT;
```



|DEPTNO|DNAME|LOC|
|-----:|:------|:---|
|10|ACCOUNTING|NEW YORK|
|20|RESEARCH|DALLAS|
|30|RESEARCH|DALLAS|



EX) FK 추가 예제

```sql
ALTER TABLE t1
ADD CONSTRAINT t1_fk FOREIGN KEY (DEPTNO) REFERENCES t2 (DEPTNO);
```
     
```sql
INSERT INTO t1 (EMPNO,ENAME,HIREDATE)
     VALUES(7782, 'CLARK', 10, '19810609');

INSERT INTO t1 (EMPNO,ENAME,HIREDATE)
     VALUES(7369, 'SMITH', 20, '19801217');

INSERT INTO t1 (EMPNO,ENAME,HIREDATE)
     VALUES(7566, 'JONES', 20, '19810402');

INSERT INTO t1 (EMPNO,ENAME,HIREDATE)
     VALUES(7499, 'ALLEN', 50, '19810220');

ORA-02291 : 무결성 제약 조건(T1_FK)이 위배되었습니다 - 부모 키가 없습니다.

DELETE t2 a 
 WHERE a.DEPTNO = 30;

DELETE t2 a 
 WHERE a.DEPTNO = 20;

ORA-02292 : 무결성 제약 조건(T1_FK)이 위배되었습니다 - 자식 레코드가 발견되었습니다.

COMMIT;
```


[T1 조회 결과]



|EMPNO|ENAME|SAL|DEPTNO|HIREDATE|COMMISION|
|----:|:----|--:|-----:|:-------|--------:|
|7782|CLARK||10|19810609|500|
|7369|SMITH||20|19801217|500|
|7566|JONES||20|19810402|500|



[T2 조회 결과]



|DEPTNO|HIREDATE|COMMISION|
|-----:|:-------|--------:|
|10|ACCOUNTING|NEW YORK|
|20|RESEARCH|DALLAS|
|~~20~~|~~RESEARCH~~|~~DALLAS~~|



---

### **5. 제약조건(Constraints) 삭제** <a id="chapter-5"></a>
칼럼에 걸려 있는 제약 조건을 삭제할 때는 ALTER TABLE 명령문의 <u><b>DROP CONTRAINT 절</b></u>을 사용한다.

  - Syntax

```sql
ALTER TABLE [schema.]table
DROP CONSTRAINT constraint_name [CASCADE];
```

- 구문 설명
  - constraint_name : 삭제하려는 제약 조건의 이름
  - CASCADE : 참조하고 있는 FK 제약 조건을 함께 삭제

<br>


EX) PK 삭제 예제

```sql
ALTER TABLE t2
DROP CONSTRAINT t1_pk;

ORA-02273 : 고유/기본 키가 외부 키에 의해 참조되었습니다

ALTER TABLE t2
DROP CONSTRAINT t1_pk CASCADE; -- t2_pk, t1_fk 제약 조건 삭제
```
     

```sql
DELETE t2 a 
 WHERE a.DEPTNO = 10;

INSERT INTO t2 (DEPTNO, DNAME, LOC)
     VALUES(20, 'RESEARCH', 'DALLAS');

INSERT INTO t2 (DEPTNO, DNAME, LOC)
     VALUES(20, 'RESEARCH', 'DALLAS');

INSERT INTO t1 (EMPNO,ENAME,HIREDATE)
     VALUES(7499, 'ALLEN', 50, '19810220');

INSERT INTO t1 (EMPNO,ENAME,HIREDATE)
     VALUES(7902, 'FORD', 90, '19811203');

COMMIT;

```


[T2 조회 결과]



|DEPTNO|HIREDATE|COMMISION|
|-----:|:-------|--------:|
|20|RESEARCH|DALLAS|
|**20**|RESEARCH|DALLAS|
|**20**|RESEARCH|DALLAS|



[T1 조회 결과]



|EMPNO|ENAME|SAL|DEPTNO|HIREDATE|COMMISION|
|----:|:----|--:|-----:|:-------|--------:|
|7782|CLARK||**10**|19810609|500|
|7369|SMITH||20|19801217|500|
|7566|JONES||20|19810402|500|
|7499|ALLEN||**50**|19810220|500|
|7902|FORD||**90**|19811203|500|


