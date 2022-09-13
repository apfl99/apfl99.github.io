---
title: "비트코인 블록 구조"
categories:
 - BEB
tags: [비트코인 헤더] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기


---



# 블록 구조

---



## 비트코인의 블록 구조

비트코인의 블록 구조는 헤더와 거래로 나누어져있다. 



### 헤더의 구조

---

![img1](../../images/2022-09-13-blockstructure/img1.png)

{: .align-center}



#### 1. 버전

- 버전 정보를 나타내며, 소프트웨어 프로토콜 업그레이드 추적을 위한 버전 번호이기도 하다.



#### 2. 이전 블록 헤더 해시

- 블록체인을 체이닝하는 개념으로 이전 블록에 대한 헤더 해시를 저장한다.



#### 3. 타임 스탬프

- 해당 블록 채굴 시각을 기록한다.



#### 4. 머클 트리

- [머클 트리](https://apfl99.github.io/beb/dlt/#%EB%A8%B8%ED%81%B4%ED%8A%B8%EB%A6%AC)
- 라이트 노드들이 저장하는 값이다.



#### 5. 난이도 비트

- 해당 블록을 채굴할 때 난이드를 의미하며, 해당 숫자는 블록의 높이에 따라 자동 설정된다.
- Difficulty(난이도) = MAX_TARGET / current_target으로 정의되는데, 여기서 MAX_TARGET은 첫 난이도로 1이고 current_target은 목표값이다.
  - 이를 대입해보면 채굴 목표값이 낮을 수록 더 낮은 현재 블록 헤더 해시값을 찾아야 하기 때문에 난이도가 올라가고 이는 식에서도 관계를 찾을 수 있다.



#### 6. 논스

- 논스는 유일하게 변하는 값이며, 채굴시, 논스의 값을 바꿔가며 해시 연산을 통해 목표값보다 낮은 값을 찾는다.
- 비트코인의 경우 난이도를 블록 생성 주기가 10분으로 유지되게 자동 조절되며, 난이도 조절 방법은 다음과 같다.
  - Next Difficulty = current difficulty * 2 weeks/T(Time in which previous 2016 blocks found)
  - 위와 같이 비트코인은 2016개의 블록을 주기로 이전 주기에서 2016개의 블록이 형성될 때까지의 시간을 체크하여 새로운 난이도를 조정한다. 


<div class="notice">
  <p>본 포스팅은 코드스테이츠 BEB 과정을 수강하며 작성한 글입니다.</p>
</div>