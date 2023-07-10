---
title: GROUP BY
layout: default
parent: DML
grand_parent: Oracle
nav_order: 5
---
<div style="text-align: right;">
작성일자 : 2023-07-10
</div>

## <span style="background-color:#FFF5b1">GROUP BY</span>
{: .d-inline-block }

Ver 0.1.1
{: .label .label-green }

## 1. 사용법
오라클 SQL에서 GROUP BY 절을 사용하여 그룹별 건수나 합계 등의 <span style="background-color:#FAB4B4">집계 값</span>들을 얻을 수 있다. 그룹별 집계된 결과 중 원하는 조건의 결과만 필터링하기 위해서는 <span style="background-color:#FAB4B4">HAVING</span>절을 사용하여 필터 조건을 사용할 수 있다.


|집계함수|용도|
|:--:|:--:|
|max| 최대값|
|min| 최소값|
|median|중앙값|
|sum|합계|
|avg|평균|
|variance|분산|
|stddev|표준편차|
|count|관측값(row) 수|



```sql
SELECT JOB
     , COUNT(*) CNT
     , SUM(SAL) SAL
  FROM EMP
 WHERE DEPTNO IN ('10', '20', '30')
 GROUP BY JOB
HAVING COUNT(*) > 2 AND SUM(SAL) > 5000;
```


|JOB|CNT|SAL|
|:--|---:|---:|
|SALESMAN|4|5,600|
|MANAGER|3|8,275|


{: .warning}
GROUP BY에 사용되지 않은 컬럼을 조건으로 사용하면 아래의 오류가 발생한다.

```sql
SELECT deptno
      ,COUNT(*) cnt
  FROM emp
 WHERE deptno IN ('10', '20', '30')
 GROUP BY job 
HAVING (deptno = '20' AND COUNT(*) > 1) 
    OR (deptno = '30' AND COUNT(*) > 2);
```
<span style="color:red">ORA-00979: GROUP BY 표현식이 아닙니다</span>

## 3. GROUPING
괄호 안에 명시된 집계 칼럼들 별로 여러 개의 개별적인 집계 그룹을 생성

```sql
SELECT a.deptno
      ,job
      ,SUM(a.sal) AS SUM_SAL
  FROM emp a
  GROUP BY GROUPING SETS((a.deptno, a.job), a.job)
  ORDER BY a.deptno, a.job;
```


|DEPNO|JOB| SUM_SAL|
|---:|:---|---:|
|10|CLERK|	1,300|
|10|MANAGER|	2,450|
|10|PRESIDENT|	5,000|
|20|ANALYST|	6,000|
|20|CLERK|	1,900|
|20|MANAGER|	2,975|
|30|CLERK|	950|
|30|MANAGER|	2,850|
|30|SALESMAN|	5,600|
||ANALYST	|6,000|
||CLERK	|4,150|
||MANAGER	|8,275|
||PRESIDENT	|5,000|
||SALESMAN	|5,600|



```sql
SELECT a.deptno
      ,SUM(a.sal) AS SUM_SAL
  FROM emp a
  GROUP BY GROUPING SETS(a.deptno, ()) -- () : 전체를 의미
  ORDER BY a.deptno;
```


|DEPNO| SUM_SAL|
|---:|---:|
|10|	8,750|
|20|	10,875|
|30|	9,400|
|	|29,025|


```sql
SELECT a.deptno
      ,a.job
      ,SUM(a.sal) AS SUM_SAL
  FROM emp a
  GROUP BY GROUPING SETS(a.deptno, a.job, ()) -- () : 전체를 의미
  ORDER BY a.deptno, a.job;
```


|DEPNO|JOB| SUM_SAL|
|---:|:---|---:|
|10	||	8,750|
|20	||	10,875|
|30	||	9,400|
|	|ANALYST|	6,000|
|	|CLERK|	4,150|
|	|MANAGER|	8,275|
|	|PRESIDENT|	5,000|
|	|SALESMAN|	5,600|
||		|29,025|



