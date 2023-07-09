---
title: DATE
layout: default
parent: DML
grand_parent: Oracle
nav_order: 1
---
<div style="text-align: right;">
작성일자 : 2023-07-04
</div>

## <span style="background-color:#FFF5b1">1. 날짜 함수(Date Funtion)</span>
{: .d-inline-block }

Ver 0.1.1
{: .label .label-green }

쿼리문을 작성함에 있어서 날짜를 잘 다루는 것은 <span style="background-color:#FAB4B4">기본이자 필수</span>이다. 나 같은 경우는 실제로 프로젝트 당시 태블로로 대시보드를 만든 후 원본 데이터로 검증을 할 때 WHERE 절에 년도 또는 월 등 기간 조건을 적용시켜 데이터를 검증을 많이 하곤 한다. 이럴 때 날짜 함수들을 많이 사용하였다. 뿐만 아니라 상황에 따라 날짜들 끼리 연산을 해야하는 경우도 있을 것이다.

지금부터 날짜 정보 추출하는 방법과 날짜 형식, 그리고 날짜 및 시간 연산에 대해서 정리해보도록 하겠다.

When writing a query statement, it is both basic and essential to handle the date well. In my case, when I actually make a dashboard with a tablo and then verify it with the original data, I often verify the data by applying term conditions such as year or month to the WHERE clause. At this time, many date functions were used. In addition, there will be cases where it is necessary to perform a calculation between dates depending on the situation.

From now on, I will summarize the method of extracting date information, date format, and date and time operations.

## <span style="background-color:#FFF5b1">2. 날짜 정보 추출 (Extract Date Information)</span>
EXTRACT 함수는 오라클에서 날짜 유형의 데이터로부터 날짜 정보를 분리하여 새로운 컬럼의 형태로 추출해주는 함수이다.

The EXTRACT function is a function that separates date information from date-type data in Oracle and extracts it in the form of a new column.

```sql
select systimestamp
      ,EXTRACT (year from systimestamp) as year
      ,EXTRACT (month from systimestamp) as month
      ,EXTRACT (day from systimestamp) as day
      ,EXTRACT (hour from systimestamp) as hour
      ,EXTRACT (minute from systimestamp) as minute
      ,EXTRACT (second from systimestamp) as second
from DUAL;
```


| SYSTIMESTAMP | YEAR | MONTH | DAY | HOUR | MINUTE | SECOND |
|:------------------------------:|---:|----:|--:|---:|-----:|-----:|
|2023-07-04 10:31:25.610 +000|2,023|7|4|10|31|25.610123|


기본적으로 날짜 속성을 가지고 있으며, 날짜에서 추출된 정보(년, 월,일,시,분)들은 숫자 속성을 가지고 있다. 

Basically, it has a date attribute, and information extracted from the date (year, month, day, hour, minute) has a numeric attribute.

## <span style="background-color:#FFF5b1">3. 날짜 형식 (Date Format)</span>
아래와 같이 다양한 인자를 통해서 날짜를 다양한 형태로 조회 할 수 있다.

The date can be inquired in various forms through various factors as follows.

```sql
select TO_CHAR(sysdate, 'CC') as "세기"
      ,TO_CHAR(sysdate, 'YYYY') as "년"
      ,TO_CHAR(sysdate, 'MM') as "월"
      ,TO_CHAR(sysdate, 'MON') as "월_약자"
      ,TO_CHAR(sysdate, 'MONTH') as "월_이름"
      ,TO_CHAR(sysdate, 'DD') as "일"
      ,TO_CHAR(sysdate, 'DDD') as "몇일째"
      ,TO_CHAR(sysdate, 'DY') as "요일"
      ,TO_CHAR(sysdate, 'DAY') as "요일_이름"
      ,TO_CHAR(sysdate, 'W') as "몇번째주"
    from DUAL;
```



|세기 |년   |월|월_약자|월_이름|일|몇일째|요일|요일_이름|몇번째주| 
|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---| 
|21|2023|07|7월|7월|04|185|화|화요일|1| 



아래와 같이 언어 설정을 통해 다양한 언어형태로 날짜형태를 표현 할 수도 있다.

The date form may be expressed in various language forms through the language setting as follows.
```sql
select sysdate
      ,TO_CHAR(sysdate, 'DY', 'NLS_DATE_LANGUAGE=KOREAN') as KOR
      ,TO_CHAR(sysdate, 'DY', 'NLS_DATE_LANGUAGE=JAPANESE') as JAP
      ,TO_CHAR(sysdate, 'DY', 'NLS_DATE_LANGUAGE=ENGLISH') as ENG
      ,TO_CHAR(sysdate, 'DAY', 'NLS_DATE_LANGUAGE=KOREAN') as KOR
      ,TO_CHAR(sysdate, 'DAY', 'NLS_DATE_LANGUAGE=JAPANESE') as JAP
      ,TO_CHAR(sysdate, 'DAY', 'NLS_DATE_LANGUAGE=ENGLISH') as ENG
    from DUAL;
```


|SYSDATE|KOR|JAP|ENG|KOR|JAP|ENG|
|:---|:---|:---|:---|:---|:---|:---|
|2023-07-04 10:39:17.000|화|火|TUE|화요일|火曜日|TUESDAY|


## <span style="background-color:#FFF5b1">4. 날짜 및 시간 연산 (Date and Time Calculations)</span>
아래 함수들은 다양한 형태로 날짜 및 시간을 연산하는 함수들이다. 이를 익혀두고 잘 활용하면 날짜를 다루는 것이 비교적 쉬워질 것이라 생각한다. 

