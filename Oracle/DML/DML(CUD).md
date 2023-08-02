---
title: DML 
layout: default
parent: Oracle
has_children: false
nav_order: 2.5
---
<div style="text-align: right;">
작성일자 : 2023-07-24<br>
수정일자 : 2023-08-02
</div>

## DML(Data Manipulation Language)
{: .d-inline-block }

Ver 0.1.2
{: .label .label-green }

- [1. INSERT](#chapter-1)<br>
- [2. UPDATE](#chapter-2)<br>
- [3. DELETE](#chapter-3)<br>
- [4. MERGE](#chapter-4)<br>

---

- 데이터베이스의 내부 데이터를 관리하기 위한 언어이다. 데이터를 CRUD( Create, Read, Update, Delete) 작업을 수행하기 위해 사용된다.

- SELECT문을 제외한 DML문은 <b><u>데이터의 변경 및 LOCK</u></b>을 발생 시키기 때문에 사용에 주의를 기울여야 한다.<br>(다른 사용자에게 영향을 미칠 수 있다.)



|종류|설명|
|:--:|:--|
|INSERT 문|테이블에 신규 행을 삽입하는 SQL 문|
|UPDATE 문|테이블에 기존 행을 갱신하는 SQL 문|
|DELETE 문|테이블에 기존 행을 삭제하는 SQL 문|
|MERGE 문|두 집합(소스, 타겟) 간에 데이터를 병합하는 SQL 문|


---

### 1. INSERT <a id="chapter-1"></a>
- 아래 INSERT 문을 사용하면 한 번에 1건만 테이블에 삽입된다.

```sql
INSERT INTO TABLE(COL1, COL2, ...)
    VALUES(VALUE1, VALUE2, ...)
```

- 구문 설명
  - TABLE : 테이블 이름
  - COLUMN : 데이터를 삽입할 칼럼의 이름
  - VALUE : 해당 칼럼에 입력하고자 하는 값
<br><br>
- 테이블의 전체 칼럼에 대해 값을 모두 입력할 경우, 문법적으로 칼럼 목록을 생략할 수 있으나, 테이블 구조 변경 등에 의한 잠재적인 문제를 내포하지 않게 되므로 <b><u>항상 칼럼 목록을 명시하는 것이 좋다.</u></b>

- 예시

```sql
DROP TABLE t1 PURGE;

CREATE TABLE t1
(
 EMPNO   NUMBER(4)
,ENAME   VARCHAR(10)
,SAL     NUMBER
,DEPTNO  NUMBER DEFAULT 20
);
```

```sql
-- 추천 방법 : 칼럼명 명시
INSERT INTO t1 (EMPNO, ENAME, SAL, DEPTNO)
    VALUES(7369, 'SMITH', 800, 20);
```
```sql
-- 추천하지 않는 방법 : 칼럼명 미명시
INSERT INTO t1 
    VALUES(7499, 'ALLEN', 1600, 30);
```
```sql
INSERT INTO t1 (EMPNO, SAL)
    VALUES(7521, 1250);
```


|EMPNO|ENAME|SAL|DEPTNO|
|----:|:----|--:|-----:|
|7369|SMITH|800|20|
|7499|ALLEN|1600|30|
|7521|     |1250|20|



```sql
ROLLBACK; -- 취소
```
<br>
- INSERT SELECT문(서브쿼리를 포함하 INSERT문)을 사용하면, 다중 행 결과 집합을 추출하여 한번에 삽입할 수 있다.

```sql
INSERT INTO TABLE(COL1, COL2, ...)
    (subquery);
```

- 구문 설명
  - TABLE : 테이블 이름
  - COLUMN : 데이터를 삽입할 칼럼의 이름
  - SUBQUERY : 테이브레 삽입할 행을 리턴하는 서브 쿼리
<br><br>
- VALUES 절은 사용하지 않으며,<b><u>칼럼 목록의 칼럼 수</u></b>와 <b><u>서브 쿼리의 SELECT 절에 있는 칼럼 수</u></b>가 일치하여야 한다.

- 예시
```sql
INSERT INTO t1 (EMPNO, ENAME, SAL, DEPTNO)
SELECT A.EMPNO
      ,A.ENAME
      ,A.SAL
      ,A.DEPTNO
    FROM EMP A
   WHERE A.DEPTNO = 10;
```


|EMPNO|ENAME|SAL|DEPTNO|
|----:|:----|--:|-----:|
|7934|MILLER|1300|10|
|7782|CLARK|2450|10|
|7839|KING|5000|10|

```sql
ROLLBACK; -- 취소
```

```sql
INSERT INTO t1 (EMPNO, ENAME)
SELECT A.EMPNO
      ,A.ENAME
    FROM EMP A
   WHERE A.SAL < 1000;
```


|EMPNO|ENAME|SAL|DEPTNO|
|----:|:----|--:|-----:|
|7369|SMITH||20|
|7900|JAMES||20|


```sql
ROLLBACK; -- 취소
```

---


### 2. UPDATE <a id="chapter-2"></a>
- UPDATE 문을 사용하여 테이블의 기존 행들을 수정할 수 있다.

```sql
UPDATE TABLE
   SET COL1 = VAL1
      ,COL2 = VAL2
      ,...
 WHERE CONTIDION;
```

- 구문 설명
  - TABLE : 테이블 이름
  - COLUMN : 데이터를 갱신할 칼럼의 이름
  - VALUE : 해당 칼럼에 대해 갱신하려는 값
  - CONDITION : 갱신될 행을 식별할 조건
<br><br>
- 예시
```sql
INSERT INTO t1 (EMPNO, ENAME, SAL, DEPTNO)
SELECT A.EMPNO
      ,A.ENAME
      ,A.SAL
      ,A.DEPTNO
    FROM EMP A
   WHERE A.JOB = 'CLERK';
```


|EMPNO|ENAME|SAL|DEPTNO|
|----:|:----|--:|-----:|
|7369|**SMITH**|**800**|20|
|7876|ADAMS|1100|20|
|7900|**JAMES**|**1300**|30|
|7934|MILLER|1300|10|


```sql
UPDATE t1 
   SET A.ENAME = LOWER(A.ENAME)
      ,A.SAL   = A.SAL + 2000
 WHERE A.SAL   < 1000;
```


|EMPNO|ENAME|SAL|DEPTNO|
|----:|:----|--:|-----:|
|7369|**smith**|**800**|20|
|7876|ADAMS|1100|20|
|7900|**james**|**2950**|30|
|7934|MILLER|1300|10|


```sql
ROLLBACK; -- 취소
```
<br>
- 상호 관련 UPDATE
  - 상관 서브 쿼리를 사용하면 다른 테이블 값의 값을 참조하여 데이터를 갱신할 수 있다.

```sql
UPDATE TABLE
   SET COL1 = (SUBQUERY)
      ,COL2 = (SUBQUERY)
      ,...
 WHERE CONTIDION;
```

- 구문 설명
  - TABLE : 테이블 이름
  - COLUMN : 데이터를 갱신할 칼럼의 이름
  - SUBQUERY : 갱실할 값을 리턴하는 서브쿼리
  - CONDITION : 갱신될 행을 식별할 조건
<br><br>
- 예시
```sql
INSERT INTO t1 (EMPNO, ENAME, SAL, DEPTNO)
SELECT A.EMPNO
      ,A.ENAME
      ,A.SAL
      ,A.DEPTNO
    FROM EMP A
   WHERE A.JOB = 'CLERK';
```


|EMPNO|ENAME|SAL|DEPTNO|
|----:|:----|--:|-----:|
|7369|SMITH|**800**|**20**|
|7876|ADAMS|**1100**|**20**|
|7900|JAMES|**1300**|**30**|
|7934|MILLER|**1300**|**10**|


```sql
UPDATE t1 
   SET A.SAL   = (SELECT ROUND(AVG(X.SAL))
                    FROM EMP X
                   WHERE X.DEPTNO = A.DEPTNO
                     AND X.DEPTNO <> 10)
      ,A.DETNO = 30;
```

{: .important }
> UPDATE 문에 WHERE절을 기술하지 않으면 테이블의 전체 행이 수정됩니다.
> 
> 실무에서는 필요한 행만 선택해서 수정하는 경우가 훨씬 더 많기 때문에, "WHERE 절을 기술하는 것이 일반적이다."라고 알아두기

|EMPNO|ENAME|SAL|DEPTNO|
|----:|:----|--:|-----:|
|7369|SMITH|**2175**|**30**|
|7876|ADAMS|**2175**|**30**|
|7900|JAMES|**1567**|**30**|
|7934|MILLER||**30**|


```sql
ROLLBACK; -- 취소
```
<br>
- UPDATE 문 사전 검증 
  - 사전에 SELECT 문을 작성하여 갱신할 행과 값을 미리 확인하면 실수를 방지할 수 있다.
- 사전 검증 SELECT 문
```sql
SELECT A.*
      ,A.SAL AS SAL_BF
      ,(SELECT ROUND(AVG(X.SAL))
          FROM EMP X
         WHERE X.DEPTNO = A.DEPTNO
           AND X.DEPTNO <> 10) AS SAL_AF
  FROM t1 A
 WHERE A.DEPTNO <= 20;
```

- UPDATE 문
```sql
UPDATE t1 A
   SET A.SAL =(SELECT ROUND(AVG(X.SAL))
                 FROM EMP X
                WHERE X.DEPTNO = A.DEPTNO
                  AND X.DEPTNO <> 10)
 WHERE A.DEPTNO <= 20;
```

```sql
ROLLBACK; -- 취소
```

---

### 3. DELETE <a id="chapter-3"></a>

- DELETE 문을 사용하여 테이블의 기존 행들을 삭제할 수 있다.

```sql
DELETE [FROM] TABLE
 WHERE CONTIDION;
```

- 구문 설명
  - TABLE : 테이블 이름
  - CONDITION : 삭제될 행을 식별할 조건

{: .warning }
> **테이블 데이터 전체 삭제**
> 
> TRUNCATE TABLE table;
>
> ※ TRUNCATE TABLE 문은 Auto Commit을 발생시키므로 사용에 유의

<br>
- 예시
```sql
INSERT INTO t1 (EMPNO, ENAME, SAL, DEPTNO)
SELECT A.EMPNO
      ,A.ENAME
      ,A.SAL
      ,A.DEPTNO
    FROM EMP A
   WHERE A.JOB = 'CLERK';
```

|EMPNO|ENAME|SAL|DEPTNO|
|----:|:----|--:|-----:|
|7369|SMITH|800|20|
|**7876**|ADAMS|1100|20|
|**7900**|JAMES|1300|30|
|**7934**|MILLER|1300|10|


```sql
DELETE t1 A
 WHERE A.EMPNO <> 7369;
```


|EMPNO|ENAME|SAL|DEPTNO|
|----:|:----|--:|-----:|
|7369|SMITH|800|20|



```sql
ROLLBACK; -- 취소
```
<br>

- 상호 관련 DELETE
  - WHERE 절에 상관 서브 쿼리를 사용하여 삭제될 행을 지정할 수 있다.

- 예시
```sql
INSERT INTO t1 (EMPNO, ENAME, SAL, DEPTNO)
SELECT A.EMPNO
      ,A.ENAME
      ,A.SAL
      ,A.DEPTNO
    FROM EMP A
   WHERE A.JOB = 'CLERK';
```


|EMPNO|ENAME|SAL|DEPTNO|
|----:|:----|--:|-----:|
|**7369**|**SMITH**|**800**|**20**|
|**7876**|**ADAMS**|**1100**|**20**|
|7900|JAMES|1300|30|
|7934|MILLER|1300|10|


```sql
DELETE t1 A 
 WHERE EXIST (SELECT 'X'
                FROM DEPT X
               WHERE X.LOC = 'DALLAS'
                 AND X.DEPTNO = A.DEPTNO);
```


|EMPNO|ENAME|SAL|DEPTNO|
|----:|:----|--:|-----:|
|7900|JAMES|1300|30|
|7934|MILLER|1300|10|


```sql
ROLLBACK; -- 취소
```
<br>
- DELETE 문 사전 검증 
  - 사전에 SELECT 문을 작성하여 삭제할 행을 미리 확인하면 실수를 방지할 수 있다.
- 사전 검증 SELECT 문
```sql
SELECT A.*
  FROM t1 A 
 WHERE EXIST (SELECT 'X'
                FROM DEPT X
               WHERE X.LOC = 'DALLAS'
                 AND X.DEPTNO = A.DEPTNO);
```

- UPDATE 문
```sql
DELETE t1 A 
 WHERE EXIST (SELECT 'X'
                FROM DEPT X
               WHERE X.LOC = 'DALLAS'
                 AND X.DEPTNO = A.DEPTNO);
```

```sql
ROLLBACK; -- 취소
```

---

### 4. MERGE <a id="chapter-4"></a>
- MERGE 문을 사용하면 조건에 따라 데이터를 갱신하거나 삽입할 수 있다.
  - 조인 조건을 만족하는 경우 갱신(UPDATE)를 수행하고, 만족하지 않을 경우 삽입(INSERT)를 수행한다.

```sql
MERGE INTO target_table a
USING (source_set) b
   ON (join condition)
 WHEN MATCHED THEN
      UPDATE
         SET a.COL1 = VAL1
            ,a.COL2 = VAL2
            ,...
    [WHERE condition]
  [DELETE
    WHERE condition]
 WHEN NOT MATCHED THEN
      INSERT (a.COL1, a.COL2, ...)
      VALUES (VAL1, VAL2, ...)
     [WHERE condition]
```

- 구문 설명
  - target_table : 갱신/삽입 대상 테이블 이름
  - source_set : 병합(merge)할 소스 집합 (table / view / subquery)
  - ON 절 : 대상 테이블과 소스 집합 간의 조인 조건
  - MERGE UPDATE (WHEN MATCHED THEN 이하) : ON 절의 조인 조건을 만족하는 행에 대해 수행될 구문
  - MERGE INTO (WHEN NOT MATCHED THEN 이하) : ON 절의 조인 조건을 만족하지 않는 행에 대해 수행될 구문
<br><br>

- 예시
```sql
INSERT INTO t1 (EMPNO, ENAME, SAL, DEPTNO)
SELECT A.EMPNO
      ,A.ENAME
      ,A.SAL
      ,A.DEPTNO
    FROM EMP A
   WHERE A.JOB = 'CLERK';
```


|EMPNO|ENAME|SAL|DEPTNO|
|----:|:----|--:|-----:|
|7369|SMITH|800|20|
|7876|ADAMS|1100|20|
|7900|JAMES|950|30|
|7934|MILLER|**1300**|10|



- MERGE 문 예시
```sql
MERGE INTO t1 a
USING (SELECT b.EMPNO
             ,b.ENAME
             ,b.DEPTNO
             ,b.SAL - 500 AS SAL
         FROM EMP b
        WHERE b.DEPTNO = 10) b
   ON (b.EMPNO = a.EMPTNO)
 WHEN MATCHED THEN
      UPDATE
         SET a.SAL = b.SAL
 WHEN NOT MATCHED THEN
      INSERT (a.EMPNO, a.ENAME, a.SAL, a.DEPTNO)
      VALUES (b.EMPNO, b.ENAME, b.SAL, b.DEPTNO)
```



|EMPNO|ENAME|SAL|DEPTNO|
|----:|:----|--:|-----:|
|7369|SMITH|800|20|
|7876|ADAMS|1100|20|
|7900|JAMES|950|30|
|7934|MILLER|**800**|10|
|**7839**|**KING**|**4500**|**10**|
|**7934**|**CLARK**|**1950**|**10**|



```sql
ROLLBACK; -- 취소
```


- MERGE 문 예시 (MERGE UPDATE 절 ONLY)
```sql
MERGE INTO t1 a
USING (SELECT b.EMPNO
             ,b.ENAME
             ,b.DEPTNO
             ,b.SAL - 500 AS SAL
         FROM EMP b
        WHERE b.DEPTNO = 10) b
   ON (b.EMPNO = a.EMPTNO)
 WHEN MATCHED THEN
      UPDATE
         SET a.SAL = b.SAL
```



|EMPNO|ENAME|SAL|DEPTNO|
|----:|:----|--:|-----:|
|7369|SMITH|800|20|
|7876|ADAMS|1100|20|
|7900|JAMES|950|30|
|7934|MILLER|**800**|10|



```sql
ROLLBACK; -- 취소
```


- MERGE 문 예시 (MERGE UPDATE 절 ONLY - DELETE 문)
```sql
INSERT INTO t1 (EMPNO, ENAME, SAL, DEPTNO)
SELECT A.EMPNO
      ,A.ENAME
      ,A.SAL
      ,A.DEPTNO
    FROM EMP A
   WHERE A.JOB = 'CLERK';
```


|EMPNO|ENAME|SAL|DEPTNO|
|----:|:----|--:|-----:|
|7369|SMITH|800|20|
|7876|ADAMS|1100|20|
|7900|JAMES|950|30|
|7934|MILLER|**1300**|10|


```sql
MERGE INTO t1 a
USING (SELECT b.EMPNO
             ,b.ENAME
             ,b.DEPTNO
             ,b.SAL - 500 AS SAL
         FROM EMP b
        WHERE b.DEPTNO = 10) b
   ON (b.EMPNO = a.EMPTNO)
 WHEN MATCHED THEN
      UPDATE -- 수행 1
         SET a.SAL = b.SAL
      DELETE -- 수행 2
       WHERE a.SAL < 1000;
```



|EMPNO|ENAME|SAL|DEPTNO|
|----:|:----|--:|-----:|
|7369|SMITH|800|20|
|7876|ADAMS|1100|20|
|7900|JAMES|950|30|


{: .important }
> MERGE UPDATE 절에 기술한 DELETE 문은 UPDATE 문에 의해 갱신된 행들 중에서 WHERE 절 조건에 만족하는 행들만 삭제한다.


```sql
ROLLBACK; -- 취소
```


- MERGE 문 예시 (MERGE INSERT 절 ONLY)
```sql
INSERT INTO t1 (EMPNO, ENAME, SAL, DEPTNO)
SELECT A.EMPNO
      ,A.ENAME
      ,A.SAL
      ,A.DEPTNO
    FROM EMP A
   WHERE A.JOB = 'CLERK';
```



|EMPNO|ENAME|SAL|DEPTNO|
|----:|:----|--:|-----:|
|7369|SMITH|800|20|
|7876|ADAMS|1100|20|
|7900|JAMES|950|30|
|7934|MILLER|1300|10|



```sql
MERGE INTO t1 a
USING (SELECT b.EMPNO
             ,b.ENAME
             ,b.DEPTNO
             ,b.SAL - 500 AS SAL
         FROM EMP b
        WHERE b.DEPTNO = 10) b
   ON (b.EMPNO = a.EMPTNO)
 WHEN NOT MATCHED THEN
      INSERT (a.EMPNO, a.ENAME, a.SAL, a.DEPTNO)
      VALUES (b.EMPNO, b.ENAME, b.SAL, b.DEPTNO)
```



|EMPNO|ENAME|SAL|DEPTNO|
|----:|:----|--:|-----:|
|7369|SMITH|800|20|
|7876|ADAMS|1100|20|
|7900|JAMES|950|30|
|7934|MILLER|1300|10|
|**7839**|**KING**|**4500**|**10**|
|**7934**|**CLARK**|**1950**|**10**|

 

```sql
ROLLBACK; -- 취소
```


- MERGE 문 예시 (MERGE INSERT 절 ONLY - WHERE 절)
```sql
INSERT INTO t1 (EMPNO, ENAME, SAL, DEPTNO)
SELECT A.EMPNO
      ,A.ENAME
      ,A.SAL
      ,A.DEPTNO
    FROM EMP A
   WHERE A.JOB = 'CLERK';
```



|EMPNO|ENAME|SAL|DEPTNO|
|----:|:----|--:|-----:|
|7369|SMITH|800|20|
|7876|ADAMS|1100|20|
|7900|JAMES|950|30|
|7934|MILLER|1300|10|



```sql
MERGE INTO t1 a
USING (SELECT b.EMPNO
             ,b.ENAME
             ,b.DEPTNO
             ,b.SAL - 500 AS SAL
         FROM EMP b
        WHERE b.DEPTNO = 10) b
   ON (b.EMPNO = a.EMPTNO)
 WHEN NOT MATCHED THEN
      INSERT (a.EMPNO, a.ENAME, a.SAL, a.DEPTNO)
      VALUES (b.EMPNO, b.ENAME, b.SAL, b.DEPTNO)
       WHERE b.SAL > 3000;
```



|EMPNO|ENAME|SAL|DEPTNO|
|----:|:----|--:|-----:|
|7369|SMITH|800|20|
|7876|ADAMS|1100|20|
|7900|JAMES|950|30|
|7934|MILLER|1300|10|
|**7839**|**KING**|**4500**|**10**|

 

 {: .important }
> MERGE INSERT 절에 기술한 WHERE 절을 기술하면 조건에 해당하는 행만 선택적으로 삽입할 수 있다.


```sql
ROLLBACK; -- 취소
```