```sql
SELECT a.deptno
      ,SUM(a.sal) AS SUM_SAL
  FROM emp a
  GROUP BY a.deptno, GROUPING SETS(a.job, ()) -- () : 전체를 의미
  ORDER BY a.deptno, a.job;
```


|DEPNO| SUM_SAL|
|---:|---:|
|10|	1,300|
|10|	2,450|
|10|	5,000|
|10|	8,750|
|20|	6,000|
|20|	1,900|
|20|	2,975|
|20|	10,875|
|30|	950|
|30|	2,850|
|30|	5,600|
|30|	9,400|


## 4. ROLLUP
집계 단위가 많아질수록 GROUPING SETS()로 하는 거보다 조회 속도 더 빠름

```sql
SELECT a.deptno
      ,a.job
      ,SUM(a.sal) AS SUM_SAL
  FROM emp a
  GROUP BY ROLLUP (a.deptno, a.job) -- N + 1 집계 그룹 결과 나옴
  ORDER BY a.deptno, a.job;
```


|DEPNO|JOB| SUM_SAL|
|---:|:---|---:|
|10|	CLERK|	1,300|
|10|	MANAGER|	2,450|
|10|	PRESIDENT|	5,000|
|10|		|8,750|
|20|	ANALYST|	6,000|
|20|	CLERK|	1,900|
|20|	MANAGER|	2,975|
|20|		|1,0875|
|30|	CLERK|	950|
|30|	MANAGER|	2,850|
|30|	SALESMAN|	5,600|
|30|		|9,400|
||		|29,025|


```sql
SELECT a.deptno
      ,a.job
      ,SUM(a.sal) AS SUM_SAL
  FROM emp a
  GROUP BY GROUPING SETS ( (a.deptno, a.job),
                            a.deptno,
                            () )  -- 위 쿼리와 같은 결과
  ORDER BY a.deptno, a.job;
```


|DEPNO|JOB| SUM_SAL|
|---:|:---|---:|
|10|	CLERK|	1,300|
|10|	MANAGER|	2,450|
|10|	PRESIDENT|	5,000|
|10|		|8,750|
|20|	ANALYST|	6,000|
|20|	CLERK|	1,900|
|20|	MANAGER|	2,975|
|20|		|10,875|
|30|	CLERK|	950|
|30|	MANAGER|	2,850|
|30|	SALESMAN|	5,600|
|30|		|9,400|
||		|29,025|


```sql
SELECT a.deptno
      ,a.job
      ,SUM(a.sal) AS SUM_SAL
  FROM emp a
  GROUP BY ROLLUP ((a.deptno, a.job))
  ORDER BY a.deptno, a.job;
```


|DEPNO|JOB| SUM_SAL|
|---:|:---|---:|
|10|	CLERK|	1,300|
|10|	MANAGER|	2,450|
|10|	PRESIDENT|	5,000|
|20|	ANALYST|	6,000|
|20|	CLERK|	1,900|
|20|	MANAGER|	2,975|
|30|	CLERK|	950|
|30|	MANAGER|	2,850|
|30|	SALESMAN|	5,600|
||		|29,025|



```sql
SELECT a.deptno
      ,a.job
      ,SUM(a.sal) AS SUM_SAL
  FROM emp a
  GROUP BY a.deptno, ROLLUP (a.job)
  ORDER BY a.deptno, a.job;
```


|DEPNO|JOB| SUM_SAL|
|---:|:---|---:|
|10|	CLERK|	1,300|
|10|	MANAGER|	2,450|
|10|	PRESIDENT|	5,000|
|10|		|8,750|
|20|	ANALYST|	6,000|
|20|	CLERK|	1,900|
|20|	MANAGER|	2,975|
|20|		|10,875|
|30|	CLERK|	950|
|30|	MANAGER|	2,850|
|30|	SALESMAN|	5,600|
|30|		|9,400|


