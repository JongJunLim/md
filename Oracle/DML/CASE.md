---
title: CASE
layout: default
parent: DML
grand_parent: Oracle
nav_order: 8
---
<div style="text-align: right;">
작성일자 : 2023-07-23
</div>

## <span style="background-color:#FFF5b1">CASE </span>
{: .d-inline-block }

Ver 0.1.1
{: .label .label-green }

- [1.CASE](#1CASE)<br>
    - [1-1.단순 CASE 표현](#1-1단순-case-표현)<br>
    - [1-2.검색 CASE 표현](#1-2검색-case-표현)<br>
    - [1-3.중첩 CASE 표현](#1-3중첩-case-표현)<br>
    - [1-4.WHERE절 CASE 표현](#1-4where절-case-표현)<br>
    - [1-5.ORDER BY절 CASE 표현](#1-5order-by절-case-표현)<br>
- [2.DECODE (Oracle)](#2DECODE-(Oracle))



## 1.CASE
- 비교 값 또는 조건에 따라 데이터를 가공하거나 변환할 수 있다.
- 프로그래밍 언어의 조건문(IF - ELSEIF - ELSE)과 유사한 처리가 가능하다
 
### 1-1.단순 CASE 표현
```sql
 SELECT A.ENAME
       ,A.JOB 
       ,CASE A.JOB WHEN 'PRESIDENT' THEN 'P'
                   WHEN 'MANAGE' THEN 'M'
                   ELSE 'E'
        END AS JOB_SE
	FROM EMP A
   WHERE A.SAL BETWEEN 1500 AND 5000
     AND A.DEPTNO IN (10,20);
```



|ENAME|JOB|JOB_SE|
|:---|:---|:---|
|JONES|	MANAGER|	E|
|CLARK|	MANAGER|	E|
|SCOTT|	ANALYST|	E|
|KING|	PRESIDENT|	P|
|FORD|	ANALYST|	E|



### 1-2.검색 CASE 표현 
```sql
 SELECT A.ENAME
       ,A.JOB 
       ,CASE A.JOB WHEN 'PRESIDENT' THEN 'P'
                   WHEN 'MANAGE' THEN 'M'
                   ELSE 'E'
        END AS JOB_SE
	FROM EMP A
   WHERE A.SAL BETWEEN 1500 AND 5000
     AND A.DEPTNO IN (10,20);
```



|ENAME|JOB|JOB_SE|
|:---|:---|:---|
|JONES|	MANAGER|	E|
|CLARK|	MANAGER|	E|
|SCOTT|	ANALYST|	E|
|KING|	PRESIDENT|	P|
|FORD|	ANALYST|	E|


- 단순 CASE 표현보다 업무에서 더 많이 사용됨
    - 이유
      - 단순 CASE는 CASE 뒤에 오는 필드만 정의 가능
      - 검색 CASE는 여러 조건을 사용할 수 있음



```sql
SELECT A.COMM
      ,A.ENAME
      ,A.SAL
      ,CASE WHEN A.COMM IS NOT NULL THEN 1
            WHEN A.ENAME LIKE '%E%' THEN 2
            WHEN A.SAL < 2000 OR A.SAL > 3000 THEN 3
       END AS EMP_CLS_CD
	FROM EMP A
   WHERE A.SAL BETWEEN 1500 AND 5000
     AND A.DEPTNO IN (10,30);
```


|COMM|ENAME|SAL|EMP_CLS_CD|
|---:|:---|---:|---:|
|300|	ALLEN|	1600|	1|
|	|BLAKE|	2850|	2|
|	|CLARK|	2450|	|
|	|KING|	5000|	3|
|0	|TURNER|	1500|	1|



### 1-3.중첩 CASE 표현 
```sql
    
 SELECT A.ENAME
       ,A.JOB 
       ,A.COMM
       ,CASE WHEN A.JOB = 'SALESMAN'
             	THEN (CASE WHEN A.SAL + A.COMM > 2500 THEN 'A'
             		   WHEN A.SAL + A.COMM BETWEEN 1500 AND 2500 THEN 'B'
             	           ELSE 'C'
             	      END
             	     )
             ELSE    (CASE WHEN A.SAL > 3000 THEN 'A'
                           WHEN A.SAL BETWEEN 2000 AND 3000 THEN 'B'
                           ELSE 'C'
                      END
                      )
         END AS SAL_GRADE                       
	FROM EMP A
   WHERE A.DEPTNO IN (10,30);  
```



|ENAME|JOB|SAL|COMM|SAL_GRADE|
|:---|:---|---:|---:|:---|
|WARD|	SALESMAN|	1250|	500|	B|
|MARTIN|	SALESMAN|	1250|	1400|	A|
|ALLEN|	SALESMAN|	1600|	300|	B|
|TURNER|	SALESMAN|	1500|	0|	B|
|KING|	PRESIDENT|	5000|		|A|
|BLAKE|	MANAGER|	2850|		|B|
|CLARK|	MANAGER|	2450|		|B|
|JAMES|	CLERK|	950|		|C|
|MILLER|	CLERK|	1300|		|C|



### 1-4.WHERE절 CASE 표현
```sql
SELECT A.ENAME
       ,A.JOB
       ,A.SAL
       ,A.COMM
	FROM EMP A
   WHERE CASE WHEN A.COMM IS NOT NULL
              	THEN A.SAL + A.COMM
              ELSE A.SAL
         END > 2500;    
```


|ENAME|JOB|SAL|COMM|
|:---|:---|---:|---:|
|JONES|	MANAGER|	2975|	|
|MARTIN|	SALESMAN|	1250|	1400|
|BLAKE|	MANAGER|	2850|	|
|SCOTT|	ANALYST|	3000|	|
|KING|	PRESIDENT|	5000|	|
|FORD|	ANALYST|	3000|	|



- 위 식과 같은 결과
```sql
SELECT A.ENAME
       ,A.JOB
       ,A.SAL
       ,A.COMM
	FROM EMP A
   WHERE A.SAL + NVL(A.COMM,0) > 2500;    
```


### 1-5.ORDER BY절 CASE 표현
```sql
SELECT A.ENAME
       ,A.JOB
       ,A.DEPTNO
       ,A.COMM
	FROM EMP A
   WHERE A.SAL >= 1500
ORDER BY CASE WHEN A.JOB IN ('PRESIDENT', 'MANAGER') THEN 1
              ELSE 2
         END
        ,A.DEPTNO DESC
        ,A.SAL DESC;    
```


|ENAME|JOB|DEPTNO|SAL|
|:---|:---|---:|---:|
|BLAKE|	MANAGER|	30|	2850|
|JONES|	MANAGER|	20|	2975|
|KING|	PRESIDENT|	10|	5000|
|CLARK|	MANAGER|	10|	2450|
|ALLEN|	SALESMAN|	30|	1600|
|TURNER|	SALESMAN|	30|	1500|
|SCOTT|	ANALYST|	20|	3000|
|FORD|	ANALYST|	20|	3000|	


## 2.DECODE (Oracle)
```sql
DECODE(expr, search1, result1 [,search2, result2 ...] (default])
``` 
- expr이 search1과 일치하면 result1, search2와 일치하면 result2, 모두 일치하지 않으면 default를 리턴(단, 모두 일치 하지 않는데 default를 지정하지 않았으면 NULL을 리턴)    

```sql
SELECT A.ENAME
       ,A.JOB 
       ,DECODE(A.JOB,'PRESIDENT','P','MANAGE','M','E') AS JOB_SE
	FROM EMP A
   WHERE A.SAL BETWEEN 1500 AND 5000
     AND A.DEPTNO IN (10,20);
```


|ENAME|JOB|JOB_SE|
|:---|:---|:---|
|JONES|	MANAGER|	E|
|CLARK|	MANAGER|	E|
|SCOTT|	ANALYST|	E|
|KING|	PRESIDENT|	P|
|FORD|	ANALYST|	E|
