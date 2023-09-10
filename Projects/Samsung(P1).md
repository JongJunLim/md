---
title: 02.Samsung(Phase1)
layout: default
nav_order: 2
parent : Projects
---
<div style="text-align: right;">
작성일자 : 2023-07-03
</div>

<style>
  .image-container {
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>

<div class="image-container">
  <img src="https://mms.businesswire.com/media/20230126005482/en/1605352/22/Samsung_Logo_Wordmark_2022.jpg" width="400" height="200">
</div>

## <span style="color:#2d3748; background-color:#b3e5fc">1. 요약</span>
1. 프로젝트 명칭 : 삼성 전자 DS 메모리 부분 세일즈포스 구축(Phase 1)
2. 기간 : '22.01.04 ~ '22.05.06
3. 장소 : 경기도 용인시, 테라타워
4. 고객사 : <a href="hhttps://semiconductor.samsung.com/kr/" target="_blank">삼성전자 DS 메모리사업부</a> (제조사)
5. 업무 : 대시보드 3개 개발 (C360 관련 매출 대시보드)
6. 데이터 소스 : 오라클
7. 배운점 및 느낀점<br>
   1. 대시보드 컨테이너 사용
   2. 커스텀 쿼리를 활용한 대시보드 개발
   3. Swapping 차트를 이용한 다이나믹 대시보드 개발
   4. 복잡한 계산식 및 숨기기, 참조선을 활용한 대시보드 개발
   5. 대시보드 퍼포먼스 향상 방법
   6. 세일즈포스 공부 필요성
   7. 대시보드 임베딩 매개변수

## <span style="color:#2d3748; background-color:#b3e5fc">2. 회고록</span>
23년의 시작과 동시에 나의 커리어 첫 프로젝트가 시작되었다. 첫 프로젝트의 장소는 삼성전자 DS였다. 삼성전자는 회사의 주요 고객사 중 하나였다. 해당 프로젝트는 21년 하반기부터 시작되었던 프로젝트였는데, 인원 교체를 해야할 필요가 있어서 내가 교체 인원으로 투입되게 되었다.

프로젝트 장소는 경기도 용인에 위치해있었다. 기흥과 화성에 삼성전자의 캠퍼스들이 있었고, 캠퍼스 근처에 협력사가 일을 할수 있는 건물이 있었다.
삼성은 8시부터 5시까지 근무였고, 근무지가 멀리 있는 만큼 통근버스를 타고 다녀야했다. 이에 오전 5시 반에 일어나서 준비해 통근버스를 타고 다니곤 했었다.

해당 프로젝트는 세일즈포스 구축 프로젝트로 태블로 대시보드도 동시에 개발하여 세일즈포스에 임베딩하여 사용하고 있었다. 10여명이 넘는 세일즈포스 직원들이 있었고, 태블로 개발자는 나를 포함한 동료직원 1명으로 총 2명이었다.

내가 담당하게 될 대시보드는 C360(Customer 360)의 Home 화면에 임베딩될 화면 1개와, Sales Summary, 그리고 Account의 Sales Summary 이렇게 총 3개였다.

고객의 요건을 충족시키기 위해서 **커스텀 쿼리**를 써야할 필요성이 있었다. 사실 그 당시에는 부끄럽지만 복잡한 쿼리문에 대해서 잘 알지 못했다. 직접 쿼리를 작성하지는 못했지만, 앞으로의 미래를 위해서 쿼리 공부를 열심히 해야할 것이라고 결심하기도 했었다. (자격증이 실력을 보증한다고 할 수는 없지만 꾸준히 공부한 끝에 프로젝트 종료 후 6월에 **SQLD 자격증을 취득**하기도 했었다.)

실제 고객의 데이터를 가지고 요건 분석과 데이터, 대시보드 분석 및 개발을 하면서 역량이 많이 향상 되었다. 오토닉스 POC 당시에는 적용하지 못했던 **컨테이너를 활용**하였고, **매개변수 동작을 활용한 다수개의 Swapping 차트로 다이나믹 대시보드**를 개발했다. **복잡한 계산식**과 그동안 사용하지 못했던 **숨기기 기능**과 **참조선**을 활용한 대시보드로 고객 요건을 충족시키면서 개발에 있어서 새로운 방법을 배우기도 했다.

대시보드 개발도 중요한 업무였지만, **대시보드 퍼포먼스 향상**도 중요한 업무이기도 했다. 태블로 서버에 있는 기능을 활용해 대시보드 퍼포먼스에 대해서 이상점(Outlier)을 발견하고, 개선시키기 위해 다방면으로 분석했다. 계약상 나는 롤아웃을 하게되어 실제로 대시보드를 개선하는 단계를 직접 경험하지는 못했지만, 개선시키기 위한 사전 단계에 대해서 경험을 하고 나온것 만으로도 값진 경험이었다.

세일즈포스 프로젝트였으므로 개발한 대시보드는 세일즈포스 컴포넌트에 임베딩되어 사용된다. 세일즈포스의 세계에 대해서도 잘 몰랐던 나였고, 해당 프로젝트를 하면서 세일즈포스를 조금이나마 체감할 수 있었다. 그 과정에서 **세일즈포스의 환경과 임베딩 방식**에 대해서 알 수 있었다.

이 프로젝트는 나의 첫 프로젝트로 참 좋았던 프로젝트라고 생각한다. 다양한 요건을 접하면서 대시보드 개발 능력 향상도 되었고, 세일즈포스 환경도 접해볼수 있었다. 세계에서 손꼽히는 기업 중 선진 기업인 삼성전자에서 프로젝트를 하는 것도 개인적으로 큰 동기부여가 되기도 했다. 시작이 좋았던 나의 첫 프로젝트였다.

---
## <span style="color:#2d3748; background-color:#b3e5fc">1. Summary</span>

1. Name : Samsung Electronics DS Memory Division SFDC (Salesforce) Establishment
2. Period : '22.01.04 ~ '22.05.06
3. Location : Terra Tower, Yongin
4. Customer: <a href="https://semiconductor.samsung.com/" target="_blank">Samsung Electronics Memory Division</a> (manufacturer)
5. Task : Developed 3 dashboards (sales dashboard related with C360)
6. Data Source : Oracle
7. What I learned and felt<br>
   1. Need to study how to use dashboard containers<br>
   2. Date functions (Datetrunc, Dateparse, etc.) and how to utilize parameters
   3. Development of Dynamic Dashboard Using Swapping Chart
   4. Complex calculation formula and hiding, dashboard development using reference lines
   5. How to improve dashboard performance
   6. Need to study Salesforce
   7. Dashboard embedding parameters

## <span style="color:#2d3748; background-color:#b3e5fc">2. Memoirs</span>
At the beginning of 23 years, my first project in my career began. The site of the first project was Samsung Electronics DS. Samsung Electronics was one of the company's major clients. This project was a project that started in the third quarter of 2021, but I was put in as a replacement because it needed to be replaced.

The project site was located in Yongin, Gyeonggi-do. There were Samsung Electronics' campuses in Giheung and Hwaseong, and there was a building near the campus where partners could work. Samsung worked from 8 a.m. to 5 p.m., and I had to take a commuter bus as the workplace was far away. Therefore, I used to get up at 5:30 a.m. to get ready and take a commuter bus.

The project was a salesforce Establishment project, and tableau dashboards were also developed and embedded in the salesforce. There were more than 10 Salesforce employees, and the Tableau developer was one fellow employee, including me, with a total of two.

The dashboards I will be in charge of were one screen to be embedded in the Home screen of the C360 (Customer 360), Sales Summary, and Account's Sales Summary.

There was a need to write a **custom query** to meet the customer's requirements. In fact, I didn't know much about complicated query statements at the time. I couldn't write my own queries, but I also decided that I would have to study them hard for the future. (The certificate cannot be said to guarantee skills, but after studying steadily, I also obtained a **SQLD certificate** in June after the project was completed.)

Capabilities have improved a lot by analyzing requirements, data, dashboard analysis, and development with actual customer data. **Containers that could not be applied at the time of Autonics POC was used**, and a **dynamic dashboard was developed with a number of swapping charts using parameter actions.** I also learned new ways in development while meeting customer requirements with **complex calculations, hidden functions that have not been used so far, and dashboards using reference lines.**

Dashboard development was also an important task, but **dashboard performance improvement** was also an important task. Using the functions in the tableau server, we discovered outliers in dashboard performance and analyzed them in various ways to improve them. I didn't actually experience the steps of improving the dashboard in person due to my contractual rollout, but it was a valuable experience just to come out with the experience of the preliminary steps to improve.

Since it was a Salesforce project, the developed dashboard is embedded in the Salesforce component. I didn't know much about the Salesforce, and I was able to feel a little bit of Salesforce while working on the project. In the process, **I was able to learn about the environment and embedding method of Salesforce.**

I think this project was very good as my first project. As I encountered various requirements, my dashboard development ability was improved, and I was able to experience the sales force environment. Working on a project at Samsung Electronics, one of the world's leading companies, was also a great motivation. It was my first project with a good start.