GROUPING SETS + ROLLUP 응용
```sql
SELECT a.deptno
      ,a.job
      ,SUM(a.sal) AS SUM_SAL
  FROM emp a
  GROUP BY GROUPING SETS (a.deptno, ROLLUP(a.job))
  ORDER BY a.deptno, a.job;
```


|DEPNO|JOB| SUM_SAL|
|---:|:---|---:|
|10|		|8,750|
|20|		|10,875|
|30|		|9,400|
|	|ANALYST|	6,000|
|	|CLERK|	4,150|
|	|MANAGER|	8,275|
|	|PRESIDENT|	5,000|
|	|SALESMAN|	5,600|
|	|	|29,025|



## 5. CUBE
괄호 안의 집계 컬럼들로 만들어지는 모든 경우의 수를 집계 기준으로 하여 집계 그룹 생성 (다차원 집계)

```sql
SELECT a.deptno
      ,a.job
      ,SUM(a.sal) AS SUM_SAL
  FROM emp a
  GROUP BY CUBE (a.deptno, a.job) -- 2^n 개수로 집계됨
  ORDER BY a.deptno, a.job;
```


|DEPNO|JOB| SUM_SAL|
|---:|:---|---:|
|10|	CLERK|	1,300|
|10|	MANAGER|	2,450|
|10|	PRESIDENT|	5,000|
|10|		|8,750|
|20|	ANALYST|	6,000|
|20|	CLERK|	1,900|
|20|	MANAGER|	2,975|
|20|		|10,875|
|30|	CLERK|	9,50|
|30|	MANAGER|	2,850|
|30|	SALESMAN|	5,600|
|30|		|9,400|
|	|ANALYST|	6,000|
|	|CLERK|	4,150|
|	|MANAGER|	8,275|
|	|PRESIDENT|	5,000|
|	|SALESMAN|	5,600|
|	|	|29,025|


## 6. 확장
1. GROUP BY 절의 확장 기능과 NULL
GROUP BY 컬럼이 NOT NULL 인경우
```sql
SELECT NVL(a.job, 'Total') AS JOB 
      ,COUNT(*) AS CNT_EMP
  FROM emp a
  GROUP BY ROLLUP (a.job) 
  ORDER BY a.job;
```


|JOB| SUM_SAL|
|:---|---:|
|ANALYST|	2|
|CLERK|	4|
|MANAGER|	3|
|PRESIDENT|	1|
|SALESMAN|	4|
|Total|	14|


2. GROUP BY 컬럼이 NULLABLE
GROUPING(expr) : 집계 컬럼 값이염 0, 아니면(그룹화된 값이면) 1을 리턴

```sql
SELECT a.comm
       ,GROUPING(a.comm) AS GRP
      ,COUNT(*) AS CNT_EMP
  FROM emp a
  GROUP BY ROLLUP (a.comm) 
  ORDER BY a.comm;
```


|COMM|GRP| CNT_EMP|
|---:|---:|---:|
|0	|0|	1|
|300|	0|	1|
|500|	0|	1|
|1,400|	0|	1|
||	0	|10|
||	1	|14|


```sql
SELECT CASE WHEN GROUPING(a.comm) =1 
            THEN 'Total'
            ELSE TO_CHAR(a.comm)
       END AS COMM
      ,COUNT(*) AS CNT_EMP
  FROM emp a
  GROUP BY ROLLUP (a.comm) 
  ORDER BY a.comm;
```


|COMM| CNT_EMP|
|---:|---:|
|0	|1|
|300	|1|
|500	|1|
|1,400	|1|
||	10|
|Total|	14|



3. GROUPING_ID(expr [,edxpr, ...]) : 인자 1개 - GROUPING 함수와 동일한 결과 리턴 / 인자 N개 - 각 인자의 GROUPING 함수 리턴값을 2진수화 한 후, 그에 대한 10진수 값을 리턴

