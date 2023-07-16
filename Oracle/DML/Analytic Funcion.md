---
title: PARTITION/WINDOW
layout: default
parent: DML
grand_parent: Oracle
nav_order: 6
---
<div style="text-align: right;">
작성일자 : 2023-07-12
수정일자 : 2023-07-16
</div>

## <span style="background-color:#FFF5b1">Analytic Funcion</span>
{: .d-inline-block }

Ver 0.1.2
{: .label .label-green }


## 1. 분석 함수(Analytic Fungtion)
- 개별 행을 유지한 채 집계 값을 계산할 수 있는 함수이다.
  - 집계 함수 (Aggregate Functions)는 행 그룹 별로 값을 집계하고, 각 행 그룹을 단일 행 (1 row)으로 그룹화하여 리턴한다. 하지만 분석함수는 개별 행을 유지한 채로 값을 집계하기 때문에 원본 값과 집계 값을 함께 분석할 수 있다.
  - 분석 함수 실행시 대상이 되는 행의 범위를 윈도우(window)라고 하며, analytic_clause에 의해 각 행(current row) 별로 윈도우가 정의 된다.
- 모든 join, where절, group by 절, having 절의 수행은 분석 함수 실행 전에 완료 된다. 따라서 분석함수는 select 절, order by 절 내에서만 사용할 수 있다.

## 2. PARTITION
- PARTITION BY 절을 사용하여 GROUPO BY 절과 유사하게 논리적인 행의 그룹을 생성할 수 있으며, 각 행의 윈도우(실행 대상 행의 범위)는 파티션을 벗어날 수 없다.
  

```sql 
SELECT a.empno
      ,a.ename
      ,a.hiredate
      ,a.deptno
      ,a.sal
      ,SUM(a.sal) OVER (PARTITION BY a.deptno) AS SUM_SAL
  FROM emp a;
```

|EMPNO|ENAME|HIREDATE|DEPTNO|SAL|SUM_SAL|
|---:|:---|---:|---:|---:|---:|
|7782	|CLARK	|1981-06-09 00:00:00.000|	10|	2450|8750|
|7839	|KING	|1981-11-17 00:00:00.000|	10|	5000|	8750|
|7934	|MILLER	|1982-01-23 00:00:00.000|	10|	1300|	8750|
|7566	|JONES	|1981-04-02 00:00:00.000|	20|	2975|	10875|
|7902	|FORD	|1981-12-03 00:00:00.000|	20|	3000|	10875|
|7876	|ADAMS	|1987-05-23 00:00:00.000|	20|	1100|	10875|
|7369	|SMITH	|1980-12-17 00:00:00.000|	20|	800|	10875|
|7788	|SCOTT	|1987-04-19 00:00:00.000|	20|	3000|	10875|
|7521	|WARD	|1981-02-22 00:00:00.000|	30|	1250|	9400|
|7844	|TURNER	|1981-09-08 00:00:00.000|	30|	1500|	9400|
|7499	|ALLEN	|1981-02-20 00:00:00.000|	30|	1600|	9400|
|7900	|JAMES	|1981-12-03 00:00:00.000|	30|	950|	9400|
|7698	|BLAKE	|1981-05-01 00:00:00.000|	30|	2850|	9400|
|7654	|MARTIN	|1981-09-28 00:00:00.000|	30|	1250|	9400|


- 분석 함수 활용 (다양한 집계 기준)
  - PARTITION BY 절만 변경하면 여러 집계 기준의 집계 값을 한번에 추출할 수 있다.

```sql
SELECT a.empno
      ,a.ename
      ,a.hiredate
      ,a.deptno
      ,a.sal
      ,SUM(a.sal) OVER (PARTITION BY a.deptno) AS DEPTNO_SUM_SAL
      ,SUM(a.sal) OVER (PARTITION BY a.job) AS JOB_SUM_SAL
  FROM emp a;
```


