---
title: "[블록체인 기본] 공개키 암호화와 전자서명"
categories:
 - GroundX
tags: [Electronic signature, Symmetric-key algorithm, ASymmetric-key algorithm] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기
---

# **암호화**

---------------------------

## **대칭키 암호/비대칭키 암호**

암호화에 사용한 키와 복호화에 사용한 키가 동일한 경우 **대칭키 암호**로 분류

암호화에 사용한 키와 복호화에 사용한 키가 다를 경우 **비대칭키 암호**로 분류



## **비대칭키 암호(공개키 암호)**

두 개의 키를 사용하여 암호화와 복호화를 실행

- 암호화에 사용되는 키 = 공개키(Public Key, PK)
- 복호화에 사용되는 키 = 비밀키(private Key/Secret Key, SK)



### 비대칭키 암호의 목적

"누구든지 암호화할 수 있지만 비밀키를 아는 사람만 복호화할 수 있어야 한다."

- 공개키와 비밀키는 한 쌍으로 묶여있는 아주 큰 숫자들
- 비밀키로부터 공개키를 도출하는 것은 쉬움
- 공개키로부터 비밀키를 찾는 것은 매우 어려움



## **공개키 암호를 사용한 안전한 통신**

1. A는 B의 공개키로 암호화
2. B는 자신의 비밀키로 복호화

-> 그러나 통신 과정에서 공개키는 말 그대로 공개되어 있는 키이기 때문에 어떤 사용자가 보냈는지 인증할 수 없다. 따라서 나온 것이 **"전자서명"**



## **전자서명**

**비대칭키 암호는 지정된 사람만 정보를 확인할 수 있도록 도움(privacy)**

- 보낸 사람은 어떻게 확인할까?

**전자서명은 누가 정보를 보냈는지 알기 위해 사용(non-repudiation)**

- 전자서명은 비대칭암호의 응용 프로그램
- 서명은 비밀키로만 생성가능
- 공개키는 서명이 짝을 이루는 비밀키로 생성되었는지를 검증



## **공개키 암호와 전자서명을 사용한 안전한 통신**

1. A는 자신의 비밀키로 서명을 생성 & A는 B의 공개키로 데이터를 암호화
2. B는 A의 공개키로 서명을 복호화(확인) & B는 자신의 비밀키로 데이터를 복호화



### **+ 해싱을 통한 데이터 위/변조 확인**

위 과정에 해싱을 활용하여 데이터의 해쉬 값을 검사하는 과정이 추가된다면, 데이터의 위/변조가 확인 가능하다.

<div class="notice">
  <p>본 포스팅은 Klaytn 스마트계약과 탈중앙앱을 수강하며 작성한 글입니다.</p>
</div>