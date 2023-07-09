---
title: ROWNUM
layout: default
parent: DML
grand_parent: Oracle
nav_order: 4
---
<div style="text-align: right;">
작성일자 : 2023-07-09
</div>

## <span style="background-color:#FFF5b1">ROWNUM</span>
{: .d-inline-block }

Ver 0.1.1
{: .label .label-green }

## 1.ROWNUM
Oracle의 ROWNUM은 칼럼과 비슷한 성격의 Pseudo Column으로써 SQL 처리 결과 집합의 각 행에 대해 <span style="background-color:#FAB4B4">임시로 부여되는 일련번호</span>이며, 원하는 만큼의 행만 가져오고 싶을 때 WHERE 절에서 행의 개수를 제한하는 목적으로 사용한다.

WHERE 절에 ROWNUM 조건을 사용하면 조회되는 행의 개수를 제한할 수 있다.
- ROWNUM = 1이나 ROWNUM >=1 등은 결과가 출력되긴 하지만 사용을 권장하지는 않는다. <span style="background-color:#FAB4B4">(ROWNUM <= 1 사용 권장)</span>

Oracle's ROWNUM is a column-like Pseudo Column, <span style="background-color:#FAB4B4">a temporary serial number</span> given to each row in the SQL processing result set, and is used to limit the number of rows in the WHERE clause when you want to import only as many rows as you want.

The ROWNUM condition in the WHERE clause can be used to limit the number of rows that are queried.
- ROWNUM = 1 or ROWNUM >=1 are output, but are not recommended for use. <span style="background-color:#FAB4B4">(Recommended using ROWNUM <= 1)</span>

```sql
SELECT ROWNUM
      ,e.EMPNO
      ,e.ENAME 
      ,e.JOB 
      ,e.DEPTNO 
    FROM EMP e  
   WHERE e.JOB IN ('ANALYST','CLERK');
```


|ROWNUM|EMPNO|ENAME|JOB|DEPTNO|
|----:|----:|:----|:----|----:|
|1|	7369|	SMITH|	CLERK|	20|
|2|	7788|	SCOTT|	ANALYST|	20|
|3|	7876|	ADAMS|	CLERK|	20|
|4|	7900|	JAMES|	CLERK|	30|
|5|	7902|	FORD|	ANALYST|	20|
|6|	7934|	MILLER|	CLERK|	10|


```sql
SELECT ROWNUM
      ,e.EMPNO
      ,e.ENAME 
      ,e.JOB 
      ,e.DEPTNO 
   FROM EMP e  
  WHERE e.DEPTNO = 20; 
```


|ROWNUM|EMPNO|ENAME|JOB|DEPTNO|
|----:|----:|:----|:----|----:|
|1|	7369|	SMITH|	CLERK|	20|
|2|	7566|	JONES|	MANAGER|	20|
|3|	7788|	SCOTT|	ANALYST|	20|
|4|	7876|	ADAMS|	CLERK|	20|
|5|	7902|	FORD|	ANALYST|	20|	 
 


```sql 
 SELECT ROWNUM
 	   ,e.EMPNO
 	   ,e.ENAME
	FROM EMP e 
   WHERE ROWNUM <= 5;
```	


|ROWNUM|EMPNO|ENAME|
|----:|----:|:----|
|1|	7369|	SMITH|
|2|	7499|	ALLEN|
|3|	7521|	WARD|
|4|	7566|	JONES|
|5|	7654|	MARTIN|


인라인 뷰 내에서 ROWNUM 값에 칼럼 별칭을 지정하면 마치 칼럼처럼 사용이 가능하다
 
If specify a column alias for the ROWNUM value within an inline view, it can be used as if it were a column

```sql
 SELECT e.RN
       ,e.EMPNO
       ,e.ENAME
 	FROM (
 		  SELECT ROWNUM AS RN
 		  		,e.EMPNO
 		  		,e.ENAME
 		  	FROM EMP e
 		 ) e
	WHERE e.RN > 5;
```
	

|RN|EMPNO|ENAME|
|----:|----:|:----|
|6|	7698|	BLAKE|
|7|	7782|	CLARK|
|8|	7788|	SCOTT|
|9|	7839|	KING|
|10|	7844|	TURNER|
|11|	7876|	ADAMS|
|12|	7900|	JAMES|
|13|	7902|	FORD|
|14|	7934|	MILLER|



{: .warning}
> 오라클은 정렬이 완료된 후 데이터를 출력하는게 아니라, 데이터 일부를 추출한 후에 정렬작업이 일어난다. 
>
> Oracle does not output data after sorting is completed, but the sorting operation occurs after extracting some of the data.

틀린쿼리(Wrong)
```sql
SELECT ROWNUM
      ,ENAME
      ,SAL
    FROM EMP
   WHERE ROWNUM < 4    -- 1 
   ORDER BY SAL DESC  ;   -- 2
```

|ROWNUM|ENAME|SAL|
|----:|:----|----:|
|2|	ALLEN|	1600|
|3|	WARD|	1250|
|1|	SMITH|	800|

올바른 쿼리(Correct)
```sql
SELECT ROWNUM
      ,ENAME
      ,SAL
    FROM ( SELECT ENAME
                 ,SAL
            FROM EMP
            ORDER BY SAL DESC  )   -- 인라인 뷰를 이용해서 데이터셋을 구성해줘야 한다. 
    WHERE ROWNUM < 4 ;
```

|ROWNUM|ENAME|SAL|
|:----|:----|----:|
|1|	KING|	5000|
|2|	SCOTT|	3000|
|3|	FORD|	3000|


평균 급여가 높은 순으로 3개 직무
3 jobs in the order of average salary

