---
title: PIVOT/UNPIVOT
layout: default
parent: DML
grand_parent: Oracle
nav_order: 7
---
<div style="text-align: right;">
작성일자 : 2023-07-15
</div>

## <span style="background-color:#FFF5b1">PIVOT/UNPIVOT</span>
{: .d-inline-block }

Ver 0.1.1
{: .label .label-green }


## 1. PIVOT

- Oracle 11g 버전부터 사용 가능
  - FROM 절과 WHERE 절 사이에 기술한다.
	- aggregate_funtion(arg)에는 PIVOT 결과로 출력할 값을 지정
	- FOR 절에는 PIVOT 기준 컬럼을 지정
	- IN 절에는 PIVOT 기준 컬럼의 값을 지정

Syntax
```sql
SELECT Col1, Col2, ...
	FROM Table/Inline View
   PIVOT (aggregate_funtsion(arg) [, aggregate_function(arg2),...]
   		FOR Col1 [,Col2, ...]
		  IN (Value1 [, Value2,...)
		 );
```

Example
```sql
SELECT *
	FROM (SELECT a.DEPTNO
                    ,a.SAL 
                 FROM EMP a) a
	PIVOT (sum(a.SAL)
		FOR deptno 
	 	 IN (10
                    ,20
                    ,30)
	      );
```


|10|20|30|
|---:|---:|---:|
|8,750|10,875|9,400|


부분 집계를 활용한 PIVOT 기능 구현
```sql
SELECT SUM(CASE WHEN a.deptno = 10 THEN a.sal END) AS D10_SUMSAL
      ,SUM(CASE WHEN a.deptno = 20 THEN a.sal END) AS D20_SUMSAL
      ,SUM(CASE WHEN a.deptno = 30 THEN a.sal END) AS D30_SUMSAL
	FROM emp a;
```


|D10_SUMSAL|D20_SUMSAL|D30_SUMSAL|
|---:|---:|---:|
|8,750|10,875|9,400|



- PIVOT 절 내에서 참조하지 않은 컬럼들을 기준으로 행이 그룹화 된다.
  - 일반적으로 인라인 뷰 or with 문으로 컬럼을 한정한다

```sql
SELECT *
	FROM emp a
	PIVOT (SUM(a.sal)
		FOR deptno
		 IN (10
		    ,20
		    ,30)
	      );
```


|EMPNO|ENAME|JOB|MGRHIREDATE|COMM|10|20|30|40|
|---:|:---|:---|---:|---:|---:|---:|---:|---:|
|7369|	SMITH|	CLERK|	7902|	1980-12-17 00:00:00.000|||			800||	
|7499|	ALLEN|	SALESMAN|	7698|	1981-02-20 00:00:00.000|	300|||			1600|
|7521|	WARD|	SALESMAN|	7698|	1981-02-22 00:00:00.000|	500|||		1250|
|7566|	JONES|	MANAGER|	7839|	1981-04-02 00:00:00.000|||			2975||	
|7654|	MARTIN|	SALESMAN|	7698|	1981-09-28 00:00:00.000|	1400|||			1250|
|7698|	BLAKE|	MANAGER|	7839|	1981-05-01 00:00:00.000||||				2850|
|7782|	CLARK|	MANAGER|	7839|	1981-06-09 00:00:00.000||		2450	|||	
|7788|	SCOTT|	ANALYST|	7566|	1987-04-19 00:00:00.000|||			3000	||
|7839|	KING|	PRESIDENT|		|1981-11-17 00:00:00.000||		5000|||		
|7844|	TURNER|	SALESMAN|	7698|	1981-09-08 00:00:00.000|	0	|||	1500|
|7876|	ADAMS|	CLERK|	7788	|1987-05-23 00:00:00.000|||			1100	||
|7900|	JAMES|	CLERK|	7698	|1981-12-03 00:00:00.000||||				950|
|7902|	FORD|	ANALYST|	7566|	1981-12-03 00:00:00.000|||			3000	||
|7934|	MILLER|	CLERK|	7782	|1982-01-23 00:00:00.000||		1300		|||


