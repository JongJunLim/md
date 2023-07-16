---
title: SINGLE ROW
layout: default
parent: DML
grand_parent: Oracle
nav_order: 0.5
---
<div style="text-align: right;">
작성일자 : 2023-07-16
</div>

## <span style="background-color:#FFF5b1">1. 단일행 함수(Single Row Funtion)</span>
{: .d-inline-block }

Ver 0.1.1
{: .label .label-green }

<div style="text-align: Left;">
<a href="https://docs.oracle.com/en/database/oracle/oracle-database/19/sqlrf/Single-Row-Functions.html#GUID-B93F789D-B486-49FF-B0CD-0C6181C5D85C" target="_blank"><u>Oracle - Single Row Function</u></a>
</div>

- DBMS에서 제공하는 내장 함수(Built-In Function)를 이용하면 <u>각 행 별로 데이터를 가공하거나 변환</u>할 수 있다.
- 함수는 인자 값(Arguments)과 리턴 값이 존재한다.
  - 인자 값은 함수에 따라 여러개가 될 수 있다.
  - <u>함수의 리턴 값은 항상 1개(단일 값)이다.</u>
- 단일행 함수 종류



|종류|설명|
|:---|:---|
|문자 함수|**문자 데이터**를 가공하는 단일행 함수|
|숫자 함수|**숫자 데이터**를 가공하는 단일행 함수|
|날짜 함수|**날짜 데이터**를 가공하는 단일행 함수|
|변환 함수|**데이터 유형을 변환**하는 단일행 함수|
|NULL 관련 함수|NULL과 관련된 가공 및 변환을 하는 단일 행 함수|

- 함수 중첩
  - 단일행 함수는 <u>중접하여 사용이 가능</u>하다. 즉, 함수의 인자로 다른 함수의 리턴 값이 올 수 있다.

Example
```sql
SELECT a.ENAME
      ,a.HIREDATE
      ,ADD_MONTHS(a.HIREDATE, -12*10)                                  AS USE_1FN
      ,LAST_DAY(ADD_MONTHS(a.HIREDATE,-12*10))                         AS USE_2FN
      ,TO_CHAR(LAST_DAY(ADD_MONTHS(a.HIREDATE,-12*10)), 'YYYY-MM-DD')  AS USE_3FN
	FROM emp a
WHERE a.DEPTNO = 20;
```

|ENAME|HIREDATE|USE_1FN|USE_2FN|USE_3FN|
|:---|:---|:---|:---|:---|
|SMITH|	1980-12-17 00:00:00.000|	1970-12-17 00:00:00.000|	1970-12-31 00:00:00.000|	1970-12-31|
|JONES|	1981-04-02 00:00:00.000|	1971-04-02 00:00:00.000|	1971-04-30 00:00:00.000	|1971-04-30|
|SCOTT|	1987-04-19 00:00:00.000|	1977-04-19 00:00:00.000|	1977-04-30 00:00:00.000|	1977-04-30|
|ADAMS|	1987-05-23 00:00:00.000|	1977-05-23 00:00:00.000|	1977-05-31 00:00:00.000	|1977-05-31|
|FORD|	1981-12-03 00:00:00.000|	1971-12-03 00:00:00.000|	1971-12-31 00:00:00.000	|1971-12-31|

- 데이터 유형
  - 오라클은 여러 유형의 데이터가 저장되어 있으며 <u>각 데이터 유형마다 사용할 수 있는 함수나 데이터의 가공 및 처리 방식이 다를 수 있다.</u>
    - 호환이 가능한 형태의 데이터는 변환형 함수를 이용하여 데이터 유형을 변환할 수 있다.
      - 문자 데이터
        - 문자열 정보를 가진 데이터
        - CHAR, VARCHAR2 데이터 타입
      - 숫자 데이터
        - 숫자 값 정보를 가진 데이터
        - NUMBER 데이터 타입
      - 날짜 데이터
        - 일자와 시간 정보를 가진 데이터
        - DATE, TIMESTAMP 데이터 타입