|EMPNO|ENAME|HIREDATE|DEPTNO|SAL|DEPTNO_SUM_SAL|JOB_SUM_SAL|
|---:|:---|---:|---:|---:|---:|---:|
|7782	|CLARK	|1981-06-09 00:00:00.000|	10|	2450|8750|4150|
|7839	|KING	|1981-11-17 00:00:00.000|	10|	5000|	8750|5000|
|7934	|MILLER	|1982-01-23 00:00:00.000|	10|	1300|	8750|8275|
|7566	|JONES	|1981-04-02 00:00:00.000|	20|	2975|	10875|4150|
|7902	|FORD	|1981-12-03 00:00:00.000|	20|	3000|	10875|6000|
|7876	|ADAMS	|1987-05-23 00:00:00.000|	20|	1100|	10875|4150|
|7369	|SMITH	|1980-12-17 00:00:00.000|	20|	800|	10875|6000|
|7788	|SCOTT	|1987-04-19 00:00:00.000|	20|	3000|	10875|8275|
|7521	|WARD	|1981-02-22 00:00:00.000|	30|	1250|	9400|5600|
|7844	|TURNER	|1981-09-08 00:00:00.000|	30|	1500|	9400|5600|
|7499	|ALLEN	|1981-02-20 00:00:00.000|	30|	1600|	9400|5600|
|7900	|JAMES	|1981-12-03 00:00:00.000|	30|	950|	9400|5600|
|7698	|BLAKE	|1981-05-01 00:00:00.000|	30|	2850|	9400|4150|
|7654	|MARTIN	|1981-09-28 00:00:00.000|	30|	1250|	9400|8275|



- 분석 함수 활용 (누적 집계)
  - 파티션 내에서 ORDER BY 절에 기술한 순서대로 현재 행까지의 값을 누적 집계 할수 있다.
    - PARTITION BY 절이 생략되면 전체 행을 대상으로 누적 집계 값을 계산한다. (애매한 부분이 있음 - 동일 값 집계 X)


```sql
SELECT a.empno
      ,a.ename
      ,a.hiredate
      ,a.deptno
      ,a.sal
      ,SUM(a.sal) OVER (PARTITION BY a.deptno ORDER BY a.sal) AS SUM_SAL
  FROM emp a;
```

## 2-1. LAG / LEAD
오라클에서 이전 행의 값을 찾거나 다음 행의  값을 찾기 위해서는 LAG, LEAD 함수를 사용하면 된다.
```sql
LAG(expr [,offset] [,default]) OVER([partition_by_clause] order_by_clause)
```

```sql
  - LEAD(expr [,offset] [,default]) OVER([partition_by_clause] order_by_clause)
```
  - LAG 함수 : 이전 행의 값을 리턴
  - LEAD 함수 : 다음 행의 값을 리턴
  - expr : 대상 컬럼명
  - offset : 값을 가져올 행의 위치 기본값은 1, 생략가능
  - default : 값이 없을 경우 기본값, 생략가능
  - partition_by_clause : 그룹 컬럼명, 생략가능
  - order_by_clause : 정렬 컬럼명, 필수


기본 사용법
```sql
SELECT empno
     , ename
     , job
     , sal
     , LAG(empno) OVER(ORDER BY empno)  AS empno_prev
     , LEAD(empno) OVER(ORDER BY empno) AS empno_next
  FROM emp 
 WHERE job IN ('MANAGER', 'ANALYST', 'SALESMAN')
 ```


|EMPNO|ENAME|JOB|SAL|EMPNO_PREV|EMPNO_NEXT|
|---:|:---|:---|---:|---:|---:|
|7499|	ALLEN|	SALESMAN|	1600|		|7521|
|7521|	WARD	|SALESMAN|	1250|	7499|	7566|
|7566|	JONES	|MANAGER|	2975	|7521|	7654|
|7654|	MARTIN|	SALESMAN|	1250|	7566|	7698|
|7698|	BLAKE	|MANAGER|	2850|	7654|	7782|
|7782|	CLARK	|MANAGER|	2450|	7698|	7788|
|7788|	SCOTT	|ANALYST|	3000|	7782|	7844|
|7844|	TURNER|	SALESMAN|	1500|	7788|	7902|
|7902|	FORD	|ANALYST|	3000|	7844|	|



현재 행 기준으로 두번째 이전 값을 표시
```sql
SELECT empno
     , ename
     , job
     , sal
     , LAG(empno, 2) OVER(ORDER BY empno) AS empno_prev
  FROM emp 
 WHERE job IN ('MANAGER', 'ANALYST', 'SALESMAN')
```