```sql
SELECT b.dname
      ,GROUPING_ID(b.dname)           AS GRP1
      ,a.comm
      ,GROUPING_ID(a.comm)           AS GRP2
      ,GROUPING_ID(b.dname, a.comm)  AS GRP_ID
      ,COUNT(*)                       AS CNT
  FROM emp a,
       dept b
 WHERE b.deptno = a.deptno
 GROUP BY CUBE (b.dname, a.comm) 
 ORDER BY b.dname, a.comm;
```


|DNAME|GRP1|COMM|GRP2|GRP_ID|CNT|
|---:|---:|---:|---:|---:|---:|
|ACCOUNTING|	0|	|	0|	0|	3|
|ACCOUNTING|	0|	|	1|	1|	3|
|RESEARCH|	0	|	|0|	0|	5|
|RESEARCH|	0	|	|1|	1|	5|
|SALES|	0	|0|	0|	0|	1|
|SALES|	0	|300	|0	|0	|1|
|SALES|	0	|500	|0	|0	|1|
|SALES|	0	|1,400	|0	|0	|1|
|SALES|	0	|	|0	|0	|2|
|SALES|	0	|	|1	|1	|6|
|	|1	|0	|0	|2	|1|
|	|1	|300|	0	|2|	1|
|	|1	|500|	0|	2|	1|
|	|1	|1,400|	0	|2	|1|
|	|1	|	|0|	2|	10|
|	|1	|	|1|	3|	14|



```sql
SELECT CASE WHEN GROUPING_ID(b.dname, a.comm) == 2
                THEN 'ALL DEPTS'
            WHEN GROUPING_ID(b.dname, a.comm) == 3
                THEN 'TOTAL'
            ELSE b.deptno
        END AS DNAME
       ,CASE WHEN GROUPING_ID(b.dname, a.comm) == 1
                THEN 'ALL DEPTS'
            WHEN GROUPING_ID(b.dname, a.comm) == 3
                THEN 'TOTAL'
            ELSE TO_CHAR(a.comm)
        END AS COMM
      ,GROUPING_ID(b.dname. a.comm)  AS GRP_ID
      ,COUNT(*)                       AS CNT
  FROM emp a,
       dept b
 WHERE b.deptno = a.deptno
 GROUP BY CUBE (b.dname, a.comm) 
 ORDER BY b.dname, a.comm;
```

HAVING 절 조건에 GROUPING_ID 함수 사용 예시 (성능 좋음)

```sql
SELECT b.dname
      ,a.comm
      ,GROUPING_ID(b.dname, a.comm)  AS GRP_ID
      ,COUNT(*)                       AS CNT
  FROM emp a,
       dept b
 WHERE b.deptno = a.deptno
 GROUP BY CUBE (b.dname, a.comm) 
    HAVING GROUPING_ID(b.dname, a.comm) IN (1,2)
 ORDER BY b.dname, a.comm;
```


|DNAME|COMM|GRP_ID|CNT|
|:---|---:|---:|---:|
|ACCOUNTING|	|	1|	3|
|RESEARCH|	|	1|	5|
|SALES|		|1	|6|
|	|0	|2	|1|
|	|300	|2|	1|
|	|500	|2|	1|
|	|1400	|2|1|
|	||	2|	10|



4.GROUP_ID() : 동일한 기준을 가진 집계 그룹에 대한 순번을 0부터 린턴 (1 이상이면 중복 집계 그룹이 존재)

```sql
SELECT b.dname
      ,a.job
      ,GROUP_ID()  AS GRP_ID
  FROM emp a,
       dept b
 WHERE b.deptno = a.deptno
 GROUP BY b.dname
         ,a.job
         ,ROLLUP(b.dname, a.comm);
```


