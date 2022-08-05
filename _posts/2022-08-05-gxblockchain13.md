---
title: "[블록체인 상태와 트랜잭션] 트랜잭션 경로"
categories:
 - GroundX
tags: [Transaction Journey] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기
---

# **Transaction Journey**
----------------

![img](../../images/2022-08-05-gxblockchain13/img-20220805133058397.png)
{: .align-center}

## **Alice와 Node 사이의 통신**

### **사용자와 Node는 같은 프로토콜로 통신해야 한다.**

**Alice -> Node**

- Alice는 TX를 생성, 서명하여 Node에게 전달
- 이때 데이터 구조는 온전하게 전달하고자 RLP 알고리즘으로 TX를 직렬화
- Alice와 Node가 같은 프로토콜로 통신하는 것이 중요

**Node -> Alice**

- 올바른 TX 수신 시 TX 해시를 반환
- TX 체결 시 Receipt를 반환; 소요된 Gas, TX 해시, input 등이 기록

<div class="notice">
  <p>본 포스팅은 Klaytn 스마트계약과 탈중앙앱을 수강하며 작성한 글입니다.</p>
</div>