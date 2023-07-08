---
title: DML
layout: default
parent: Oracle
has_children: true
nav_order: 3
---
<div style="text-align: right;">
작성일자 : 2023-07-08<br>
</div>

## DDL(Data Definition Language)
{: .d-inline-block }

Ver 0.1.1
{: .label .label-green }
: 테이블과 컬럼을 정의하는 명령어로 생성,수정,삭제 등의 데이터 전체 골격을 결정하는 역할을 담당한다.


**특징**
  - DDL은 명령어를 입력하는 순간 작업이 <span style="background-color:#FFF5b1">즉시 반영(Auto Commit)</span> 되기 때문에 사용할 때 주의해야한다.

**DDL 종류**


{: .important }
> FROM ➡️ WHERE ➡️ GROUP BY ➡️ HAVING ➡️ SELECT ➡️ ORDER BY


### 1) Create
테이블을 <span style="background-color:#FFF5b1">생성</span>하는 명령어
- 객체를 의미하는 것이므로 단수형으로 이름을 짓는걸 권고한다.
- 유일한 이름으로 명명해야 한다.
- 테이블 내의 컬럼명 또한 중복되지 않는 유일한 이름으로 명명해야 한다.
- 정의할 때 각 컬럼은 ,으로 구분하며 테이블 생성문의 마지막은 ;이다.
- 컬럼명은 데이터 표준화 관점에서 일관성 있게 사용해야 한다.
- 컬럼 뒤에 데이터 유형을 반드시 지정해야 한다.
- 테이블과 컬럼명은 반드시 문자로 시작한다.
- 대소문자 구분을 하지 않지만, 기본적으로 대문자로 만들어진다.

### 2) ALTER
테이블의 <span style="background-color:#FFF5b1">구조를 수정</span>하는 명령어

- ADD COLUMN : 컬럼을 추가
- DROP COLUMN : 컬럼을 삭제
- MODIFY COLUMN : 컬럼을 수정
- RENAME COLUMN : 컬럼 이름을 변경
- DROP CONSTRAIN : 컬럼의 제약조건을 기반해서 삭제
  
### 3) DROP
테이블을 <span style="background-color:#FFF5b1">삭제</span>하는 명령어

### 4)RENAME
테이블의 span style="background-color:#FFF5b1">이름을 변경</span>하는 명령어

### 5) TRUNCATE
테이블을 <span style="background-color:#FFF5b1">초기화</span>하는 명령어

### 6) ORDER BY
SELECT절보다 나중에 실행되기 때문에 SELECT 절에서 지정한 Alias를 사용할 수 있다.
