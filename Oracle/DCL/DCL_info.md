---
title: DCL
layout: default
parent: Oracle
has_children: false
nav_order: 3
---
<div style="text-align: right;">
작성일자 : 2023-07-08<br>
</div>

## DCL(Data Control Language)
{: .d-inline-block }

Ver 0.1.1
{: .label .label-green }
데이터를 관리 목적으로 보안, 무결성, 회복, 병행 제어 등을 정의하는데 사용한다.<br>DCL을 사용하면 데이터베이스에 접근하여 읽거나 쓰는 것을 제한할 수 있는 권한을 부여하거나 박탈할 수 있고 트랜잭션을 명시하거나 조작할 수 있다.

{: .highlight }
불법적인 사용자로부터 데이터를 보호하기 위한 데이터 보안의 역할을 수행하며, 데이터의 정확성을 위한 무결성을 유지하기도 한다. 마지막으로 시스템 장애에 대비한 회복과 병행수행을 제어한다.

### 1) GRANT
권한을 <span style="background-color:#FFF5b1">정의</span>할때 사용하는 명령어

### 2) REVOKE
권한을 <span style="background-color:#FFF5b1">삭제</span>하는 명령어