```sql
SELECT rownum 
       ,a.job
       ,a.cnt_emp
       ,a.avg_sal
	FROM (
	 	  SELECT a.job
	 	        ,count(a.empno)       AS cnt_emp
	 	        ,round(avg(a.sal), 1) AS avg_sal
	 	    FROM emp a
	 	   GROUP BY a.job
	 	   ORDER BY avg(a.sal) DESC) a
	WHERE ROWNUM <= 3;
```

|ROWNUM|JOB|CNT_EMP|AVT_SAL|
|----:|:----|----:|---:|
|1|	PRESIDENT|	1|	5000|
|2|	ANALYST|	2|	3000|
|3|	MANAGER|	3|	2758.3|


## 1-2.TOP N
특정 기준으로 상위 N개를 추출하는 쿼리를 TOP-N쿼리라고 한다.

TOP 절
SQL Server는 TOP 절을 사용하여 결과 집합으로 출력되는 행의 수를 제한할 수 있다. TOP 절의 표현식은 다음과 같다.

A query that extracts the TOP N on a specific basis is called a TOP-N query.

TOP clause
SQL Server can use TOP clauses to limit the number of rows output to the result set. The expressions in the TOP section are as follows.

TOP (N) [PERCENT] [WITH TIES]
 
1. TOP N : 검색건수를 N개로 제한(Limit the number of searches to N)

월급 상위 5명의 직원정보 조회
Employee information of the top 5 salaries
```sql
SELECT TOP 5 * 
    FROM EMP 
    ORDER BY SAL DESC;
```
 
2. TOP N PERSENT : 검색건수를 N%로 제한(Limit the number of searches to N%)

월급 상위 5%인 직원정보 조회
Employee information of the top 5% salaries
```sql
SELECT TOP 5 PERSENT * 
     FROM EMP 
     ORDER BY SAL DESC;
```

3. TOP N WITH TIES : 하한선의 동일한 값을 모두 조회(Query all equal values of lower bound)

가령, SAL 컬럼 값이 5000, 4000, 3000, 3000, 3000, 3000, 3000, 1000, 500 이 테이블에 입력되어있을 경우

월급 상위 3명을 동일한 하한선의 직원정보들까지 조회

For example, if the SAL column values are 5000, 4000, 3000, 3000, 3000, 3000, 3000, 1000, and 500 in the table

Check the top 3 salaries up to employee information at the same lower limit
```sql
SELECT TOP 3 WITH TIES * 
    FROM EMP 
    ORDER BY SAL DESC;
```

3명만 조회한다고 설정하였지만 하한선의 동일한 값을 모두 출력한다
결과 : 5000, 4000, 3000, 3000, 3000, 3000, 3000
총 6개의 결과가 검색되었다. (3번째로 높은값인 3000이 4개 포함됨)

It is set to inquire only three people, but all the same values of the lower limit are output
Results: 5000, 4000, 3000, 3000, 3000, 3000, 3000, 3000, 3000
A total of six results were searched. (Includes 4 of the 3rd highest value 3000)

## 2. ROW_NUMBER()
ORDER BY 된 결과에 순번을 매길때에는 ROWNUM 보다 ROW_NUMBER() 함수가 더 편하다.

When ordering ORDER BY results, the ROW_NUMBER() function is more convenient than ROWNUM.

```sql
SELECT ROW_NUMBER() OVER(ORDER BY a.job, a.ename) ROW_NUMBER     
      ,a.EMPNO
      ,a.ENAME
      ,a.JOB   
  FROM emp a  
 ORDER BY a.job, a.ename;
```


|ROWNUM|JOB|CNT_EMP|AVT_SAL|
|----:|:----|----:|---:|
|1|	7902|	FORD|	ANALYST|
|2|	7788|	SCOTT|	ANALYST|
|3|	7876|	ADAMS|	CLERK|
|4|	7900|	JAMES|	CLERK|
|5|	7934|	MILLER|	CLERK|
|6|	7369|	SMITH|	CLERK|
|7|	7698|	BLAKE|	MANAGER|
|8|	7782|	CLARK|	MANAGER|
|9|	7566|	JONES|	MANAGER|
|10|	7839|	KING|	PRESIDENT|
|11|	7499|	ALLEN|	SALESMAN|
|12|	7654|	MARTIN|	SALESMAN|
|13|	7844|	TURNER|	SALESMAN|
|14|	7521|	WARD| SALESMAN|



<span style="background-color:#FAB4B4">그룹별(PARTITION)</span> 로 순번을 따로 부여할 수 있다.

Order can be assigned separately by <span style="background-color:#FAB4B4">group (PARTITION).</span>

```sql
SELECT ROW_NUMBER() OVER(PARTITION BY a.job ORDER BY a.job, a.ename) ROW_NUMBER      
      ,a.EMPNO
      ,a.ENAME
      ,a.JOB 
  FROM emp a  
 ORDER BY a.job, a.ename;
```


|ROWNUM|JOB|CNT_EMP|AVT_SAL|
|----:|:----|----:|---:|
|1|	7902|	FORD|	ANALYST|
|2|	7788|	SCOTT|	ANALYST|
|1|	7876|	ADAMS|	CLERK|
|2|	7900|	JAMES|	CLERK|
|3|	7934|	MILLER|	CLERK|
|4|	7369|	SMITH|	CLERK|
|1|	7698|	BLAKE|	MANAGER|
|2|	7782|	CLARK|	MANAGER|
|3|	7566|	JONES|	MANAGER|
|1|	7839|	KING|	PRESIDENT|
|1|	7499|	ALLEN|	SALESMAN|
|2|	7654|	MARTIN|	SALESMAN|
|3|	7844|	TURNER|	SALESMAN|
|4|	7521|	WARD| SALESMAN|
