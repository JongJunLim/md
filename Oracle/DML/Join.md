---
title: JOIN
layout: default
parent: DML - SELECT
grand_parent: Oracle
nav_order: 2
---
<div style="text-align: right;">
작성일자 : 2023-07-05<br>
수정일자 : 2023-08-06
</div>


## <span style="background-color:#FFF5b1">1. Join</span> <a id="chapter-1"></a>
{: .d-inline-block }

Ver 0.1.4
{: .label .label-green }


- [1.Join](#chapter-1)<br>
    - [1-1.필요성](#chapter-1-1)<br>
    - [1-2.중복 제거](#chapter-2)<br>
    - [1-3.비등가 조인](#chapter-3)<br>
    - [1-4.순환 관계 데이터 모델 (계층형 모델)](#chapter-4)<br>
    - [1-5.Inner Join](#chapter-5)<br>
    - [1-6.Outer Join](#chapter-6)<br>
    - [1-7.Cross Join](#chapter-7)<br>
    - [1-8.ANSI 표준 조인(ANSI Standard Join)](#chapter-8)<br>
- [2.MULTIPLE JOIN](#chapter-9)
- [3.JOIN 실습](#chapter-10)

---

### **1-1. 필요성** <a id="chapter-1-1"></a>

관계형 데이터베이스의 구조적 특징으로 정규화를 수행하면 의미있는 데이터 집합으로 다수개의 테이블들이 구성되고 각 테이블끼리는 <span style="background-color:#FAB4B4">PK와 FK 값</span> 들끼리 관계를 갖게 된다. 정규화를 통해 데이터베이스는 저장 공간의 효율성과 확장성이 향상된다. 

이 과정에서 서로 관계가 있는 데이터가 여러 테이블로 나뉘어 저장되므로 각 테이블에 저장된 데이터를 효과적으로 검색하기 위해 Join이 필요하다.

조인은 두 테이블 간에 특정 컬럼(들)의 값을 비교하여, 비교 결과가 TRUE인 행들을 연결시킨다.FROM 절에는 조인할 테이블들을 콤마(,)로 구분하여 기술하고, WHERE 절에는 조인 조건(양쪽 테이블 컬럼들간의 비교 조건) 및 일반 조건을 함께 기술한다.

### **1-1. Necessity**

When normalization is performed with the structural features of relational databases, multiple tables are composed of a meaningful dataset, and each table has a relationship between <span style="background-color:#FAB4B4">PK and FK values</span>. Through normalization, databases improve storage efficiency and scalability.

In this process, data related to each other is divided into several tables and stored, so a join is needed to effectively search for data stored in each table.

The join compares the values of a specific column(s) between the two tables, connecting rows whose comparison result is TRUE.The FROM clause describes the tables to be joined by a comma (,) and the WHERE clause describes the joining conditions (comparative conditions between both table columns) and general conditions.

```sql
SELECT A.col, B.col, ...
  FROM Table A, Table B, ...
 WHERE Join Condition
   AND General Condition;
```



{: .important }
> 조인은 선행 집합의 조인 컬럼 값을 가지고, 후행 집합을 반복적으로 Lookup하면서 조인 조건이 TRUE가 되는 행을 서로 연결하여 결과 집합에 포함시킨다. <br> 집합을 읽는 순서는 DBMS(옵티마이저)에 의해 정해진다.
>
> The join has the value of the join column of the preceding set, and repeatedly looks up the trailing set, linking the rows whose join condition is TRUE to each other and including them in the result set. <br> The order in which the set is read is determined by the DBMS (optimizer).



```sql
SELECT B.EMPNO
      ,B.ENAME
      ,B.JOB
      ,A.DEPTNO
      ,A.DNAME
  FROM DEPT A, EMP B
 WHERE B.DEPTNO = A.DEPTNO          -- 조인 조건
   AND A.DNAME = 'RESEARCH'         -- 일반 조건
   AND B.JOB IN ('CLECK','ANALYST') -- 일반 조건
 ORDER BY B.SAL;
```



| EMPNO | ENAME | JOB | DEPTNO | DNAME |
|---:|:---|:---|---:|:---|
|7,902|FORD|ANALYST|20|RESEARCH|
|7,788|SCOTT|ANALYST|20|RESEARCH|



```sql
SELECT B.EMPNO
      ,B.ENAME
      ,B.JOB
      ,A.DEPTNO
      ,A.DNAME
  FROM DEPT A, EMP B
 WHERE B.DEPTNO = A.DEPTNO                      -- 조인 조건
 ORDER BY A.DEPTNO, TO_CHAR(B.HIREDATE, 'YYYY');
```


| EMPNO | ENAME | JOB | DEPTNO | DNAME |
|---:|:---|:---|---:|:---|
|7,782|	CLARK	|MANAGER	|10	|ACCOUNTING|
|7,839|	KING	|PRESIDENT|	10	|ACCOUNTING|
|7,934|	MILLER|	CLERK	|10|	ACCOUNTING|
|7,369|	SMITH	|CLERK	|20	|RESEARCH|
|7,566|	JONES	|MANAGER|	20|	RESEARCH|
|7,902|	FORD	|ANALYST|	20|	RESEARCH|
|7,788|	SCOTT	|ANALYST|	20|	RESEARCH|
|7,876|	ADAMS	|CLERK	|20|	RESEARCH|
|7,698|	BLAKE	|MANAGER|	30|	SALES|
|7,900|	JAMES	|CLERK	|30|	SALES|
|7,499|	ALLEN	|SALESMAN|	30|	SALES|
|7,844|	TURNER|	SALESMAN|	30|	SALES|
|7,654|	MARTIN|	SALESMAN|	30|	SALES|
|7,521|	WARD	|SALESMAN|	30|	SALES|


{: .warning }
> 1:M 관계에 있는 두 테이블을 조인하면, 1쪽 테이블의 행이 M쪽 테이블에 맞춰 늘어난다.
>
> When two tables in a 1:M relationship are joined, the rows of the one-sided table are stretched to match the M-side table.


```sql
SELECT COUNT(B.EMPNO)           AS CNT_EMP
      ,COUNT(A.DEPTNO)          AS WRONG_CNT_DEPT
      ,COUNT(DISTINCT A.DEPTNO) AS CNT_DEPT
  FROM DEPT A, EMP B
 WHERE B.DEPTNO = A.DEPTNO; 
```

|CNT_EMP|WRONG_CNT_DEPT|CNT_DEPT|
|---:|---:|---:|
|14|14|3|


---

### 1-2. **중복 제거(Deduplication)** <a id="chapter-2"></a>
{: .d-inline-block }

IMPORTANT
{: .label .label-red }
- Way 1) SELECT DISTINCT
```sql
SELECT DISTINCT -- DISTINCT를 추가
       person.id
      ,person.name
      ,job.job_name
  FROM person 
  INNER JOIN job
  ON person.name = job.person_name;
```

{: .warning }
> 매우 쉬운 방법이지만 레코드 수가 많은 경우 성능이 느림
>
> It's a very easy way, but slow performance with a large number of records


- Way 2) <span style="background-color:#FFF5b1">JOIN 전에 중복 제거(Deduplication before JOIN)</span>
  - 성능을 위해서 JOIN 전에 중복을 제거하는 방법 
  - Remove Duplication before JOIN for performance improvement
```sql
SELECT person.id
      ,person.name
      ,job.job_name
  FROM person 
  INNER JOIN (
    -- 중복 제거를 위한 inline view
              SELECT DISTINCT person_name
                             ,job_name
              FROM job
             ) AS job 
  ON person.name = job.person_name;
```

---

### 1-3.**비등가 조인 (<>, <, BETWEEN, LIKE)** <a id="chapter-3"></a>

```sql
SELECT *
  FROM EMP A, SALGRADE B
 WHERE A.SAL BETWEEN B.LOSAL AND B.HISAL;

 ```


| EMPNO | ENAME | JOB | MGR | HIREDATE | SAL | COMM |DEPTNO | GRADE | LOSAL |HISAL |
|---:|:---|:---|---:|:---|:---|:---|:---|:---|:---|:---|
|7369	|SMITH	|CLERK	|7902	|1980-12-17 00:00:00.000	|800| [NULL] |20	|1	|700	|1200|
|7900	|JAMES	|CLERK|	7698|	1981-12-03 00:00:00.000	|950| [NULL]|		30|	1|	700|	1200|
|7876|	ADAMS|	CLERK|	7788|	1987-05-23 00:00:00.000	|1100|	[NULL]|	20|	1|	700|	1200|
|7521|	WARD|	SALESMAN|	7698|	1981-02-22 00:00:00.000	1250|	500|	30|	2|	1201|	1400|
|7654|	MARTIN|	SALESMAN|	7698|	1981-09-28 00:00:00.000|	1250|	1400|	30|	2|	1201|	1400|


.
.
.


---

### 1-4. **순환 관계 데이터 모델 (계층형 모델)** <a id="chapter-4"></a>
  - 각 관리자는 여러 명의 사원을 관리 할 수 있다.
  - 각 사원은 한 명의 관리자를 가질 수 있다.

```sql
SELECT B.EMPNO
      ,B.ENAME AS "관리자"
      ,A.EMPNO
      ,A.ENAME AS "직원"
  FROM EMP A -- 사원
      ,EMP B -- 관리자
 WHERE B.EMPNO = A.MGR; 
 ```


|EMPNO|관리자|EMPNO|직원|
|---:|:---|---:|:---|
|7566|	JONES|	7902|	FORD|
|7566|	JONES|	7788|	SCOTT|
|7698|	BLAKE|	7844|	TURNER|
|7698|	BLAKE|	7499|	ALLEN|
|7698|	BLAKE|	7521|	WARD|
|7698|	BLAKE|	7900|	JAMES|
|7698|	BLAKE|	7654|	MARTIN|
|7782|	CLARK|	7934|	MILLER|
|7788|	SCOTT|	7876|	ADAMS|
|7839|	KING|	7698|	BLAKE|
|7839|	KING|	7566|	JONES|
|7839|	KING|	7782|	CLARK|


---

### 1-5. **Inner Join** <a id="chapter-5"></a>
  - 조인에 성공한 행들만 리턴하는 조인
  - Joins that return only rows that succeed in joining


---


### 1-6. **Outer Join** <a id="chapter-6"></a>
  - Inner Join의 결과와 조인에 실패한 기준 집합의 행들을 함께 리턴하는 조인
  - 오라클에서는 조인 조건에 (+) 기호를 사용하여 기준 집합을 지정한다.
  - <span style="background-color:#FAB4B4">(+) 기호가 사용된 쪽의 반대쪽 집합이 기준 집합</span>이 된다.
  - Join that returns the result of the Inner Join and the rows of the baseline set that failed the join
  - Oracle specifies a set of criteria using the (+) symbol for the join condition.
  - <span style="background-color:#FAB4B4">The opposite set of the side where the (+) symbol is used becomes the reference set</span>.

```sql
SELECT B.EMPNO
      ,B.ENAME
      ,B.JOB
      ,B.DEPTNO
      ,A.DEPTNO
      ,A.DNAME
      ,A.LOC
  FROM DEPT A, EMP B
 WHERE B.DEPTNO(+) = A.DEPTNO
   AND A.LOC IN ('NEW YORK','BOSTON');
```

```sql
SELECT B.EMPNO
      ,B.ENAME
      ,B.JOB
      ,B.DEPTNO
      ,A.DEPTNO
      ,A.DNAME
      ,A.LOC
  FROM DEPT A, EMP B
 WHERE B.DEPTNO(+) = A.DEPTNO -- 2) A.DEPTNO를 기준으로 조인이 되고 없는 값들도 보여짐
   AND B.DEPTNO(+) <> 20; -- 1) 20이 아닌 것들 먼저 거름
```

```sql
SELECT B.EMPNO
      ,B.ENAME
      ,B.JOB
      ,B.DEPTNO
      ,A.DEPTNO
      ,A.DNAME
      ,A.LOC
  FROM DEPT A, EMP B
 WHERE B.DEPTNO(+) = A.DEPTNO
   AND B.DEPTNO <> 20; -- (+) 기호 누락
```


---


### 1-7. **Cross Join** <a id="chapter-7"></a>
  - 양쪽 집합의 카타시안 곱(M*N건)을 리턴하는 조인
  - 양쪽 집합 간에 조인 조건이 없을 때 Cross Join이 수행된다.
  - Join that returns the Katashian product of both sets (M*N cases)
  - Cross join is performed when there is no join condition between both sets.

```sql
SELECT B.EMPNO
      ,B.ENAME
      ,B.JOB
      ,B.DEPTNO
      ,A.DEPTNO
      ,A.DNAME
      ,A.LOC
  FROM DEPT A, EMP B
 WHERE A.LOC IN ('NEW YORK','BOSTON')                 -- 일반 조건
   AND B.JOC IN ('ANALYST' , 'MANAGER', 'PRESIDENT'); -- 일반 조건
```

---

### 1-8. **ANSI 표준 조인(ANSI Standard Join)** <a id="chapter-8"></a>
- 오라클을 포함한 대부분의 상용 DBMS는 ANSI(American National Standards Institue) 표준 문법을 지원한다.
- ANSI 조인 문법은 조인 조건과 일반 조건을 구분하여 기술한다.
  - <span style="background-color:#FAB4B4">FROM 절에는 조인할 테이블과 조인 유형을 기술</span>한다.
  - <span style="background-color:#FAB4B4">FON 절에는 조인 조건을, WHERE 절에는 일반 조건을 기술</span>한다.
  
- Most commercial DBMSs, including Oracle, support the American National Standards Institute (ANSI) standard grammar.
- ANSI join grammar describes join conditions and general conditions separately.
  - <span style="background-color:#FAB4B4">The FROM section describes the table to join and the type of join.
  - <span style="background-color:#FAB4B4">Describe join conditions in FON clause and general conditions in WHERE clause</span>.

{: .important }
> ON 절에 기술한 조건은 모두 조인 조건으로 동작한다.
>
> All conditions described in the ON section operate as join conditions.



```sql
SELECT A.col, B.col, ...
  FROM Table A
  JOIN 
  TYPE Table B ... -- JOIN (INNER JOIN), LEFT JOIN, RIGHT JOIN, FULL OUTER JOIN
    ON (JOIN Condition)
 WHERE General Condition;
```


{: .highlight }
> WHERE 절에 비기준 집합에 대한 일반 조건을 기술하면 INNER JOIN이 수행된다.<br> (ON 절 안에 일반 조건 넣어주면 FULL OUTER 수행 됨)
>
> Describing general conditions for non-reference sets in the WHERE clause results in INNER JOIN.<br> (If you put general conditions in the ON section, full out will be performed)


---

## 2.MULTIPLE JOIN <a id="chapter-9"></a>


**INNER JOIN** 

ORACLE

```sql
SELECT ...
  FROM Table_X X, Table_Y Y, Table_Z Z
 WHERE X.COL_A = Y.COL_B
   AND Y.COL_C = Z.COL_D 
```

ANSI 표준

```sql
SELECT ...
  FROM Table_X X
  JOIN Table_Y Y
    ON X.COL_A = Y.COL_B
  JOIN Table_Z Z
    ON Y.COL_C = Z.COL_D 
```

**OUTER JOIN** 

: LEFT JOIN을 사용하여 중심이 되는 테이블의 모든 레코드를 출력하는 방식

ORACLE

```sql
SELECT ... 
  FROM Table_X X, Table_Y Y, Table_Z Z
 WHERE X.COL_A = Y.COL_B (+)
   AND Y.COL_C = Z.COL_D (+)
```

ANSI 표준


```sql
SELECT ...
  FROM Table_X X
  LEFT JOIN Table_Y Y
    ON X.COL_A = Y.COL_B
  LEFT JOIN Table_Z Z
    ON Y.COL_C = Z.COL_D 
```

**조회시 기준이 되는(모든 컬럼을 출력할) 테이블을 중심으로 LEFT JOIN 등으로 통일하면 혼동하지 않을 수 있음**



---

## 3.JOIN 실습 <a id="chapter-10"></a>
해당 실습은 클래스 101 강의 중 하나의 실습 문제를 적어둔 것입니다.

Q1. 10번 부서에 속하는 모든 사원의 직무와 부서 위치를 표시하시요.

```sql
SELECT A.JOB
      ,A.ENAME
      ,B.DNAME
      ,B.LOC
   FROM EMP A, DEPT B
   WHERE A.DEPTNO = 10
     AND A.DEPTNO = B.DEPTNO;
```

Q2. "CHICAGO"에서 근무하는 모든 사원의 이름, 직무, 부서 번호 및 부서 이름을 표시하십시오.


```sql
SELECT A.ENAME
      ,A.JOB
      ,A.DEPTNO
      ,B.DNAME
   FROM EMP A,DEPT B
   WHERE B.LOC = 'CHICAGO'
     AND A.DEPTNO = b.DEPTNO;
```

Q3. 각 사원의 이름 및 사원 번호를 해당 관리자의 이름 및 사원 번호와 함께 표시하고, 레이블을 순서대로 EMP_NM, EMP#, MGR_NM, MGR#로 지정하오. (단, 관리자가 없는 사원은 포함하지 않는다.)

```sql
SELECT A.ENAME  AS EMP_NM
      ,A.EMPNO  AS EMP#
      ,B.ENAME  AS MGR_NM
      ,B.EMPNO  AS MGR#
   FROM EMP A -- 사원 
        ,EMP B -- 관리자 
   WHERE A.MGR = B.EMPNO ;     
```    
    
Q4. 각 사원 이름 미 사원 번호를 해당 관리자의 이름 및 사원 번호와 함께 표시하고 레이블 순서대로 EMP_NM, EMP#, MGR_NM, MGR#로 지정하시오. <br>(단, 관리자가 없는 사원도 포함하여 표시하고, 결과 집합은 사원 번호(EMP#)기준으로 정렬하시오.

```sql
SELECT A.ENAME  AS EMP_NM
      ,A.EMPNO  AS EMP#
      ,B.ENAME  AS MGR_NM
      ,B.EMPNO  AS MGR#
   FROM EMP A -- 사원 
        ,EMP B -- 관리자 
   WHERE A.MGR = B.EMPNO(+) 
   ORDER BY A.EMPNO;  
```
Q5."Marting"과 같은 부서의 사원을 표시하고, 레이블을 순서대로 ENAME, DEPTNO, ENAME_1로 지정하시오.

```sql
SELECT A.ENAME  AS ENAME
      ,B.DEPTNO AS DEPTNO
      ,B.ENAME  AS ENAME_1 
   FROM EMP A  -- MARTIN
	    ,EMP B  -- MARTIN 과 동일 부서 사원 
   WHERE A.EMPNO = '7654'
     AND A.DEPTNO = B.DEPTNO
     AND B.EMPNO  <> A.EMPNO; --  OR B.ENAME <> 'MARTIN'
```

Q6."CLARK" 사원보다 늦게 입사한 사원의 이름 입사 일자를 표시하시오.
```sql
SELECT MAX(B.ENAME)    AS ENAME
      ,MIN(B.HIREDATE) AS HIREDATE
   FROM EMP A -- CLARK
       ,EMP B -- CLARK 보다 늦게 입사한 사
   WHERE A.ENAME = 'CLARK'
     AND A.HIREDATE < B.HIREDATE
     AND B.ENAME <> 'CLARK'
     GROUP BY B.EMPNO;   -- 동명이인이 있을경우를 위해 GROUP BY와, MIN, MAX 활용 
```

Self Join 실습<br>
Q1.
```sql
SELECT A.EMPNO
      ,A.ENAME
      ,A.MGR
	FROM emp A
	WHERE A.MGR IS NULL;
```
Q2.
```sql
SELECT B.EMPNO
      ,B.ENAME
      ,A.EMPNO
      ,A.ENAME 
   FROM EMP A, EMP B
    WHERE A.MGR IS NULL
      AND A.EMPNO = B.MGR; 
```
Q3.
```sql
SELECT C.EMPNO
      ,C.ENAME
      ,B.EMPNO
      ,B.ENAME
      ,A.EMPNO
      ,A.ENAME
 FROM EMP A --1번 사원 
     ,EMP B --2번 사원 / 1번 사원의 하위 사원
     ,EMP C --3번 사원 / 2번 사원의 하위 사원
WHERE A.MGR IS NULL
  AND A.EMPNO = B.MGR
  AND B.EMPNO = C.MGR;
```

Q4.
```sql
SELECT D.EMPNO
      ,D.ENAME
      ,C.EMPNO
      ,C.ENAME
      ,B.EMPNO
      ,B.ENAME
      ,A.EMPNO
      ,A.ENAME
 FROM EMP A --1번 사원 
     ,EMP B --2번 사원 / 1번 사원의 하위 사원
     ,EMP C --3번 사원 / 2번 사원의 하위 사원
     ,EMP D --4번 사원 / 3번 사원의 하위 사원
     WHERE A.MGR IS NULL
	  AND A.EMPNO = B.MGR(+)
	  AND B.EMPNO = C.MGR(+)
	  AND C.EMPNO = D.MGR(+);
```