- IN 절에 지정한 값에 대해서만 PIVOT을 수행한다.
  - 집계 함수와 IN 절에 별칭(Alias)을 지정할 수 있다.
	- IN 절에 지정한 별칭 + 'ㅡ' + 집계 함수에 지정한 별칭으로 최종 결과 열의 Label이 지정된다.

```sql
SELECT job, D10_SUMSAL, D30_SUMSAL
	FROM (SELECT a.job
		    ,a.DEPTNO
		    ,a.SAL
		    FROM EMP a
	     ) a
	PIVOT (sum(a.SAL) AS SUMSAL
		FOR deptno 
	 	 IN (10 AS D10
	 	    ,30 AS D30
	 	    )
	      );			
```


|JOB|D10_SUMSAL|D30_SUMSAL|
|:---|---:|---:|
|CLERK|	1300|	950|
|SALESMAN|		|5600|
|MANAGER|	2450|	2850|
|ANALYST|		| |
|PRESIDENT|	5000	|


- 집계 함수와 IN 절 둘 중 하나에만 별칭을 지정하는 것도 가능하다.
  - 집계함수와 IN절 모두에 별칭을 지정하는 것이 좋다.	

- 여러 개의 집계 함수를 사용하여 PIVOT을 수행할 수 있다.
  - PIVOT에 의해 집계 함수의 개수*IN 절 값의 개수 만큼 결과 열이 출력된다

```sql
SELECT *
	FROM (SELECT a.job
		    ,a.DEPTNO
		    ,a.SAL 
		   FROM EMP a
		 ) a
	PIVOT (sum(a.SAL) AS SUMSAL
	      ,count(*) AS CNT
		FOR deptno 
	 	 IN (10 AS D10
	 	    ,20 AS D20
	 	    ,30 AS D30
	 	    )
	      );			
```


|JOB|D10_SUMSAL|D10_CNT|D20_SUMSAL|D20_CNT|D30_SUMSAL|D30_CNT|
|:---|---:|---:|---:|---:|---:|---:
|CLERK|	1300|	1|	1900|	2|	950|	1|
|SALESMAN|		|0|	|	0|	5600|	4|
|MANAGER|	2450|	1|	2975|	1|	2850|	1|
|ANALYST|		|0|	6000|	2|	|	0|
|PRESIDENT|	5000|	1||		0||		0|



- 집계함수를 2개 이상 사용하였을 때,별칭을 지정하지 않으면 문법 오류가 발생한다.
 - PIVOT 절 사용시 집계 함수와 IN 절 모두에 별칭을 지정하는 것이 좋다.


올바른 사용 예시
```sql
SELECT *
	FROM (SELECT a.job
	            ,a.deptno
	            ,a.sal 
	       FROM emp a
	     ) a
	PIVOT ( SUM(a.sal) AS SUMSAL
	       ,COUNT(*) AS CNT
	     FOR deptno
	      IN (20 AS D20
	         ,30 AS D30)
	      );
```	  


오류 발생 예시
```sql
SELECT *
	FROM (SELECT a.job
	            ,a.deptno
	            ,a.sal 
	       FROM emp a
	     ) a
	PIVOT (SUM(a.sal)
	      ,COUNT(*)
	     FOR deptno
	      IN (20 AS D20
	         ,30 AS D30)
	      );      
```
SQL Error [918] [42000]: ORA-00918: 열의 정의가 애매합니다


집계 함수를 사용하지 않으면 오류 발생
```sql
SELECT *
	FROM (SELECT a.job
	            ,a.deptno
	            ,a.sal 
	       FROM emp a
	     ) a
	PIVOT ( a.sal
	     FOR deptno
	      IN (20 AS D20
	         ,30 AS D30)
	      );  	       
```

SQL Error [56902] [99999]: ORA-56902: 피벗 작업 내에서는 합계 함수가 필요합니다.


참조하는 컬럼을 select 절에 직접 기술하면 오류 발생
```sql
SELECT job, deptno, sal, d20_sumsal, d30_sumsal
	FROM (SELECT a.job
	            ,a.deptno
	            ,a.sal 
	       FROM emp a) a
	PIVOT ( SUM(a.sal) AS SUMSAL
	     FOR deptno
	      IN (20 AS D20
	         ,30 AS D30)
	      );       
```	     
SQL Error [904] [42000]: ORA-00904: "SAL": 부적합한 식별자


