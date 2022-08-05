---
title: "[블록체인 기본] 블록체인 구조, 주요 용어"
categories:
 - GroundX
tags: [Block, BlockHeader, HashPointer,BlockHeight, BlockGenerationCycle] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기
---



# Linked List에서 아이템은 메모리 주소를 기반으로 연결되어 데이터를 구분짓고 연결되어 있다. 

#  그렇다면 블록체인에서는 어떠한 방식으로 데이터를 구분짓고 연결할까?

--------------------------



## **블록, 블록헤더, 해시포인터**

![img](../../images/2022-08-03-gxblockchain2/img-20220803095734998.png)

- 블록은 헤더와 바디로 구분
- 헤더는 블록을 설명하는 정보와 이전 블록의 해시를 포함
- 이전 블록의 해시(Hash Pointer)를 가지기 때문에 어떤 블록이 앞에 와야하는지 결정적으로 알 수 있고, 이를 바탕으로 블록의 순서를 결정할 수 있음
  - 사실 순서보다는 체인된 상태가 중요



### **해시 값이 같다면 문제가 생기지 않을까?**

--------------------------

- 데이터를 기록할 때 기본적으로 같은 해시 값이 나오기 힘들다.(ex. seq num)
- 또한, 같은 블록이 연결되어 있더라도 뒤의 블록이 앞의 블록을 기억한다는 체인된 결과가 중요한 것이기 때문에 상관없다.



<br>

# **블록체인 용어**

--------------------------



## **블록높이, 블록생성주기**



### **블록높이란?**

--------------------------

블록들을 이전 블록이 아래에 최근 블록이 위로 오도록 정렬하면 블록이 생성됨에 따라 체인의 높이가 늘어난다.

블록의 순서를 그 블록이 위치한 '높이'(block height)라 부른다. 첫 번째 블록은 편의상 높이를 0이라 한다.



### **블록생성주기란?**

--------------------------

다음 블록을 생성하기까지 걸리는 시간을 블록생성시간이라하고 블록생성시간이 비교적 일정한 경우 블록생성주기란 표현을 사용한다.

- 실제로 중요하다, 데이터가 블록에 들어가는 것을 '체결'되었다라고 하는데, 이 과정에 따라 블록의 용도가 달라진다.
  - ex. 비트코인의 경우, 블록생성주기가 10분정도인데 비트코인으로 결제를 한다할 때, 서비스 사용자의 불편을 초래할 수 있다.


<div class="notice">
  <p>본 포스팅은 Klaytn 스마트계약과 탈중앙앱을 수강하며 작성한 글입니다.</p>
</div>