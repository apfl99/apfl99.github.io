---
title: "DankSharding"
categories:
 - DankSharding
tags: [] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기




---



DankSharding은 이더리움의 계층 중 데이터 가용성 계층에 해당하는 Shard Chain의 복잡한 디자인을 단순화하기 위한 제안으로, 해당 글에서는 DankSharding 이전 상황의 Shard Chain에서 DankSharding이 단순화를 위해 적용한 기술들에 관하여 설명한다.

![image-20230315175327381](../../images/2023-03-15-dankSharding/image-20230315175327381.png)



# DankSharding 이전 상황

---



## 이더리움 트릴레마

블록체인 기술이 금융을 포함한 다양한 산업 분야에서 유용함을 입증하고 있지만, 기존의 중앙 집중형 네트워크와 비교했을 때, 보편적으로 사용되기는 아직해결해야 할 문제가 많다. 대표적인 예시로, 다음과 같이

![image-20230315142354483](../../images/2023-03-15-dankSharding/image-20230315142354483.png)

- 확장성(Scalable): 사용자 수가 늘어나더라도 유연하게 대응할 수 있는 정도
- 탈중앙성(Decentralized): 중앙집권화를 벗어나 분산된 소규모 단위가 모여 자율적으로 운영되는 정도
- 보안성(Secure): 블록체인 내의 데이터나 프로그램을 권한이 없는 이용자가 사용할 수 없도록 제한하여, 외부의 공격으로부터 프로그램을 보호하는 정도

를 모두 충족시킬 수 없는 블록체인 트릴레마가 있다. 블록체인 트릴레마 측면에서 현재 이더리움은 탈중앙성과 보안성에 중점을 둔 체인으로, 확장성의 경우 중앙집중형 네트워크인 Visa가 약 24,000 TPS를 처리할 수 있는 반면, 이더리움의 TPS는 약 15TPS로 이더리움은 확장성 문제를 해결하기 위해 많은 업데이트들이 제안되었다.



## Ethereum Layer

이에 현재 이더리움의 방향은 롤업 중점의 로드맵인데, 롤업(Rollup)은 다음과 같이

![image-20230315145842137](../../images/2023-03-15-dankSharding/image-20230315145842137.png) 

외부 체인(Layer 2)에서 트랜잭션을 실행하고 메인 체인(Ethereum, Layer 1)으로 수백 개의 압축된 트랜잭션 모음과 증명이나 서명을 Layer 1의 트랜잭션 CallData 영역에 게시함으로써 메인 체인의 보안을 상속받는 솔루션으로, 현재 이더리움에 도입 시, 

- 현재 이더리움 블록당 가스 한도: 1250만개
- CallData의 바이트당 가스 비용: 16Gas
- 롤업 검증 데이터(서명이나 증명) 비용: 50만 Gas(추정)
- 롤업에서 하나당 트랜잭션 용량: 12bytes
- 평균 블록 생성 시간: 13초

**=> 하나의 블록에서 롤업 검증 데이터를 제외한 트랜잭션이 들어갈 수 있는 용량: (1250-50)만/16 = 75만 bytes**

**=> 하나의 블록 안에 들어갈 수 있는 트랜잭션 개수: 75만/12 = 62,500개**

**=> TPS: 62,500/13 = ~4807TPS**

 확장성 성능 지표인 TPS(Transaction Per Second)에서 우수한 예측값을 보여주면서 이더리움은 다음과 같은 롤업 중점의 로드맵을 제시했다.

![image-20230315151618350](../../images/2023-03-15-dankSharding/image-20230315151618350.png)

해당 로드맵은 Main Chain, Beacon Chain, Shard Chain, Rollup으로 구성되는데, 각 주요 역할은 다음과 같이

- Main Chain: 이더리움의 전체적인 상태(State, Tx, Receipt)를 저장 및 업데이트
- Beacon Chain: PoS(Proof of Stake)합의 계층으로써, Main Chain에 상태를 업데이트하기 전, 블록 생성 및 검증에 필요한 작업
  - 검증자(Validator), 보증금(Stake) 관리
  - 검증자를 위원회(Committee)와 블록 제안자(Proposer)로 배치
  - 위원회의 투표로, 블록에 대한 분기 및 완결성 보장
  - 검증자에 대한 보상과 처벌
