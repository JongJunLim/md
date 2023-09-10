---
title: Optimization
layout: default
parent: Oracle
has_children: true
nav_order: 4
---
<div style="text-align: right;">
작성일자 : 2023-09-10<br>
</div>

## Optimization

<style>
  .image-container {
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>

<div class="image-container">
  <img src="https://image.yes24.com/goods/24089836/XL" width="300" height="400">
</div>

<div style="text-align: right;">
<a href="https://www.yes24.com/Product/Goods/24089836" target="_blank"><u>참고 서적 : SQL 레벨업</u></a>
</div>
<br><br>
이 페이지는 <span style="background-color:#FFF5b1">성능 좋은 SQL을 쓰는 방법, 특히 대량의 데이터를 처리하는 SQL의 성능을 향상시키는 방법</span>을 정리한 페이지입니다.
<br><br>
프로젝트를 하면서 SQL을 많이 사용하게 되었습니다. 작성일 기준으로 저의 메인 직무는 Tableau를 이용한 대시보드 개발이기에 Tableau를 잘 다루는 것이 중요합니다. <span style="background-color:#FFF5b1">하지만, 대시보드를 만들기 위해 데이터에 대한 분석과 DB를 다루는 역량 또한 매우 중요합니다.</span> 마치 요리사가 식재료에 대해서 잘 알아야하며, 요리 도구를 잘 다루어야하는 것과 동일합니다. 어떤 관점에서는 Tableau를 활용하는 능력보다도 더 중요하다고 볼 수도 있을 것 같습니다. 아무리 대시보드를 만드는 능력이 뛰어나다고 하더라도 데이터가 맞지 않다면, 그 대시보드는 의미가 없기 때문입니다.
<br><br>
처음에는 대시보드 검증을 위해서 데이터 조회의 목적으로 주로 사용했었지만, 이제는 대시보드 개발을 위해 구조를 바꾸거나 결합 및 변형 등 다양한 쿼리를 이용해 대시보드 개발을 하기도 했습니다. 이에 쿼리를 많이 작성해보게 되면서 자연스럽게 성능이 좋은 쿼리에 대해 알고싶은 마음이 커졌습니다.
<br><br>
SQL의 첫 번째 목적은 사용자가 원하는 데이터를 선택하는 것 또는 데이터를 갱신하는 것입니다. 일반적인 프로그램 언어와 마찬가지로, 한 가지 목적을 구현하는 코드는 많고 다양합니다.
<br><br>
다만 그러한 구현 방법은 기능적으로는 차이가 없어도 성능적으로는 큰 차이가 있습니다. <span style="background-color:#FFF5b1">따라서 SQL을 작성할 때는 효율과 성능을 중시해서 코드를 작성</span>해야 합니다.
<br><br>
<span style="background-color:#FFF5b1">데이터베이스 성능을 이해하려면, SQL뿐만 아니라 데이터 베이스 내부 아키텍쳐와 같이 저장소 같은 하드웨어 특성을 고려해야합니다.</span> 어떤 SQL이 빠르고 왜 그러한 결과가 나오는지, 왜 다른 SQL은 느린지를 이해하려면 블랙박스의 뚜껑을 열어 안을 들여다봐야 합니다. 성능 최적화는 뚜껑을 열고 실행 계획을 들여다봄으로써 블랙박스를 화이트박스로 만드는 것이 목적입니다.
