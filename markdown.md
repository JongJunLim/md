---
title: Markdown Syntax
layout: default
parent: Markdown
nav_order: 1
---
<div style="text-align: right;">
작성일자 : 2023-06-19<br>
수정일자 : 2023-09-16
</div>

## <span style="background-color:#FFF5b1">Markdown Syntax</span>
{: .d-inline-block }

Ver 0.1.5
{: .label .label-green }
 

- [1. 제목](#chapter-1)<br>
- [2. 구분선 및 줄바꿈](#chapter-2)<br>
- [3. 글자 속성](#chapter-3)<br>
  - [3.1 강조](#chapter-3.1)<br>
  - [3.2 색상](#chapter-3.2)<br>
- [4. 인용문구](#chapter-4)<br>
- [5. 들여쓰기](#chapter-5)<br>
- [6. 이미지](#chapter-6)<br>
- [7. 외부 링크](#chapter-7)<br>
- [8. 내부 링크](#chapter-8)<br>
- [9. 코드 블럭](#chapter-9)<br>
- [10. 체크리스트](#chapter-10)<br>
- [11. 표](#chapter-11)<br>
- [12. 토글버튼](#chapter-12)<br>


---
> 마크다운(Markdown)은 일반 텍스트 기반의 경량 마크업 언어다. 일반 텍스트로 서식이 있는 문서를 작성하는 데 사용되며, 일반 마크업 언어에 비해 문법이 쉽고 간단한 것이 특징이다. HTML과 리치 텍스트(RTF) 등 서식 문서로 쉽게 변환되기 때문에 응용 소프트웨어와 함께 배포되는 README 파일이나 온라인 게시물 등에 많이 사용된다.

# 1. 제목<a id="chapter-1"></a>
'#' 기호로 제목 나눔 (1~6개)

```
# Heading
Heading
===

## Heading 
Heading
---
```
# Heading
Heading
===
## Heading 
Heading
---
### Heading
#### Heading
##### Heading
###### Heading
---
# 2. 구분선 및 줄바꿈<a id="chapter-2"></a>
```
***
---
___
```
***
---
___

#### 줄바꿈
```
JayJay <br> Just Do It
```
JayJay <br> Just Do It

---
# 3. 글자 속성<a id="chapter-3"></a>

## 3.1 강조<a id="chapter-3.1"></a>
```
*Just Do It*
```
*Just Do It*


```
<u>Just Do It</u>
```
<u>Just Do It</u>


```
_Just Do It_
```
_Just Do It_

```
**Just Do It**
```
**Just Do It**

```
Just Do~~n't~~ ~~qu~~it
```
Just Do~~n't~~ ~~qu~~it

```
Just Do<del>n't</del> <del>qu</del>it
```
Just Do<del>n't</del> <del>qu</del>it


```
**Just Do**~~**n't**~~ ~~**qu**~~**it**
```
**Just Do**~~**n't**~~ ~~**qu**~~**it**


```
==Just Do it==
```
==Just Do it==

## 3.2 색상<a id="chapter-3.2"></a>

```
<span style="color: #CC0000; background-color: #FFFF33;">Just Do It</span>
```
<span style="color: #CC0000; background-color: #FFFF33;">Just Do It</span>

```
<span style="color: red; background-color: skyblue;">Just Do It</span>
```
<span style="color: RED; background-color: skyblue;">Just Do It</span>

```
<span style="color:rgb(245, 235, 13)">rgb(245, 235, 13)</span>
```
<span style="background-color:rgb(245, 235, 13);">Just Do It</span>


<details>
<summary>헥스 코드</summary>
<div markdown="1">

<img src="https://i.namu.wiki/i/WVIbDkRYhgea24bwAKyy6kxilvFLGD0xljO7VuFKLJHcOvAr0-PVVYSTYAeNKM2Anzzzh67pDbCUEwXzamXuzdmFdRcteuwWdR8yF3iH5_sxYRG6KOkzPLGJzAi_RMC6gQ8m5hncOjRgfis1I3Hg3g.webp
" width="700" height="700" />

<a href="https://namu.wiki/w/%ED%97%A5%EC%8A%A4%20%EC%BD%94%EB%93%9C" target="_blank">헥스 코드</a>


</div>
</details>


---

# 4. 인용문구<a id="chapter-4"></a>
```
> Just Do It
>> Just Do It
>>> Just Do It
```
> Just Do It
>> Just Do It
>>> Just Do It

```
> Nike
>> - Slogan `Just Do It`
```
> Nike
>> - Slogan `Just Do It`

---

# 5. 들여쓰기<a id="chapter-5"></a>
```
- *,+,- 를 이용하여 순서가 없는 목록 생성
    - 들여쓰기를 하면 모양이 바뀜
      - 제목

1. list 1
2. list 2
   1. list 3
   2. list 4

1. 머리
   * 머리카락
   * 뚝배기
     + 털
2. 다리
   - 다리털
   - 발가락
     1. 허벅지
```
- *,+,- 를 이용하여 순서가 없는 목록 생성
    - 들여쓰기를 하면 모양이 바뀜
      - 제목

1. list 1
2. list 2
   1. list 3
   2. list 4

1. 머리
   * 머리카락
   * 뚝배기
     + 털
2. 다리
   - 다리털
   - 발가락
     1. 허벅지

---
# 6. 이미지<a id="chapter-6"></a>
```
- 링크와 비슷하지만 앞에 !가 붙는다.
- 인라인 이미지 ![alt text](/test.png)
- 링크 이미지 ![alt text](image_URL)
- 사이즈 변경은 <img width="ooopx" height="ooopx"></img>
- 링크와 이미지를 합친 문법
[ ![텍스트](이미지URL) ]( 링크URL )
[![텍스트](https://static.nike.com/a/images/f_jpg,q_auto:eco/61b4738b-e1e1-4786-8f6c-26aa0008e80b/swoosh-logo-black.png)](https://www.nike.com/kr/)

<img src="https://imgur.com/JUQRe.jpg" width="500" height="200" />

<!-- a태그를 이용한 이미지 링크 생성법-->
<a href="#">
	<img src="https://github.com/images/markdown_syntax.jpg" width="400px" alt="sample image">
</a>

```
- 링크와 비슷하지만 앞에 !가 붙는다.
- 인라인 이미지 ![alt text](/test.png)
- 링크 이미지 ![alt text](image_URL)
- 사이즈 변경은 <img width="ooopx" height="ooopx"></img>
- 링크와 이미지를 합친 문법
[ ![텍스트](이미지URL) ]( 링크URL )
[![텍스트](https://static.nike.com/a/images/f_jpg,q_auto:eco/61b4738b-e1e1-4786-8f6c-26aa0008e80b/swoosh-logo-black.png
)](https://www.nike.com/kr/
)

<img src="https://imgur.com/JUQRe.jpg" width="500" height="200" />


<!-- a태그를 이용한 이미지 링크 생성법-->
<a href="#">
	<img src="https://github.com/images/markdown_syntax.jpg" width="400px" alt="sample image">
</a>


---

# 7. 외부 링크<a id="chapter-7"></a>

- 방법1
```
[Google](http://www.google.com "구글")
[Naver](http://www.naver.com "네이버")
[Github](http://www.github.com "깃허브")

구글 www.google.com
네이버 www.naver.com

[Nike.com](https://www.nike.com/kr/)
```
[Google](http://www.google.com "구글")
[Naver](http://www.naver.com "네이버")
[Github](http://www.github.com "깃허브")

구글 www.google.com
네이버 www.naver.com

[Nike.com](https://www.nike.com/kr/)

- 방법2
```
<a href="www.google.com" target="_blank">구글</a>
```
<a href="www.google.com" target="_blank">구글</a>

---
# 8. 내부(해시)링크<a id="chapter-8"></a>
[보여지는 내용](#이동할 헤드(제목))
- 괄호 안의 링크를 쓸 때는 **띄어쓰기는 -로 연결**, 영어는 모두 **소문자**로 작성


### 테이블 구성
  * [1장](#chapter-1)
  * [2장](#chapter-2)
  * [3장](#chapter-3)

```
  * [1장](#chapter-1)
  * [2장](#chapter-2)
  * [3장](#chapter-3)

###### 1장 <a id="chapter-1"></a>
1장의 내용은.....

###### 2장 <a id="chapter-2"></a>
2장의 내용은.....

###### 3장 <a id="chapter-3"></a>
3장의 내용은.....
```

---
# 9. 코드 블럭<a id="chapter-9"></a>
- 간단한 인라인 코드는 텍스트를 앞뒤로 `기호로 감싸면 됨
- ` 혹은 ~~~ 코드 
- 코드가 여러 줄인 경우, 줄 앞에 공백 네 칸을 추가하면 됩니다.
- ` 옆에 언어를 지정해주면 syntax color가 적용됩니다.

```sql
SELECT a.ename
      ,a.deptno
      ,a.job
    FROM EMP a
```

```python
import pandas as pd
import numpy as np
```
---
# 10. 체크 리스트<a id="chapter-10"></a>
- 줄 앞에 -[x] 를 써서 완료된 리스트 표시
- 줄 앞에 -[]를 써서 미완료되 리스트 표시
- 체크 안에서 강조 외 여러 기능 사용 가능

```
- [x] this is a complete item
- [ ] this is an incomplete item
- [x] @mentions, #refs, [links](), **formatting**, and <del>tags</del> supported
```
- [x] this is a complete item
- [ ] this is an incomplete item
- [x] @mentions, #refs, [links](), **formatting**, and <del>tags</del> supported

---
# 11. Table<a id="chapter-11"></a>
- 헤더와 셀을 구분 할때 3개 이상의 **- 기호 필요**
- 헤더 셀을 구분하면서 **: 기호로 셀안에 내용을 정렬**
- 가장 좌측과 가장 우측에 있는 | 기호는 생략 가능

테이블 생성
```
헤더1|헤더2|헤더3|헤더4
---|---|---|---
셀1|셀2|셀3|셀4
셀5|셀6|셀7|셀8
셀9|셀10|셀11|셀12
```
헤더1|헤더2|헤더3|헤더4
---|---|---|---
셀1|셀2|셀3|셀4
셀5|셀6|셀7|셀8
셀9|셀10|셀11|셀12

테이블 정렬
```
헤더1|헤더2|헤더3
:---|:---:|---:
Left|Center|Right
1|2|3
4|5|6
7|8|9
```

헤더1|헤더2|헤더3
:---|:---:|---:
Left|Center|Right
1|2|3
4|5|6
7|8|9
---
# 12. 토글 버튼<a id="chapter-12"></a>
마크다운에서 토글은 지원하지 않는다. 그렇기 때문에 html의 태그를 사용해서 토글 기능을 사용할 수 있다. 이 기능을 제공하는 html의 태그가 바로 details이다. 그리고 div markdown="1"을 꼭 넣어줘야 하는데 이것은 jekyll에서 html사이에 markdown을 인식 하기 위한 코드이다.
```markdown
<details>
<summary>Nike Slogan</summary>
<div markdown="1">

Just Do It

</div>
</details>
```
<details>
<summary>Nike Slogan</summary>
<div markdown="1">

Just Do It

</div>
</details>