- Shard Chain: 데이터 가용성(Data Availability) 계층으로써, 롤업을 통해 게시된 데이터나 각 샤드 간의 데이터 가용성 증명
- Rollup: 실행(Execution) 계층으로써, 트랜잭션 실행 및 계산, 압축된 트랜잭션 데이터 모음과 증명이나 서명을 Shard Chain으로 게시

동작되며, 종합해서 정리하자면 롤업에서 발생하는 트랜잭션을 Shard Chain에서 데이터 가용성 증명 후, Beacon Chain에서 해당 트랜잭션을 검증함으로써 Main Chain에서 전체 상태를 업데이트하는 방식이다.



## Shard Chain

이더리움 롤업 중점의 로드맵에서 Shard Chain은 앞서 말한 것과 같이 데이터 가용성 계층으로, 여기서 데이터 가용성이란 "데이터가 정상 상태에서 사용될 수 있는 유효한 정도"이다. 이는 롤업과 샤딩을 도입하는데 중요한 개념으로, 롤업에서 Layer 1은 Layer 2의 전체 데이터를 가지고 있지 않고, 서로 다른 샤드간 트랜잭션에서 각 샤드는 자신의 샤드에 해당하는 데이터만 가지고 있는 상황에서 검증은 다음과 같은 구조에서  

![image-20230315145842137](../../images/2023-03-15-dankSharding/image-20230315145842137.png)



![image-20230316120130735](../../images/2023-03-15-dankSharding/image-20230316120130735.png)

머클 루트 재계산을 통해 이루어지는데, 만약 다음과 같은 머클 트리에서 데이터 C를 증명하고자 할 때, 

![image-20230316120421999](../../images/2023-03-15-dankSharding/image-20230316120421999.png)

Layer 2의 Aggregator와 샤드의 Proposer는 머클 루트를 계산하기 위한 D, H(A-B), H(E-H)를 게시한다. 그러나 이 때 악의적인 Aggregator나 Proposer가 H(E-H)를 게시하지 않는 상황에서 데이터 가용성이 고려되지 않는다면, 롤업에서의 Layer 1 검증자나 샤드의 검증자는 우선 전달받은 데이터로 H(A-H)를 재계산할 것이고, 해당 검증은 유효하지 않을 뿐더러 DoS 공격과 같은 자원낭비 문제를 발생할 수 있다.

따라서 이러한 문제와 같이 검증자가 전달받은 데이터가 정상 상태인지 유효한지 판단하는 문제를 데이터 가용성 문제라고 하며, 이를 해결하기 위해 Shard Chain에서는

![image-20230316133015294](../../images/2023-03-15-dankSharding/image-20230316133015294.png)

이와 같은 Origin Data, Erasure Coding으로 확장된 데이터, Merkle Proof로 구성된 Body와 KZG commitment으로 생성된 KZG Root으로 구성된 Header의 구조를 가진 Blob 형태의 데이터를 사용한다.



### Blob

---

Shard Chain의 데이터 형태인 Blob은 데이터 가용성 문제를 해결하기 위한 대책인 DAS(Data Availability Sampling)를 도입하기 위한 자료 구조이다.

DAS란 데이터 가용성을 보장하기 위한 메커니즘으로, 다음과 같이 수행되는데

![image-20230316141439413](../../images/2023-03-15-dankSharding/image-20230316141439413.png)

여기서 Blob은 아직 Shard Chain에서 사용되는 Blob이 아닌 블록을 임의의 길이를 가진 문자열과 배열을 인코딩하는 방식인 RLP(Recursive Length Prefix)인코딩을 통해 직렬화한 후, 이진데이터로 나타내서 잘라낸 것으로 다음과 같이 나타내며,

![blob](../../images/2023-03-15-dankSharding/blob.png)