|EMPNO|ENAME|JOB|SAL|EMPNO_PREV|
|---:|:---|:---|---:|---:|
|7499|	ALLEN|	SALESMAN|	1600|		|
|7521|	WARD	|SALESMAN|	1250|	|
|7566|	JONES	|MANAGER|	2975	|7499|
|7654|	MARTIN|	SALESMAN|	1250|	7521|
|7698|	BLAKE	|MANAGER|	2850|	7666|
|7782|	CLARK	|MANAGER|	2450|	7654|
|7788|	SCOTT	|ANALYST|	3000|	7698|
|7844|	TURNER|	SALESMAN|	1500|	7782|
|7902|	FORD	|ANALYST|	3000| 7788|

가져올 행이 없는 경우 기본값(9999)를 가져온다.
```sql
SELECT empno
     , ename
     , job
     , sal
     , LAG(empno, 2, 9999) OVER(ORDER BY empno) AS empno_prev
  FROM emp 
 WHERE job IN ('MANAGER', 'ANALYST', 'SALESMAN')
```
|EMPNO|ENAME|JOB|SAL|EMPNO_PREV|
|---:|:---|:---|---:|---:|
|7499|	ALLEN|	SALESMAN|	1600|	9999|
|7521|	WARD	|SALESMAN|	1250|	9999|
|7566|	JONES	|MANAGER|	2975	|7499|
|7654|	MARTIN|	SALESMAN|	1250|	7521|
|7698|	BLAKE	|MANAGER|	2850|	7666|
|7782|	CLARK	|MANAGER|	2450|	7654|
|7788|	SCOTT	|ANALYST|	3000|	7698|
|7844|	TURNER|	SALESMAN|	1500|	7782|
|7902|	FORD	|ANALYST|	3000| 7788|

## 3. Window
- 각 행마다 분석함수의 실행 대상이 되는 행의 범위(윈도우)를 세밀하게 지정한다.
- 물리적인 행의 범위를 지정하거나 논리적인 값의 범위를 지정할 수 있다.
  - analytic_clause 내 order by 절의 컬럼 또는 표현식이 윈도우의 기준이 된다.
- 각 행의 윈도우(실행 대상 행의 범위)는 파티션을 벗어날 수 없다.
- Syntax : ROWS/RANGE BETWEEN start_point and end_point
  - Start_point & end_point : ROWS (시작행 / 종료행) / RANGE (값의 범위)
  - UNBOUNDED PRECEDING : 파티션의 첫 번째 행 / 파티션의 첫 번째 행의 값
  - N PRECEDING : (파티션 내에서) 현재 행을 기준으로 이전 N번째 행 / 현재 행의 값 - N
  - CURRENT ROW : 현재 행 / 현재 행의 값
  - N FOLLOWING : (파티션 내에서) 현재 행을 기준으로 이후 N번째 행 / 현재 행의 값 + N
  - UNBOUNDED FOLLOWING : 파티션의 마지막 행 / 파티션의 마지막 행의 값



UNBOUNDED PRECEDING : 파티션의 첫 번째 행 / CURRENT ROW : 현재 행 -- 누적 집계
```sql
SELECT a.empno
      ,a.ename
      ,a.hiredate
      ,a.deptno
      ,a.sal
      ,SUM(a.sal) OVER (PARTITION BY a.deptno ORDER BY a.sal --정렬 기준
                        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT_ROW) AS SUM_SAL
  FROM emp a;
```


N PRECEDING : (파티션 내에서) 현재 행을 기준으로 이전 N번째 행 / N FOLLOWING : (파티션 내에서) 현재 행을 기준으로 이후 N번째 행

```sql
SELECT a.empno
      ,a.ename
      ,a.hiredate
      ,a.deptno
      ,a.sal
      ,SUM(a.sal) OVER (PARTITION BY a.deptno ORDER BY a.sal --정렬 기준
                        ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) AS SUM_SAL
  FROM emp a;
```



 RANGE

```sql
SELECT a.empno
      ,a.ename
      ,a.hiredate
      ,a.deptno
      ,a.sal
      ,SUM(a.sal) OVER (PARTITION BY a.deptno ORDER BY a.sal --정렬 기준
                        RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT_ROW) AS SUM_SAL
  FROM emp a;
```

ROWS / RANGE BETWEEN UNBOUNDED PRECEDING = ROWS / RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT_ROW (명시해주는 것이 좋음)