##2.UNPIVOT
- FROM 절과 WHERE 절 사이에 기술한다.
  - UNPIVOT 컬럼에 UNPIVOT된 값이 출력될 컬럼을 지정한다.
  - FOR 절에는 구분자 값이 출력될 컬럼을 지정한다.
  - IN 절에는 UNPIVOT 대상 컬럼과 구분자 값을 지정

Syntax
```sql
SELECT Col1, Col2, ...
	FROM Table/Inline View
 UNPIVOT [INCLUDE | EXCLUDE NULL]
   		FOR Col1 [,Col2, ...]
		 IN (Col1 AS Alias1 [,Col2 AS Alias2,...])
		 )
   WHERE 조건;
```  
  
- 행 복제(Cross Join, 카타시안 곱)을 활용한 UNPIVOT 구현

```sql
WITH W_PIVOT AS
	(SELECT D10_SUMSAL
	       ,D20_SUMSAL
	       ,D30_SUMSAL
	   FROM (SELECT a.deptno
	   	       ,a.sal
	   		   FROM emp a
	   	) a 
	   PIVOT (SUM(a.sal) AS SUMSAL
	    	FOR deptno
	    	 IN (10 AS D10
	    	    ,20 AS D20
	    	    ,30 AS D30)
	    	 )
	)
SELECT *
    FROM W_PIVOT;
```


|10|20|30|
|---:|---:|---:|
|8,750|10,875|9,400|


- UNPIVOT 절을 기존 행수 * IN 절에 지정한 컬럼 수만큼 행을 복제한다.
  - 단, NULL 값은 결과에서 제외(EXCLUDE  NUL)된다.
  - UNPIVOT 절에 INCLUDE NULL 키워드를 기술하면 NULL 값도 결과에 포함된다.

```sql
WITH W_PIVOT AS
	(SELECT JOB, D10_SUMSAL, D20_SUMSAL, D30_SUMSAL
	   FROM (SELECT a.job
	   			   ,a.deptno
	   			   ,a.sal
	   		   FROM emp a ) a 
	   PIVOT (SUM(a.sal) AS SUMSAL
	    	FOR deptno
	    	 IN (10 AS D10
	    	    ,20 AS D20
	    	    ,30 AS D30)
	    	 )
	)  

SELECT *
 	FROM W_PIVOT
    UNPIVOT INCLUDE NULLS
   		   (SUMSAL 
   		   FOR DEPNO IN (D10_SUMSAL AS 10
   		                ,D20_SUMSAL AS 20
   		                ,D30_SUMSAL AS 30)
   		   );                     
```


|JOB|D10_SUMSAL|D20_SUMSAL|
|:---|---:|---:|
|CLERK	|10	|1300|
|CLERK	|20	|1900|
|CLERK	|30	|950|
|SALESMAN|	10||	
|SALESMAN|	20	||
|SALESMAN|	30	|5600|
|MANAGER|	10	|2450|
|MANAGER|	20	|2975|
|MANAGER|	30	|2850|
|ANALYST|	10	||
|ANALYST|	20	|6000|
|ANALYST|	30	||
|PRESIDENT|	10	|5000|
|PRESIDENT|	20	||
|PRESIDENT|	30	||



다중 컬럼 UNPIVOT 예시 
```sql
WITH W_PIVOT AS
	(SELECT JOB, D10_SUMSAL, D10_CNT, D30_SUMSAL, D30_CNT
	   FROM (SELECT a.job
	   	       ,a.deptno
	   	       ,a.sal
	   	 FROM emp a ) a 
	   PIVOT (SUM(a.sal) AS SUMSAL
	         ,COUNT(*) AS CNT
	    	FOR deptno
	    	 IN (10 AS D10
	    	    ,30 AS D30)
	    	 )
	)     		  
SELECT *
    FROM W_PIVOT;
```


|JOB|D10_SUMSAL|D10_CNT|D30_SUMSAL|D30_CNT|
|:---|---:|---:|---:|---:|
|CLERK|	1300|	1|	950|	1|
|SALESMAN|		|0|	5600|	4|
|MANAGER|	2450|	1|	2850|	1|
|ANALYST|	|	0|		|0|
|PRESIDENT|	5000|	1|	|	0|


