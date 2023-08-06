---
title: TCL 
layout: default
parent: DML
grand_parent: Oracle
nav_order: 1
---
<div style="text-align: right;">
작성일자 : 2023-08-06<br>
</div>

## TCL(Transaction Control Language)
{: .d-inline-block }

Ver 0.1.1
{: .label .label-green }

- [1. TCL](#chapter-1)<br>
- [2. TRANSACTION](#chapter-2)<br>
- [3. LOCK](#chapter-3)<br>
- [4. COMMIT](#chapter-4)<br>
- [5. ROLLBACK](#chapter-5)<br>
- [6. SAVEPOINT](#chapter-6)<br>
- [7. 명시적 트랜잭션 VS 암시적 트랜잭션](#chapter-7)<br>

---

### 1.TCL(Transaction Control Language) <a id="chapter-1"></a>

- TCL 문을 사용하면 아래와 같이 **트랜잭션을 제어**할 수 있다.
  - 현재 트랜잭션의 <u><b>변경 내용을 데이터베이스에 영구적으로 저장(COMMIT)</b></u>하고 트랜잭션을 종료한다.
  - 현재 트랜잭션의 <u><b>변경 내용을 모두 취소(ROLLBACK)</b></u>하고 트랜잭션을 종료한다.
  - ROLLBACK이 가능한 <u><b>저장점(SAVEPOINT)을 생성</b></u>한다.

```sql
-------------------------------------------- 트랜잭션 1 시작
INSERT INTO t1 ... ;
UPDATE t1 ... ;
COMMIT ;
-------------------------------------------- 트랜잭션 1 종료(변경 내용 저장)
-------------------------------------------- 트랜잭션 2 시작
UPDATE t1 ...;
DELETE t1 ...;
INSERT INTO t1 ... ;
ROLLBACK;
-------------------------------------------- 트랜잭션 2 종료(변경 내용 취소)
```

---

### 2.TRANSACTION <a id="chapter-2"></a>
- <u><b>함께 수행되어야 하는 작업의 논리적인 단위</b></u>이다. 즉, 의미적으로 분리될 수 없는 1개 이상의 데이터 조작(DML)을 말한다.<br> EX) 계좌 이체

```sql
UPDATE Account SET Amount = Amount - 20000
 WHERE Account_Num = '100912345678';

UPDATE Account SET Amount = Amount  + 20000
 WHERE Account_Num = '910078781234';

COMMIT
```
- 특징 (ACID)


|특징|설명|
|:---:|:----|
|원자성(Atomicity)|트랜잭션의 작업은 모두 수행되거나 모두 수행되지 않아야한다.**(All or Nothing)**|
|일관성(Consistency)|트랜잭션이 완료되면 데이터의 무결성이 일관되게 보장되어야 한다.|
|격리성(Isolation)|트랜잭션이 수행되는 세션은 다른 세션으로부터 격리되어야 한다.|
|지속성(Durability)|트랜잭션이 완료되면 변경 내용이 영구적으로 지속되어야 한다.|


---

### 3.LOCK <a id="chapter-3"></a>
- LOCK은 다중 트랜잭션 환경에서 <u>트랜잭션의 순차적 진행</u>을 보장하기 위한 매커니즘이다.
- 서로 다른 행을 조작하는 DML문 간에는 블로킹(Blocking)이 발생하지 않는다.


```sql
UPDATE t1 a SET a.SAL = a.SAL*1.2
 WHERE a.EMPNO = 7876;

COMMIT;
```

```sql
UPDATE t1 a SET a.SAL = a.SAL - 500
 WHERE a.EMPNO = 7876;  

ROLLBACK
```

```sql
UPDATE t1 a SET a.SAL = a.SAL + 700
 WHERE a.EMPNO = 7876;  
```

---


### 4.COMMIT <a id="chapter-4"></a>

- 현재 트랜잭션의 <u><b>변경 내용을 데이터베이스에 영구적으로 저장</b></u>하고 트랜잭션을 종료한다.
- 세션 1(or 사용자 1)
  - 시간 1

```sql
INSERT INTO t1 (empno, ename, sal, deptno)
     VALUES (7369, 'SMITH', 800, 20);

SELECT a.*
  FROM t1 a;
```


|EMPNO|ENAME|SAL|DEPTNO|
|--:|:--|--:|--:|
|7369|SMITH|800|20|


  - 시간 2

```sql
INSERT INTO t1 (empno, ename, sal, deptno)
     VALUES (7876, 'ADAMS', 1100, 20);

SELECT a.*
  FROM t1 a;
```


|EMPNO|ENAME|SAL|DEPTNO|
|--:|:--|--:|--:|
|7369|SMITH|800|20|
|7867|ADAMS|1100|20|


```sql
COMMIT;
```

  - 시간 3

```sql
SELECT a.*
  FROM t1 a;
```


|EMPNO|ENAME|SAL|DEPTNO|
|--:|:--|--:|--:|
|7369|SMITH|800|20|
|7867|ADAMS|1100|20|


- 세션 2(or 사용자 2)
  - 시간 1

```sql
SELECT a.*
  FROM t1 a;
```

no rows selected


  - 시간 2

```sql
SELECT a.*
  FROM t1 a;
```

no rows selected

  - 시간 3 (세션 1(사용자 1) commit 이후)

```sql
SELECT a.*
  FROM t1 a;
```


|EMPNO|ENAME|SAL|DEPTNO|
|--:|:--|--:|--:|
|7369|SMITH|800|20|
|7867|ADAMS|1100|20|


---
### 5.ROLLBACK <a id="chapter-5"></a>
- 현재 트랜잭션의 <u><b>변경 내용을 모두 취소</b></u>하고 트랜잭션을 종료한다.
- 세션 1(or 사용자 1)
  - 시간 1

```sql
INSERT INTO t1 (empno, ename, sal, deptno)
     VALUES (7900, 'JAMES', 950, 30);

INSERT INTO t1 (empno, ename, sal, deptno)
     VALUES (7934, 'MILLER', 1300, 10);

UPDATE t1 a SET a.SAL = a.SAL - 1000
 WHERE a.SAL > 1000;

SELECT a.*
  FROM t1 a;
```


|EMPNO|ENAME|SAL|DEPTNO|
|--:|:--|--:|--:|
|7867|ADAMS|1100|20|
|7900|JAMES|950|30|
|7934|MILLER|300|10|



```sql
ROLLBACK;
```
  - 시간 2

```sql
SELECT a.*
  FROM t1 a;
```


|EMPNO|ENAME|SAL|DEPTNO|
|--:|:--|--:|--:|
|7867|ADAMS|1100|20|


- 세션 2(or 사용자 2)
  - 시간 1

```sql
SELECT a.*
  FROM t1 a;
```


|EMPNO|ENAME|SAL|DEPTNO|
|--:|:--|--:|--:|
|7867|ADAMS|1100|20|


  - 시간 2

```sql
SELECT a.*
  FROM t1 a;
```


|EMPNO|ENAME|SAL|DEPTNO|
|--:|:--|--:|--:|
|7867|ADAMS|1100|20|


---
### 6.SAVEPOINT <a id="chapter-6"></a>
- <u><b>ROLLBACK이 가능한 저장점을 생성</b></u>한다.
- 세션 1(or 사용자 1)

```sql
--- 트랜잭션 시작
INSERT INTO t1 (empno, ename, sal, deptno)
     VALUES (7839, 'KING', 5000, 10);
```

|EMPNO|ENAME|SAL|DEPTNO|
|--:|:--|--:|--:|
|7839|KING|5000|10|
|7876|ADAMS|1100|20|

```sql
SAVEPOINT s1; 
```

```sql
UPDATE t1 a SET a.SAL = 3000
 WHERE a.SAL > 3000;
```

|EMPNO|ENAME|SAL|DEPTNO|
|--:|:--|--:|--:|
|7839|KING|3000|10|
|7876|ADAMS|1100|20|



```sql
INSERT INTO t1 (empno, ename, sal, deptno)
     VALUES (7499, 'ALLEN', 1600, 30);
```


|EMPNO|ENAME|SAL|DEPTNO|
|--:|:--|--:|--:|
|7839|KING|3000|10|
|7876|ADAMS|1100|20|
|7499|ALLEN|1600|30|


```sql
SAVEPOINT s2;
```

```sql
ROLLBACK TO s1;
```


|EMPNO|ENAME|SAL|DEPTNO|
|--:|:--|--:|--:|
|7839|KING|3000|10|
|7876|ADAMS|1100|20|


```sql
UPDATE t1 a SET a.SAL = 1500
 WHERE a.SAL > 1500;
```


|EMPNO|ENAME|SAL|DEPTNO|
|--:|:--|--:|--:|
|7839|KING|1500|10|
|7876|ADAMS|1100|20|


```sql
COMMIT;
--- 트랜잭션 종료
```

---
### 7.명시적 트랜잭션 VS 암시적 트랜잭션 <a id="chapter-7"></a>
- 트랜잭션의 시작을 명시하는 **명시적 트랜잭션**과 COMMIT/ROLLBACK 이후 처음 수행되는 DML문에 의해 트랜잭션이 시작되는 **암시적 트랜잭션**이 있다.
  
- 암시적 트랜잭션(ORACLE)
  
```sql
--- 트랜잭션1 시작
INSERT INTO t1 ... ;

UPDATE t1 ...;

COMMIT;
--- 트랜잭션1 종료
--- 트랜잭션2 시작
UPDATE t1 ...;

DELETE t1 ...;

INSERT INTO t1 ...;

ROLLBACK; 
--- 트랜잭션2 종료
```

- 명시적 트랜잭션
  
```sql
BEGIN TRANSACTION TR1; -- 트랜잭션1 시작
INSERT INTO t1 ... ;

UPDATE t1 ...;

COMMIT [TRANSACTION TR1]; -- 트랜잭션1 종료

BEGIN TRANSACTION TR2; -- 트랜잭션2 시작

UPDATE t1 ...;

DELETE t1 ...;

INSERT INTO t1 ...;

ROLLBACK [TRANSACTION TR2]; -- 트랜잭션2 종료
```