## <span style="background-color:#FFF5b1">2. 자주 쓰이는 문자 함수</span>

|함수|설명|
|:---|:---|
|LOWER(char)|char의 모든 문자를 소문자로 변경하여 리턴|
|UPPER(char)|char의 모든 문자를 대문자로 변경하여 리턴|
|CONCAT(char1, char2)|char1 문자열과 char2 뮨자열을 연결하여 리턴|
|SUBSTR(char, position [,length])|char 문자열의 position 위치로부터 lenth개의 문자를 잘라내서 리턴(length가 생략되면, position 위치로부터 char 우측 끝까지 잘라내서 리턴|
|REPLACE(char,search_string [,replace_string]| char 문자열에서 search_string 값을 replace_string 값으로 대체하여 리턴(replace_string 값이 생략되면, search_string 값을 NULL로 대체하여 리턴)|
|LPAD(exp1, n [,exp2])|expr1을 n자리만큼 늘리고, 왼쪽 빈 공간을 expr2로 채워서 리턴(expr2가 생략되면 기본 값은 공백|
|RPAD(exp1, n [,exp2])|expr1을 n자리만큼 늘리고, 오른쪽 빈 공간을 expr2로 채워서 리턴(expr2가 생략되면 기본 값은 공백|
|LTRIM(char [,set])|char 문자열의 왼쪽부터 set 문자를 제거하여 리턴 (set이 생략되면 기본 값은 공백)|
|RTRIM(char [,set])|char 문자열의 오른쪽부터 set 문자를 제거하여 리턴 (set이 생략되면 기본 값은 공백)|
|TRIM([LEADING, TRAILING, BOTH] [set] [FROM] char) |char 문자열의 왼쪽(LEADING), 오른쪽(TRAILING) 또는 양쪽(BOTH)으로부터 set 문자를 제거하여 리턴|
|LENGTH(char)|char 문자열의 길이를 리턴|
|INSTR(char, search_string [,position [,occurrence]])|char 문자열의 position 위치로부터 occurrence 번째로 찾은 search_string의 시작 위치를 리턴  (position과 occurrence) 값이 생략도면 기본 값은 1 |



```sql
SELECT LOWER('ABcdEFgh')     AS LOWER_FN
      ,UPPER('ABcdEFgh')     AS UPPER_FN
      ,CONCAT('ABC' 'def')   AS CONCAT_FN -- 두개까지만 지원 (연결 연산자 (||) 사용하면 N개 CONCAT 가능)
      ,SUBSTR('ABCDEFG',4,2) AS SUBSTR_FN1
      ,SUBSTR('ABCDEFG',4)   AS SUBSTR_FN2
      ,REPLACE('ABCDEGF' 'DEF', 'ZZZ') AS REPLACE_FN1
      ,REPLACE('ABCDEGF' 'DEF') AS REPLACE_FN2
      ,LPAD('ABC', 8, 'Z') AS LPAD_FN1
      ,LPAD('ABC', 8) AS LPAD_FN2
      ,RPAD('ABC', 8, '12') AS RPAD_FN1
      ,RPAD('ABC', 8) AS RPAD_FN2
    FROM DUAL;
```

|LOWER_FN|UPPER_FN|CONCAT_FN|SUBSTR_FN1|SUBSTR_FN2|REPLACE_FN1|REPLACE_FN2|LPAD_FN1|LPAD_FN2|RPAD_FN1|RPAD_FN2
|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|
|abcdefgh|ABDCEFGH|ABCdef|DE|EDFGH|ABCDZZZGH|ABCGH|ZZZZZABC|_____ABC|ABC12121|ABC_____| 




## <span style="background-color:#FFF5b1">3. 자주 쓰이는 숫자 함수</span>



|함수|설명|
|:---|:---|
|ABS(n)|숫자 n의 절대 값을 리턴|
|SIGN(n)|숫자 n의 부호를 리턴 (양수면 1, 음수면 -1, 0이면 0)|
|ROUND(n1 [,n2]| 숫자 n1을 소수점 n2 째 자리로 '반올림'하여 리턴(n2가 0이거나 생략되면 정수 값으로 반올림)|
|TRUNC(n1 [,n2]| 숫자 n1을 소수점 n2 째 자리로 '버림'하여 리턴(n2가 0이거나 생략되면 정수 값으로 반올림)|
|CEL(n)|숫자 n보다 크거나 같은 최소의 정수 값을 리턴|
|FLOOR(n)|숫자 n보다 작거나 같은 최대의 정수 값을 리턴|
|MOD(n1, n2)|숫자 n1을 n2로 나눈 나머지 값을 리턴|
|POWER(n1, n2)|숫자 n1의 n2 제곱 값을 리턴|
|SQRT(n1, n2)| 숫자 n의 제곱근 값을 리턴|


## <span style="background-color:#FFF5b1">4. 자주 쓰이는 날짜 함수</span>



|함수|설명|
|:---|:---|
|SYSDATE|데이터베이스 서버의 현재 날짜 값을 리턴|
|ROUND(date [,fmt])| 날짜 date를 fmt 포맷 요소 기준으로 '반올림'하여 리턴 (fmt : YY, MM, DD, HH, MI 등)|
|TRUNC(date [,fmt])| 날짜 date를 fmt 포맷 요소 기준으로 '버림'하여 리턴|
|NEXT_DAY(date, n|날짜 date 이후 가장 가까운 n요일 날짜 값을 리턴(n이 1:일, 2:월, 3:화, 4:수, 5:목, 6:금, 7:토)|
|LAST_DAY|날짜 date가 속한 월의 마지막 날짜 값을 리턴|
|ADD_MONTHS(date, n)|날짜 date에 n개월 수를 더한 날짜 값을 리턴|
|MONTHS_BETWEEN(date1, date2)|두 날짜(date1, date2)간 개월 수를 리턴|



## <span style="background-color:#FFF5b1">5. 자주 쓰이는 변환 함수</span>



|함수|설명|
|:---|:---|
|TO_CHAR(n [,fmt])|숫자 n을 주어진 fmt 형식의 문자 값으로 변환하여 리턴(999,999.99, L999,999.99, $000,000.00)|
|TO_CHAR(date [,fmt])|날짜 date를 주어진 fmt 형식의 문자 값으로 변환하여 리턴|
|TO_NUMBER(char)|문자열 char를 숫자 값으로 변환하여 리턴|
|TO_DATE(char [,fmt])|문자열 char를 주어진 fmt 형식의 날짜 값으로 변환하여 리턴|
|CAST (expr AS type_name)| expr을 type_name 데이터 타입으로 변환하여 리턴|



{: .warning }
> 연산 및 비교를 위해 <u>자동으로 데이터 타입이 변경</u>되는 것을 묵시적 형 변환이라고 한다.
>변환 함수를 사용한 **명시적 형 변환(Explicit Type Conversion)이 권장** 된다.


## <span style="background-color:#FFF5b1">6. 자주 쓰이는 NULL 관련 함수</span>



|함수|설명|
|:---|:---|
|NVL(expr1, expr2)|expr1이 NULL이 아니면 expr1, NULL이 아니면 expr2를 리턴|
|NVL(expr1, expr2, expr3)|expr1이 NULL이 아니면 expr2, NULL이 아니면 expr3를 리턴|
|COALESCE(expr1, expr2 [,expr3 ...])| NULL이 아닌 첫 번째 expr을 리턴|
|NULLIP(expr1, expr2)|expr1과 expr2가 다르면 expr1, 같으면 NULL을 리턴|