해당 Blob을 통해 검증자(Client)는 여러 라운드에 거쳐 임의의 인덱스를 뽑아 다운로드함으로써 데이터의 절반 이상이 가용한지 확인한다.

이를 통해 DAS는 통계적인 보증을 제공하지만 여전히 샘플링에 의해 검열되지 않은 데이터가 남아있을 수 있다. 따라서 Blob에는 Erasure Coding으로 확장된 데이터와 Erasure Coding으로 확장된 데이터에 대한 증명인 KZG commitmment으로 생성된 KZG Root가 추가적으로 사용된다.



#### Erasure Coding

---

Erasure Coding이란 원래 데이터를 확장함으로써 데이터 일부가 손실되어도 나머지 데이터로 원래 데이터를 복구할 수 있도록하여 샘플링에 의해 검열되지 않은 데이터가 생길 확률을 줄인다. 

Erasure Coding에는 Reed-Solomon Code를 활용하는데, Reed-Solomon Code는

1. Lagrange Interpolation(라그랑주 보간)
2. Modular (연산값 p는 다항식에 나눗셈이 포함되기 때문에 역원을 가져야 해서 소수)

으로 구성되어, 원래 데이터를 통해 Lagrange Interpolation으로 원래 데이터가 지나는 다항식을 구하여 해당 다항식 위의 점을 확장 데이터로 사용하고 Modular 연산을 통해 확장 데이터 값을 제한함으로써, 연산을 단순화한다. 이는 원래 데이터가 지나는 다항식으로 동작하기 때문에 확장된 데이터 중 일부 데이터가 손실되더라도 원래 데이터의 길이만큼 데이터가 남아있다면, 원래 데이터를 복구할 수 있다.

따라서 만약 원래 데이터의 길이가 k, 확장된 데이터의 길이가 2k라고 가정할 때 원래 데이터 복구에 필요한 데이터는 k이상이기 때문에 악의적인 노드는 DAS에 의해 검열되지 않기 위해 k+1이상 데이터를 숨겨야한다. 이는 한 번의 라운드에서 데이터가 가용할 확률을 약 50%(**(k+1)/****2k** )로 낮추고, 예를 들어 30번의 라운드가 진행된다고 할 때, 확률은 **2^(-30)**(약 10억분의 1)로 DAS를 효과적으로 보완한다. 

그러나 Erasure Coding으로 데이터를 확장했을 때, 확장된 데이터가 제대로 코딩되지 않으면 원래 데이터를 복구할 수 없어 확장된 데이터가 같은 방정식에 존재하는지 증명이 필요하다. 따라서 KZG commitment를 사용한다.

#### KZG commitment

----

KZG commitment는 증명자가 commitment 값인 타원곡선 점을 검증자에게 보낸 후, 증명자는 작업 중인 다항식을 변경할 수 없기 때문에 이를 commitment라고 하며, 하나의 다항식에 대해서만 유효성 증명이 가능한 특성을 활용하여 증명자가 제공하는 데이터(확장된 데이터)가 같은 다항식 내에 존재하는지 확인한다.

(추가)



### DankSharding 등장

---

이러한 Blob구조를 통해, Shard Chain에서는 다음과 같이 

![image-20230316163024688](../../images/2023-03-15-dankSharding/image-20230316163024688.png)

각 Shard의 제안자는 Blob형태로 데이터 가용성 증명을 하는 위원회에 제안하고, 위원회는 데이터가 가용하면 투표를 하여, 위원회의 2/3이 해당 Blob이 가용하다고 투표하면, Beacon Block의 제안자는 합의 결과를 집계하여 Beacon Chain에서 데이터 검증 후, Main Chain에 상태 업데이트를 진행한다.

그러나 해당 디자인은 Beacon Chain의 블록 제안자가 각 샤드에서 검증한 결과를 집계하는 과정에서 Beacon Chain의 블록 제안 주기와 맞춰 동작하려면, 엄격한 동시성을 가져야 함으로 복잡한 디자인이 요구된다.

따라서 DankSharding은 기존의 Shard Chain 디자인의 복잡성을 개선하고자 제안되었다.



# DankSharding

---

DankSharding이란 