```sql
WITH W_PIVOT AS
	(SELECT JOB, D10_SUMSAL, D10_CNT, D30_SUMSAL, D30_CNT
	   FROM (SELECT a.job
	   	       ,a.deptno
	   	       ,a.sal
	   	    FROM emp a ) a 
	   PIVOT (SUM(a.sal) AS SUMSAL
	         ,COUNT(*) AS CNT
	    	FOR deptno
	    	 IN (10 AS D10
	    	    ,30 AS D30)
	    	 )
	)     		  

SELECT *
   	FROM W_PIVOT 
   	UNPIVOT ((SUMSAL, CNT) 
   	  	FOR DEPNO
   	  	IN ((D10_SUMSAL, D10_CNT) AS 10
   	  	   ,(D30_SUMSAL, D30_CNT) AS 30)
   	  	);   	  		   
```

|JOB|DEPTNO|SUMSAL|CNT|
|:---|---:|---:|---:|
|CLERK|	10|	1300|	1|
|CLERK|	30|	950|	1|
|SALESMAN|	10|		|0|
|SALESMAN|	30|	5600|	4|
|MANAGER|	10|	2450|	1|
|MANAGER|	30|	2850|	1|
|ANALYST|	10|		|0|
|ANALYST|	30|		|0|
|PRESIDENT|	10|	5000|	1|
|PRESIDENT|	30|		|0|


- UNPIVOT 절 사용시 자주 하는 실수(데이터 타입 불일치)
  - UNPIVOT 대상 컬럼들의 데이터 타입이 일지차히 않으면 에러가 발생한다.
  	- 형 변환 함수 등을 사용하여 데이터 타입을 일치 시킨 후 UNPIVOT을 수행해야 한다.

```sql
WITH W_EMP AS
	(SELECT TO_CHAR(A.EMPNO) AS EMPNO 
	       ,A.ENAME
	       ,A.JOB
	       ,TO_CHAR(A.MGR) AS MGR
	       ,TO_CHAR(A.HIREDATE, 'YYYY-MM-DD') AS HIREDATE
	       ,TO_CHAR(A.SAL) AS SAL
	       ,TO_CHAR(A.COMM) AS COMM
	       ,TO_CHAR(A.DEPTNO) AS DEPTNO
	   FROM EMP A
	  WHERE A.EMPNO = 7369
	 ) 	

SELECT *
    FROM W_EMP; 
```	 



|EMPNO|ENAME|JOB|MGR|HIREDATE|SAL|COMM|DEPTNO|
|:---|:---|:---|:---|:---|:---|:---|:---|
|7369|	SMITH|	CLERK|	7902|	1980-12-17|	800|	|	20|



```sql	 
WITH W_EMP AS
	(SELECT TO_CHAR(A.EMPNO) AS EMPNO 
	       ,A.ENAME
	       ,A.JOB
	       ,TO_CHAR(A.MGR) AS MGR
	       ,TO_CHAR(A.HIREDATE, 'YYYY-MM-DD') AS HIREDATE
	       ,TO_CHAR(A.SAL) AS SAL
	       ,TO_CHAR(A.COMM) AS COMM
	       ,TO_CHAR(A.DEPTNO) AS DEPTNO
	   FROM EMP A
	  WHERE A.EMPNO = 7369
	 ) 		 

SELECT *
    FROM W_EMP 
    UNPIVOT (INFO_VAL
   			FOR INFO_CLS
   			 IN (EMPNO AS 'EMPNO'
   			    ,ENAME AS 'ENAME'
   			    ,JOB AS 'JOB'
   			    ,MGR AS 'MGR'
   			    ,HIREDATE AS 'HIREDATE'
   			    ,SAL AS 'SAL'
   			    ,COMM AS 'COMM'
   			    ,DEPTNO AS 'DEPNO'
   			    )
   			);
```  	 

|INFO_CLS | INFO_VAL|
|:---|:---|
|EMPNO|	7369|
|ENAME|	SMITH|
|JOB	|CLERK|
|MGR	|7902|
|HIREDATE|	1980-12-17|
|SAL	|800|
|DEPNO	|20|


고객사 요건 재구현