|DNAME|JOB|CNT|
|:---|:---|---:|
|ACCOUNTING|	MANAGER|	0|
|ACCOUNTING|	PRESIDENT|	0|
|ACCOUNTING|	CLERK|	0|
|RESEARCH|	MANAGER	|0|
|RESEARCH|	ANALYST	|0|
|RESEARCH|	CLERK	|0|
|SALES	|SALESMAN	|0|
|SALES	|SALESMAN	|0|
|SALES	|SALESMAN	|0|
|SALES	|CLERK	|0|
|SALES	|MANAGER	|0|
|SALES	|SALESMAN	|0|
|ACCOUNTING|	MANAGER	|0|
|ACCOUNTING|	PRESIDENT|	0|
|ACCOUNTING|	CLERK	|0|
|RESEARCH|	MANAGER	|0|
|RESEARCH|	CLERK	|0|
|SALES	|SALESMAN	|0|
|SALES	|CLERK	|0|
|SALES	|MANAGER	|0|
|ACCOUNTING|	MANAGER	|1|
|ACCOUNTING|	PRESIDENT	|1|
|ACCOUNTING|	CLERK	|1|
|RESEARCH|	MANAGER	|1|
|RESEARCH|	ANALYST	|1|
|RESEARCH|	CLERK	|1|
|SALES	|SALESMAN	|1|
|SALES	|CLERK	|1|
|SALES	|MANAGER	|1|



```sql
SELECT b.dname
      ,a.job
      ,GROUP_ID()  AS GRP_ID
  FROM emp a,
       dept b
 WHERE b.deptno = a.deptno
 GROUP BY b.dname
         ,a.job
         ,ROLLUP(b.dname, a.comm)
  HAVING GROUP_ID() = 0; 
```


|DNAME|JOB|CNT|
|:---|:---|---:|
|ACCOUNTING|	MANAGER|	0|
|ACCOUNTING|	PRESIDENT|	0|
|ACCOUNTING|	CLERK|	0|
|RESEARCH|	MANAGER	|0|
|RESEARCH|	ANALYST	|0|
|RESEARCH|	CLERK	|0|
|SALES	|SALESMAN	|0|
|SALES	|SALESMAN	|0|
|SALES	|SALESMAN	|0|
|SALES	|CLERK	|0|
|SALES	|MANAGER	|0|
|SALES	|SALESMAN	|0|
|ACCOUNTING|	MANAGER	|0|
|ACCOUNTING|	PRESIDENT|	0|
|ACCOUNTING|	CLERK	|0|
|RESEARCH|	MANAGER	|0|
|RESEARCH|	CLERK	|0|
|SALES	|SALESMAN	|0|
|SALES	|CLERK	|0|
|SALES	|MANAGER	|0|



```sql
SELECT b.dname
      ,a.job
      ,GROUP_ID()  AS GRP_ID
  FROM emp a,
       dept b
 WHERE b.deptno = a.deptno
 GROUP BY b.dname
         ,a.job
         ,ROLLUP (a.comm); 
 ```
 위 쿼리와 같은 결과, BUT 성능은 더 좋음 (이유 : 다 만들어 놓고 HAVING 절로 거르는 거 보다 생성 자체를 적게 하는 것이 쿼리 성능에 더 좋음)


|DNAME|JOB|CNT|
|:---|:---|---:|
|ACCOUNTING|	MANAGER|	0|
|ACCOUNTING|	PRESIDENT|	0|
|ACCOUNTING|	CLERK|	0|
|RESEARCH|	MANAGER	|0|
|RESEARCH|	ANALYST	|0|
|RESEARCH|	CLERK	|0|
|SALES	|SALESMAN	|0|
|SALES	|SALESMAN	|0|
|SALES	|SALESMAN	|0|
|SALES	|CLERK	|0|
|SALES	|MANAGER	|0|
|SALES	|SALESMAN	|0|
|ACCOUNTING|	MANAGER	|0|
|ACCOUNTING|	PRESIDENT|	0|
|ACCOUNTING|	CLERK	|0|
|RESEARCH|	MANAGER	|0|
|RESEARCH|	CLERK	|0|
|SALES	|SALESMAN	|0|
|SALES	|CLERK	|0|
|SALES	|MANAGER	|0|
