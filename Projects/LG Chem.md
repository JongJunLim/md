---
title: 03.LG Chemical
layout: default
nav_order: 3
parent : Projects
---
<div style="text-align: right;">
작성일자 : 2023-07-11
</div>

<style>
  .image-container {
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>

<div class="image-container">
  <img src="https://res.cloudinary.com/crunchbase-production/image/upload/c_lpad,f_auto,q_auto:eco,dpr_1/v1397193787/fede4d9e6c283b958fa525557d7d80e4.jpg" width="500" height="150">
</div>


## <span style="color:#2d3748; background-color:#FFAAAA">1. 요약</span>
1. 프로젝트 명칭 : LG 화학 세일즈 포스 & 태블로 구축
2. 기간 : '22.07.01 ~ '23.02.28 (8개월)
3. 장소 : 여의도
4. 고객사 : <a href="https://www.lgchem.com/main/index?lang=ko_KR" target="_blank">LG 화학</a> (화학사)
5. 업무
  - 태블로 서버 설치 (Window 2016 Server) 
 - 대시보드 20개 개발 (매출 진척, 판매법인, C&C (Claim&Complain), 고객 인사이트, 사업부 특화)
 - 현업 및 수행사(PWC) 대상 태블로 사용 교육
6. 데이터 소스 : Salesforce
7. <span style="color:#2d3748; background-color:#FFAAAA">배운점 및 느낀점</span>
   1. Salesforce Sales Cloud 경험
        - Salesforce Sales Cloud를 직접 경험해보면서 CRM의 전반적인 과정을 간접 체험
   2. 커뮤니케이션 능력 향상
        - 수행사와의 요건 조율 및 프로젝트 팀원간 협력을 통해 커뮤니케이션 능력 향상  
   3. Salesforce objects를 데이터 원본으로 태블로 대시보드 개발
        - 별도의 DM없이 직접 Salesforce를 연결하여 대시보드 개발
        - 제한 사항
          - 추출의 최소 주기 1시간
          - SOQL 사용시 증분 추출 사용 불가(필드 숨기기 사용)
          - Union 불가능
          - Object가 Blending의 보조 데이터 원본으로 사용 불가
          - Fomula Field 사용 불가
   4. SOQL(Salesforce Objects Query Language)
        - 일반 SQL과 대부분은 비슷하지만 Salesforce만의 지원 함수들이 있음
   5. 태블로 계산식을 활용한 대시보드 권한 관리
        - 태블로에서는 ismemberof() 함수를 지원하지만, 대규모 조직에서는 계산식에 대한 유지보수에 어려움이 있음
        - 조직 정보 테이블이 있기 때문에 username() 함수와 조직 정보를 활용해 계산식으로 데이터에 대한 권한 관리 실행
   6. 다양한 데이터 모델링(셀프 조인, 관계, 블렌딩)을 활용한 대시보드 개발
   7. 대시보드 URL 매개변수 컨트롤
        - Embedding시 URL 매개변수를 통해 데이터를 필터링 또는 대시보드의 옵션을 컨트롤 할 수 있음
   8. LOD 함수, 날짜, 테이블 계산 등 다양하고 복잡한 계산식 활용 및 다양한 형태의 다이나믹 대시보드 개발
        - 요건 충족을 위해 LOD 함수, 날짜, 테이블 계산의 고급 계산식을 사용
        - 매개변수 동작, Radar 차트, Scatter Plot, Table 등 새로운 형태의 차트 개발과 기본적인 차트와 계산식을 응용한 다이나믹 대시보드 개발

---

## <span style="color:#2d3748; background-color:#FFAAAA">1. Summary</span>

1. Name : LG Chemical Salesforce & Tableau Establishment
2. Period : '22.07.01 ~ '23.02.28 (8 Months)
3. Location : Yeoui-do, Seoul
4. Customer: <a href="https://www.lgchem.com/main/index" target="_blank">LG Chem</a> (Chemical industry)
5. Task
   - Installed Tableau Server (Window 2016 Server) 
   - Developed 20 dashboards (sales progress, sales corporation, C&C (Claim & Complain), customer insight, division attribute)
   - Conducted Tableau training sessions for business users and consultants from the field and PWC (Price Waterhouse Coopers).
6. Data Source : Salesforce
7. <span style="color:#2d3748; background-color:#FFAAAA">What I learned and felt</span>
   1. Experienced Salesforce Sales Cloud
      - Experience the overall process of CRM with indirenct experience of Salesforce Sales Cloud
   2. Enhanced communication skills
      - Improve communication skills through coordination of requirements with performers and collaboration between project team members 
   3. Developed tableau dashboards with Salesforce Objects as a data source
      - Developed dashboards by connecting Salesforce directly without a separate DM
      - Restrictions
        - Minimum cycle of extraction 1 hour
        - Incremental extraction disabled when using SOQL (using field hiding)
        - Union impossible
        - Object not available as secondary data source for Blending
        - Formula Field unavailable 
   4. SOQL(Salesforce Objects Query Language)
       - Most of them are similar to regular SQL, but have support functions unique to Salesforce 
   5. Managed dashboard and data permisiions using calculated fields
       - Tableau supports ismemberof() function, but large organizations have difficulty maintaining computational expressions
       - Because there is an organizational information table, use the username() function and organizational information to perform permission management on the data in a computational manner
   6. Developed of dashboars using Data modeling(Self-Join, Relation, and Blending)
   7. Dashboard URL parameter Control
       - When Embedded, URL parameters allow you to filter data or control options on the dashboard 
   8. Utilize various and complex calculated fields like LOD, Dates etc
       - Use advanced calculation expressions of LOD functions, dates, and table calculations to meet requirements
       - Development of new types of charts such as parameter behavior, Radar chart, Scatter Plot, Table, etc., and Dynamic Dashboard using basic charts and calculation formulas