The following functions are functions that calculate date and time in various forms. I think it will be relatively easy to handle the date if I learn it and use it well.

```sql
SELECT TO_CHAR(SYSDATE ,'yyyy/mm/dd') AS "오늘 날짜" 
      ,TO_CHAR(SYSDATE + 1 ,'yyyy/mm/dd') AS "내일 날짜"  
      ,TO_CHAR(SYSDATE -1 ,'yyyy/mm/dd') AS "어제 날짜" 
      ,TO_CHAR(TRUNC(SYSDATE,'dd') ,'yyyy/mm/dd hh24:mi:ss') AS "오늘 정각 날짜"
      ,TO_CHAR(TRUNC(SYSDATE,'dd') + 1,'yyyy/mm/dd hh24:mi:ss') AS "내일 정각 날짜"
      ,TO_CHAR(SYSDATE + 1/24/60/60 ,'yyyy/mm/dd hh24:mi:ss') AS "1초 뒤 시간"
      ,TO_CHAR(SYSDATE + 1/24/60 ,'yyyy/mm/dd hh24:mi:ss') AS "1분 뒤 시간"
      ,TO_CHAR(SYSDATE + 1/24 ,'yyyy/mm/dd hh24:mi:ss') AS  "1일 뒤 시간"
      ,TO_CHAR(TRUNC(SYSDATE,'mm') ,'yyyy/mm/dd') AS "이번 달 시작날짜"
      ,TO_CHAR(LAST_DAY(SYSDATE) ,'yyyy/mm/dd') AS "이번 달 마지막 날"
      ,TO_CHAR(NEXT_DAY(SYSDATE,'일요일')) AS "다음 일요일"
      ,TO_CHAR(trunc(ADD_MONTHS(SYSDATE, + 1),'mm') ,'yyyy/mm/dd') AS "다음 달 시작날짜"
      ,TO_CHAR(ADD_MONTHS(SYSDATE, 1) ,'yyyy/mm/dd hh24:mi:ss') AS "다음달 오늘 날자"
      ,TO_CHAR(TRUNC(SYSDATE, 'yyyy') ,'yyyy/mm/dd') AS "올해 시작 일"
      ,TO_CHAR(TRUNC(ADD_MONTHS(SYSDATE, -12), 'dd'),'yyyy/mm/dd') AS "작년 현재 일"
      ,TO_DATE(TO_CHAR(SYSDATE, 'YYYYMMDD')) - TO_DATE('19930315') AS "두 날짜 사이 일수 계산"
      ,MONTHS_BETWEEN(SYSDATE, '19930315') AS "두 날짜 사이의 월수 계산"
      ,TRUNC(MONTHS_BETWEEN(SYSDATE, '19930315')/12,0) AS "두 날짜 사이의 년수 계산"
	FROM DUAL; 
```


|오늘 날짜|내일 날짜|어제 날짜|오늘 정각 날짜|내일 정각 날짜|1초뒤 시간|1분 뒤 시간|1일 뒤 시간|이번 달 시작 날짜|이번 달 마지막 날|다음 일요일|다음 달 시작 날짜|다음달 오늘 날짜| 올해 시작 일| 작년 현재일| 두 날짜 사이 일수|두 날짜 사이의 월수|두 날짜 사이의 년수|
|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|-------------:|-------------:|-------------:|
|2023/07/04|2023/07/05|2023/07/03|2023/07/04 00:00:00|2023/07/05 00:00:00|2023/07/04 10:51:28|2023/07/04 10:52:27|2023/07/04 11:51:27|2023/07/01|2023/07/31|23/07/09|2323/08/01|2023/08/04 10:51:27|2023/01/01|2022/07/04|11,068|363.65|30|


보통은 위와 같이 시간을 연산하지만, 아래와 같은 형태로도 시간을 연산 할 수도 있다. 이런 방법으로도 시간을 연산할수 있다는 것을 알아둔다면 좋을 것 같다. 

Usually, time is calculated as above, but time may also be calculated in the following form. It would be nice to know that time can be calculated in this way as well.

```sql
WITH temptable AS (
    SELECT TO_DATE('2019-08-21 08:00:00', 'yyyy-mm-dd hh24:mi:ss') CURTIME 
      FROM DUAL
)

SELECT CURTIME
      ,CURTIME + 5/24         hour --5시간 더하기 
      ,CURTIME + 5/(24*60)    min  --5분 더하기
      ,CURTIME + 5/(24*60*60) sec  --5초 더하기
  FROM temptable;
```


|CURTIME|HOUR|MIN|SEC|
|:---|:---|:---|:---|
|2019-08-21 08:00:00.000|2019-08-21 13:00:00.000|2019-08-21 08:05:00.000|2019-08-21 08:00:05.000|


``` sql
--INTERVAL 활용
WITH temptable AS (
    SELECT TO_DATE('2019-08-21 08:00:00', 'yyyy-mm-dd hh24:mi:ss') CURTIME 
      FROM DUAL
)

SELECT CURTIME
      ,CURTIME + (INTERVAL '5' hour)   hour2 --5시간 더하기 
      ,CURTIME + (INTERVAL '5' minute) min2  --5분 더하기
      ,CURTIME + (INTERVAL '5' second) sec2  --5초 더하기
  FROM temptable;
 ```

|CURTIME|HOUR2|MIN2|SEC2|
|:---|:---|:---|:---|
|2019-08-21 08:00:00.000|2019-08-21 13:00:00.000|2019-08-21 08:05:00.000|2019-08-21 08:00:05.000|