```sql
CREATE TABLE stage_table(
    id INT PRIMARY KEY,
    Stage VARCHAR2(20),
    Stage1_Actual DATE,
    Stage1_Plan DATE,
    Stage2_Actual DATE,
    Stage2_Plan DATE,
    Stage3_Actual DATE,
    Stage3_PLAN DATE    
);

INSERT INTO stage_table(id, stage, Stage1_Actual, Stage1_Plan, Stage2_Actual, Stage2_Plan, Stage3_Actual, Stage3_PLAN)
VALUES(1
       ,'Complete'
       , TO_DATE('2023-05-01', 'yyyy-mm-dd')
       , TO_DATE('2023-05-03', 'yyyy-mm-dd')
       ,TO_DATE('2023-05-15', 'yyyy-mm-dd')
       ,TO_DATE('2023-05-14', 'yyyy-mm-dd')
       ,TO_DATE('2023-05-20', 'yyyy-mm-dd')
       ,TO_DATE('2023-05-25', 'yyyy-mm-dd'));

INSERT INTO stage_table(id, stage, Stage1_Actual, Stage1_Plan, Stage2_Actual, Stage2_Plan, Stage3_Actual, Stage3_PLAN)
VALUES(2
       ,'PVT'
       , TO_DATE('2023-05-02', 'yyyy-mm-dd')
       , TO_DATE('2023-05-05', 'yyyy-mm-dd')
       ,TO_DATE('2023-05-10', 'yyyy-mm-dd')
       ,TO_DATE('2023-05-20', 'yyyy-mm-dd')
       ,NULL
       ,TO_DATE('2023-05-30', 'yyyy-mm-dd'));
      
SELECT *
 FROM stage_table;
```

1개 UNPIVOT 

```sql
SELECT ID, Stage, End_Actual FROM 
 (SELECT id, Stage1_Actual, Stage2_Actual FROM stage_table)
UNPIVOT( End_Actual -- unpivot_clause
         FOR Stage -- unpivot_for_clause
         IN ( -- unpivot_in_clause
             Stage1_Actual AS 'Stage1',
              Stage2_Actual AS 'Stage2'
    )
);
```

2개 UNPIVOT
```sql
SELECT ID, Stage, End_Actual, End_Plan
 FROM (SELECT id, Stage1_Actual, Stage2_Actual, Stage3_Actual, Stage1_Plan, Stage2_Plan, Stage3_Plan FROM stage_table)
UNPIVOT( (End_Actual, End_Plan) -- unpivot_clause
         FOR Stage -- unpivot_for_clause
         IN ( -- unpivot_in_clause
             (Stage1_Actual, Stage1_Plan) AS 'Stage1',
             (Stage2_Actual, Stage2_Plan) AS 'Stage2',
             (Stage3_Actual, Stage3_Plan) AS 'Stage3'
    )
);  
```

2개 UNPIVOT + UNION ALL

```sql
SELECT ID, Stage, End_Actual, End_Plan, 'Phase1' TB_NAME 
 FROM (SELECT id
             ,Stage1_Actual
             ,Stage2_Actual
             ,Stage3_Actual
             ,Stage1_Plan
             ,Stage2_Plan
             ,Stage3_Plan FROM stage_table)
UNPIVOT( (End_Actual, End_Plan) -- unpivot_clause
         FOR Stage -- unpivot_for_clause
         IN ( -- unpivot_in_clause
             (Stage1_Actual, Stage1_Plan) AS 'Stage1',
             (Stage2_Actual, Stage2_Plan) AS 'Stage2',
             (Stage3_Actual, Stage3_Plan) AS 'Stage3'
            )
        )

UNION ALL

SELECT ID, Stage, End_Actual, End_Plan, 'Phase2' TB_NAME 
 FROM (SELECT id, Stage1_Actual, Stage2_Actual, Stage3_Actual, Stage1_Plan, Stage2_Plan, Stage3_Plan FROM stage_table)
UNPIVOT( (End_Actual, End_Plan) -- unpivot_clause
         FOR Stage -- unpivot_for_clause
         IN ( -- unpivot_in_clause
             (Stage1_Actual, Stage1_Plan) AS 'Stage1',
             (Stage2_Actual, Stage2_Plan) AS 'Stage2',
             (Stage3_Actual, Stage3_Plan) AS 'Stage3'
    )
)
; 
```
