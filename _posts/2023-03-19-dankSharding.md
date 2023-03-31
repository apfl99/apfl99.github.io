---
title: "DankSharding"
categories:
 - DankSharding
tags: [sharding, rollup, data availability, das, danksharding] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기




---



목차

- DankSharding 등장배경 
  - Sharding
  - Rollup

- DankSharding 이란
- DankSharding 발전 방향
  - Data Availability
    - Data Availability Problem
    - DAS(Data Availability Sampling)
    - Erasure Coding
    - KZG commitment

  - Rollup
    - Optimistic Rollup
    - ZK Rollup

- DankSharding
  - PBS
    - MEV 중앙화 문제
    - PBS
    - hybrid PBS(PBS + crList)
  - 2-Dimensional KZG Scheme
  
- Summary & 향후 연구



# DankSharding 등장 배경

---



## Sharding

DankSharding 이전의 샤딩은 이더리움의 확장성 솔루션으로, 다음과 같이 

![image-20230320001431002](../../images/2023-03-19-dankSharding/image-20230320001431002.png)

각 샤드마다 Main Chain의 State를 나누어 처리하기 때문에 검증 노드들은 자신이 소속한 샤드의 데이터만 검증하면 되어 데이터 처리량의 이점을 가질 뿐만 아니라, 각 노드가 검증을 위해 저장해야 할 데이터양이 분할되기 때문에 검증 노드의 하드웨어 요구사항을 줄여 더 많은 사람들이 블록체인에 참여하여 탈중앙화의 이점을 가질 수 있었다.



## Rollup

이후 다음과 같은

![image-20230320002942785](../../images/2023-03-19-dankSharding/image-20230320002942785.png)

 외부 체인(Layer 2)에서 실행한 압축된 트랜잭션 모음과 Layer 2의 State Root로 구성된 Batch을 Ethereum의 트랜잭션 Calldata 영역에 게시함으로써 이더리움의 보안을 상속받는 확장성 솔루션인 롤업이 등장하고 도입 시 이더리움 전송의 경우, 

- 현재 이더리움 블록당 가스 한도: 1250만개

- CallData의 바이트당 가스 비용: 16Gas

- 롤업 검증 데이터(서명이나 증명) 비용: 50만Gas(추정)

- 롤업에서 하나당 트랜잭션 용량: 12bytes

- 평균 블록 생성 시간: 13초

=>  **하나의 블록에서 롤업 검증 데이터를 제외한 트랜잭션이 들어갈 수 있는 용량****: (1250-50)****만****/ 16 = 75****만** **bytes**

=> **하나의 블록 안에 들어갈 수 있는 트랜잭션 개수****: 75****만****/12 = 62,500****개**

=> **TPS: 62,500/13 = ~4807TPS**

위와 같이 확장성 성능 지표인 TPS(Transaction Per Second)에서 우수한 예측 결과를 보여주면서 이더리움은 롤업 중점의 로드맵을 예정했다.

이에 샤딩의 역할은 데이터 처리가 아닌, 롤업에서 Layer 1에서 검증을 위해 Layer 2로 게시한 데이터인 Batch가 가용한가에 대한 데이터 가용성 문제에 중점을 두었고,  데이터 가용성 문제는 이더리움에서 롤업이 아니더라도 최신 블록 헤더에만 동기화하고 블록 전체 데이터를 가진 풀 노드에서 다른 정보를 요청하는 이더리움 노드인 라이트 클라이언트가 전체 노드로부터 받은 데이터에 대한 가용성 증명을 연구하고 있었기에 앞서 연구된 데이터 가용성 증명 개념을 활용하여 데이터 가용성 증명을 효율적으로 할 수 있는 샤딩 디자인인 DankSharding이 등장하였다.





# DankSharding이란

----

DankSharding이란 다음과 같이 이더리움이 확장성 개선을 위해 도입 예정이었던 VM 실행 데이터를 처리하는 데이터 샤딩에서 롤업 중점의 로드맵으로 전환함에 따라 Batch의 데이터 가용성 증명을 위해 제안된 샤딩 디자인이다. 

![image-20230319232717880](../../images/2023-03-19-dankSharding/image-20230319232717880.png)

이와 같이 샤딩 디자인의 역할이 실행 데이터 처리에서 데이터 가용성 증명으로 바뀌자

![image-20230315175327381](../../images/2023-03-19-dankSharding/image-20230315175327381.png)

데이터 가용성 측면에서 봤을 때, 기존의 샤딩은 왼쪽과 같이 실행 데이터 검증을 위해 각 샤드 위원회의 검증자들은 해당 샤드의 모든 데이터를 다운로드함으로써 데이터 가용성을 확인했다면, 

DankSharding은 기존의 Beacon Block에 이더리움의 데이터 가용성 증명을 위한 연구를 활용한 Blob이라는 형태의 블록을 추가하여 하나의 큰 Beacon Block을 생성하고 블록을 검증하는 한 위원회에서 Blob을 통해 데이터 가용성 증명을 효율적으로 검증할 수 있도록 하였다.





# DankSharding 발전 방향

---





## Data Availability

데이터 가용성(Data Availability)이란 데이터가 정상 상태에서 사용될 수 있는 유효한 정도로, DankSharding 이전의 이더리움은 최신 블록 헤더에만 동기화하고 블록 전체 데이터를 가진 풀 노드에서 다른 정보를 요청하는 이더리움 노드인 라이트 클라이언트가 풀 노드로부터 받은 데이터의 유효성을 검증하기 전 데이터가 가용한가를 판단하기 위해 데이터 가용성 증명에 관한 연구를 진행했다.





### Data Availability Problem

---


라이트 클라이언트는 풀 노드로부터 받은 데이터를 사기 증명을 통해 블록 유효성을 검증한다.

여기서 사기 증명이란 다음과 같이 데이터 C를 증명하고자 할 때, 

![image-20230320141049509](../../images/2023-03-19-dankSharding/image-20230320141049509.png)

루트값을 계산하기 위한 H(D), H(C-D), H(A-B), H(E-H)를 풀 노드로부터 제공받아 계산 결과값과 자신이 가진 블록 헤더인 H(A-H)값이 일치하는지를 통해 증명한다.

그러나 해당 상황에서 만약 악의적인 풀 노드가 H(E-H)를 제외하고 라이트 클라이언트에게 전송한다면, 라이트 클라이언트는 우선 전달받은 데이터로 최대 H(A-D)까지 계산할 수 있기 때문에 유효하지 않은 검증을 할 뿐만 아니라 DoS와 같은 자원낭비 문제를 초래할 수 있다.

이와 같이 전체 데이터가 아닌 일부 데이터를 가진 노드가 현재 사용되는 데이터가 사용가능한 데이터인지 알 수 없는 문제를 데이터 가용성 문제라고 하며, 이를 Fisherman's Dilemma로 확장하면 다음과 같이

![image-20230320141924079](../../images/2023-03-19-dankSharding/image-20230320141924079.png)

제 3자인 노드는 전체 데이터를 가지고 있지 않아, 악의적인 노드가 블록 안의 특정 데이터를 빼놓고 정상적인 노드에 의해 경보 및 적발되어 다시 정상 데이터를 올리는 Case 1과 정상적인 노드가 정상 데이터를 올리고 악의적인 노드가 거짓 경보를 발생시키는 Case 2를 구분할 수 없는 문제이다.

이를 해결하기 위해 이더리움에서는 DAS(Data Availability Sampling)을 고려했다.





### Data Availability Sampling(DAS)

---

DAS란 데이터 가용성 샘플링으로, 

![image-20230319182059567](../../images/2023-03-19-dankSharding/image-20230319182059567.png)

위와 같이 데이터 가용성 증명을 하는 클라이언트가 전체 데이터를 다운로드 받는 것이 아닌

![blob](../../images/2023-03-19-dankSharding/blob.png)

위와 같이 임의의 길이를 가진 문자열과 배열을 인코딩하는 방식인 RLP(Recursive Length Prefix)인코딩을 통해 16진수로 직렬화된 데이터를 이진 데이터로 나타내어 임의의 인덱스를 여러 라운드에 거쳐 다운로드함으로써 통계적인 보증을 얻는 방법이다.

그러나 여전히 검열되지 않은 데이터의 확률이 존재하며, 이를 보완하기 위해 데이터를 확장하는 Erasure Coding과 데이터가 올바르게 확장되었는지 검증하는 KZG commitment를 같이 사용하고, DankSharding에서는 다음과 같이

![j4afAvY](../../images/2023-03-19-dankSharding/j4afAvY-9632159.png)

각 샤드의 제안자가 Original Data, Erasure Coding으로 확장된 Data, Merkle Proof, Commitment로 구성된 Blob 형태의 데이터를 제안한다.





### Erasure Coding

---

Erasure Coding이란 원래 손실되거나 손상된 데이터를 재구성하는 데 사용할 수 있는 중복 데이터를 생성하여 데이터 손실이나 손상을 방지하는 데 사용되는 기술로, DAS에서는 원래 데이터를 확장함으로써 데이터 일부가 손실되어도 나머지 데이터로 원래 데이터를 복구할 수 있도록 하여 샘플링에 의해 검열되지 않은 데이터가 생길 확률을 줄인다.

Erasure Coding에는 Reed-Solomon Code를 활용하는데, Reed-Solomon Code는 

1. Lagrange Interpolation(라그랑주 보간)
2. Modular 연산

으로 구성되어,  다음과 같이 원래 데이터를 통해 Lagrange Interpolation으로 원래 데이터가 지나는 다항식을 구하여 해당 다항식 위의 점을 확장 데이터로 사용하고

![lagrange interpolation 3](../../images/2023-03-19-dankSharding/lagrange interpolation 3.png)

이후 Modular 연산을 통해 확장 데이터 값을 제한함으로써, 연산을 단순화한다. 이는 원래 데이터가 지나는 다항식으로 동작하기 때문에 확장된 데이터 중 일부 데이터가 손실되더라도 원래 데이터의 길이만큼 데이터가 남아있다면, 원래 데이터를 복구할 수 있다.

![modular 4](../../images/2023-03-19-dankSharding/modular 4.png)

따라서 Erasure Coding으로 다음과 같이 데이터를 확장했을 때, 

- 원래 데이터의 길이: k
- 확장된 데이터의 길이: 2k 

원래 데이터 복구에 필요한 데이터는 k이상이기 때문에 악의적인 노드는 DAS에 의해 검열되지 않기 위해 k+1이상 데이터를 숨겨야한다.

이는 한 번의 라운드에서 데이터가 가용할 확률을 약 50%(**(k+1)/****2k** )로 낮추고, 예를 들어 30번의 라운드가 진행된다고 할 때, 데이터가 전부 가용할 확률은 **2^(-30)**(약 10억분의 1)로 DAS를 효과적으로 보완한다.





### KZG commitment

---

KZG commitment는 Prover가 commitment 값인 타원곡선 점을 Verifier에게 보낸 후, Prover는 작업 중인 다항식을 변경할 수 없기 때문에 이를 commitment라고 하며, 하나의 다항식에 대해서만 유효성 증명이 가능한 특성을 활용하여 Erasure Coding으로 확장된 데이터가 같은 다항식 내에 존재하는지 확인한다.

KZG commitment에는 암호학적 성질을 부여하기 위해 이미 정의된 페어링된 타원곡선을 사용하는데, 이는 다음과 같이 이미 정의된 페어링된 타원곡선의 성질을 이용하여,

![kzg basic](../../images/2023-03-19-dankSharding/kzg basic.png)

미리 정의된 타원곡선 시작점 G1, G2와 비밀 s를 서로 페어링된 타원곡선 1, 2에 대응시킨 점 s1,s2에서

Verifier는 Erasure Coding에서 정의된 다항식에 대한 타원곡선 점인 Commitment와 p(z)가 y라는 증명에 대한 타원곡선 점인 Proof를 가지면, 다항식 p(x) 모르더라도 자신이 증명하는 데이터 z가 p(x)위에 존재하는지 증명할 수 있도록 할 수 있게 함으로써, Erasure coding에서 확장된 데이터가 올바르게 확장되었는지 증명이 가능하도록 한다.

이를 수식으로 나타내면, 

- Erasure Coding에서 데이터가 지나는 다항식: p(x)
- 미리 정의된 타원곡선 시작점: [G]<sub>1</sub>, [G]<sub>2</sub>
- 비밀 s를 서로 페어링된 타원곡선 1, 2에 대응시킨 점: [s]<sub>1</sub>,[s]<sub>2</sub> (이후 s는 파기)

가 먼저 설정되고, 다음과 같이 진행된다.

![kzg commitment flow1](../../images/2023-03-19-dankSharding/kzg commitment flow1.png)

![kzg commitment](../../images/2023-03-19-dankSharding/kzg commitment-9969590.png)





## Rollup

Rollup이란 Layer 2 확장성 솔루션으로, 다음과 같이 

![img](../../images/2023-03-19-dankSharding/access-gagraphic-3200051-20230322174302164.jpg)

이더리움의 User는 Smart Contract를 통해 롤업 프로젝트에 예치함으로써 Layer 2에 참여하게 되고, Layer 2에서 발생한 트랜잭션 데이터는 Layer 2의 Aggregator를 통해 트랜잭션에 대한 증명과 함께 Layer 1의 Smart Contract에 게시함으로써 이더리움의 보안을 상속받는다.

Layer 2에서 Layer 1으로 게시하는 과정을 보면 DankSharding의 등장배경에서 봤던 것과 같이 

![image-20230320002942785](../../images/2023-03-19-dankSharding/image-20230320002942785.png)

외부 체인(Layer 2)에서 Aggregator를 통해 실행한 압축된 트랜잭션 모음과 Layer 2의 State Root로 구성된 Batch을 Ethereum의 트랜잭션 Calldata 영역에 게시하며, 여기서 Aggregator가 게시한 Batch의 Post-state Root가 유효한지 증명하는 방식에 따라 Optimisitic Rollup과 ZK Rollup으로 나뉜다.





### Optimistic Rollup

---

Optimistic Rollup은 우선 Aggregator가 올린 트랜잭션 데이터가 유효하다는 가정하에 먼저 Layer 1의 Smart Contract에 게시하고, 이후 사기 증명을 통해 Layer 2에서 올린 상태 루트가 유효한지 증명한다.

Layer 1의 사기 증명을 위해  Layer 2에서는 다음과 같이 

![img](../../images/2023-03-19-dankSharding/tree.png) 

{: .align-center}

Batch의 트랜잭션이 포함되기 전 롤업의 Pre-state root로부터 Batch의 트랜잭션이 포함된 후 롤업의 Post-state root가 재계산되기 위한 녹색 데이터를 제공하고, 일정 기간을 두고 Aggregator가 올린 Batch가 유효한지 State Root의 재계산을 통해 검증함으로써 트랜잭션 데이터에 대한 사기 증명이 이루어진다.

이 과정에서, 일정 기간 내에  Layer 1에서 재계산된 Post-state root가 Batch의 Post-state root와 일치하지 않다는 사기 증거가 제출될 경우, Layer 2는 트랜잭션을 다시 실행하고 일치할 경우, Post-state root가 해당 Smart Contract에 업데이트 된다.





### ZK Rollup

----

ZK Rollup은 Zero-Knowledge Rollup의 약자로, 영지식 증명을 활용한 롤업이다.

영지식 증명이란 정보를 공개하지 않고, 정보의 유효성을 증명할 수 있는 방법으로, 다음과 같은 요소로 구성되어

- Prover: 자신이 가진 정보를 공개하지 않고, Verifier에게 자신이 정보를 알고 있다는 사실을 증명하고 싶은 참여자
- Verifier: Prover가 해당 정보를 알고 있음을 검증하고 싶은 참여자
- Secret: Prover가 알고 있다는 것을 증명하고 싶은 정보이자, 공개하지 않는 정보
- Challenge: Verifier가 Prover에게 Secret을 가지고 있는지 확인하기 위해 문제를 내는 과정

![image-20230328163944994](../../images/2023-03-19-dankSharding/image-20230328163944994.png)

위와 같이 Prover는 Verifier가 낸 Challenge에 따라 Secret을 공개하지 않는 Proof를 생성 및 전송하고, 이를 통해 Verifier가 확률적인 검증을 하게 된다.

따라서 이러한 점을 활용하여 ZK Rollup에서는 증명을 위해 모든 트랜잭션 데이터를 게시하는 Optimistic Rollup과 달리, 

- Prover: Aggregator
- Verifier: Layer 1의 Smart Contract
- Secret: 트랜잭션 데이터

의 구조를 가지고, Prover 역할인 Aggregator는 Layer 1의 Smart Contract에 게시하기 전, 트랜잭션 데이터에 대한 유효성 증명(Merkle Proof)을 수행하고, Pre-State Root로부터 트랜잭션 데이터가 포함되어 Post-State Root가 도출되었다는 영지식 증명을 게시하고 이를 활용하여 Verifier역할인 Layer 1의 Smart Contract는 Pre-state Root로부터 Post-state Root가 유효한지 증명한다.





# DankSharding

---

DankSharding은 앞선 개념를 토대로, 다음과 같은 구조를 갖는데

![image-20230328174139242](../../images/2023-03-19-dankSharding/image-20230328174139242.png)

이는 Beacon Block과 함께 각 샤드의 제안자가 제안한 Blob들로 새로운 Blob을 재구성하여 구성된 Blob형태의 하나의 큰 블록을 생성하고, 한 위원회에서 Blob을 통한 데이터 가용성 검사와 Beacon Block 검증을 수행한다.

이러한 구조가 도입되기 위해 DankSharding에서는 두가지 개념을 제안하는데, 

첫번째는 기존의 블록에서 Blob 데이터가 추가되어 블록 사이즈가 커지고 블록을 제안하기 위한 노드의 하드웨어 요구사항이 증가함에 따라, 블록 제안의 중앙화를 방지하기 위해 블록을 생성하는 Builder와 블록을 제안하는 Proposer를 분리하는 PBS(Proposer-Builder Separation)와

두번째는 각 샤드의 Blob이 합쳐져 대용량 데이터에 대한 하나의 Blob으로 재구성하는 과정을 단순화하기 위해 대용량 데이터를 하나의 KZG commitment로 묶기 위해 재계산하는 것이 아닌 Erasure Coding을 통해 기존 Blob들의 KZG commitment를 2배 확장하여 Blob을 재구성하는 2-Dimensional KZG Scheme가 있다.





## PBS(Proposer-Builder Separation)

PBS란 DankSharding으로 새롭게 도입된 개념이 아닌 이더리움의 PoS(Proof of Stake)에서 생기는 MEV(Maximum Extractable Value) 중앙화 문제를 해결하기 위한 목적으로 고안된 개념으로 DankSharding에서는 기존의 Proposer의 역할인 블록 생성과 제안을 Builder와 Proposer로 나눔으로써 블록 제안의 중앙화 문제를 해결한다.



### MEV 중앙화 문제

---

이더리움의 PoS의 경우, 검증자들이 예치한 금액에 따라 위원회나 블록 제안자로 선발되는데 블록 제안자로 선발될 경우, 블록 제안자는 수익의 극대화를 위해 다음과 같이 

![image-20230324150709985](../../images/2023-03-19-dankSharding/image-20230324150709985.png)

블록 생성시, 임의로 트랜잭션을 포함, 제외 및 재배치할 수 있다. 

이러한 과정에서 표준 블록 보상 및 가스 수수료를 초과하여 블록 제안자가 추출할 수 있는 최대 가치를 MEV라 하는데, 예를 들어 블록 제안자는 다음과 같이 

![image-20230324152603284](../../images/2023-03-19-dankSharding/image-20230324152603284.png)

수익성이 높은 거래를 Mempool에서 포착할 경우, 블록 제안자는 동일한 트랜잭션을 생성한 채 가스비만 더 높게 제출하여 기회를 가로챔으로써 수익을 얻을 수 있다.

여기서 MEV는 일반적으로 소규모로 운영되는 노드보다, 하드웨어 사양을 가지고 빠른 멤풀 업데이트 속도, 효율적인 MEV 추출 알고리즘을 가진 노드에 유리하다.

이렇게 MEV가 특정 중앙화된 블록 제안자들에게만 이득이 되고, 소규모 노드들에게는 불리한 구조를 MEV 중앙화 문제라고 하며, 이를 개선하고자 PBS가 등장하였다.





### PBS

---

먼저 PBS의 전체적인 동작을 설명하자면 다음의 요소로 구성되어, 

- Builder: 블록 안에 포함될 트랜잭션 및 트랜잭션 순서를 정하여 MEV 추출 블록 생성 및 생성한 블록을 Proposer에게 제출
- Proposer: Builder들이 제안한 블록 중 자신에게 이익이 되는 블록을 선택하여 제안

각각의 Builder는 다른 Builder가 자신이 생성한 블록의 MEV를 가로챌 수 있기 때문에 다음과 같이 생성한 블록의 헤더와 Proposer가 Builder로부터 받는 금액인 bid만 제출하며, 

![img](../../images/2023-03-19-dankSharding/1*6eAgi76oNo7zzLBC6__m8g.png)

Proposer는 bid가 높은 Block을 선택하고

![img](../../images/2023-03-19-dankSharding/1*9hgk-8Jjig3M-6k1QUDBfA.png)

이후 자신이 만든 블록 헤더가 선택됐음을 확인한 Builder B는 해당 헤더와 일치하는 블록 바디 또한 추가로 공개하여 Proposer는 Builder가 제출한 블록을 포함하여 제안한다.

따라서 PBS에서 블록 생성은 일부 높은 사양의 하드웨어를 가진 노드가 수행함으로써 중앙화되지만, 블록 제안은 제출받은 블록을 제안하기만 하면 되기 때문에 낮은 사양의 노드여도 가능하기 때문에 블록 제안의 탈중앙화 이점을 가지며, MEV 또한 Builder의 수익을 Proposer도 나눠 갖기 때문에 MEV 중앙화 문제에도 해결방안이 될 수 있다.

이러한 개념을 토대로 이더리움의 PBS는 아직 연구가 진행 중이며, 가장 유력한 PBS 방법은 Two-Slot PBS로 다음과 같이

![image-20230330104026863](../../images/2023-03-19-dankSharding/image-20230330104026863.png)

이더리움의 블록 제안 기간 단위인 Slot을 두 번 사용함으로써, 첫번째 슬롯에서는 Proposer가 Builder들이 제출한 블록 헤더를 bid에 따라 선택하여 블록 헤더를 포함한 Beacon Block을 공개 후 하나의 위원회가 공개된 Beacon Block을 검증하고, 두번째 슬롯에서는 선택된 Builder가 Beacon Block의 검증 서명과 자신이 제출한 블록 바디를 공개 후 나머지 위원회가 해당 임시 블록(Builder Block)을 검증하고 해당 검증 서명을 집계 후 Beacon Block에 임시 블록(Builder Block) 포함하여 블록을 연결하고, 이후 Builder들은 다시 자신이 생성한 블록 헤더를 게시한다.

그러나 이러한 PBS 구조는 Builder가 트랜잭션을 검열할 수 있는 기능을 제공하며, 이를 방지하기 위해 DankSharding에서는 트랜잭션을 블록에 포함하는 데 Builder에게 전적으로 의존되지 않도록 PBS와 Proposer가 Builder가 포함해야 하는 트랜잭션 목록을 일부 지정할 수 있는 crList를 함께 사용하는 hybrid PBS를 제안했다.





### hybrid PBS(PBS + crList)

---

crList는 Proposer가 지정하는 Builder가 포함해야 하는 트랜잭션 목록으로, Builder가 트랜잭션을 검열하는 것을 방지하는 아이디어이다.

이를 PBS에 적용하면 다음과 같이 

![crlists](../../images/2023-03-19-dankSharding/crlists.png)

1. Proposer는 crList와 crList 요약을 게시한다.
2. Builder는 블록헤더, bid와 함께 crList의 요약 해시를 제출함으로써 crList를 본 것을 증명한다.
3. Proposer는 bid에 따라 Builder가 제안한 블록 헤더를 선택한다.
4. Builder는 블록 바디를 게시하고 crList의 트랜잭션을 모두 포함했거나 블록이 가득 찼다는 증거를 제출한다.
5. 검증자들은 블록 바디를 검증한다.

동작하여 DankSharding에서는 이러한 모델을 통해 기존의 Beacon Block에 데이터 가용성 증명을 위한 하나의 큰 Blob형태의 Builder Block이 추가되어 블록 사이즈가 커짐에 따라 나타날 수 있는 블록 제안의 중앙화 현상을 방지하고자 하였다.





## 2-Dimensional KZG Scheme

2-Dimensional KZG Scheme은 DankSharding에서 다음과 같이

![j4afAvY](../../images/2023-03-19-dankSharding/j4afAvY-9632159.png)

각 샤드의 제안자들이 제안한 Blob 형태를 하나의 큰 Blob으로 재구성하는 과정을 단순화하기 위해 제안된 모델로, 블록을 생성하는 Builder가 다시 기존의 Blob들로 하나의 KZG commitment를 재계산하는 것이 아닌 기존 m개의 commitment를 Reed-Solomon Code를 활용하여 2m개의 commitment로 확장한다.

이는 다음과 같이 4개의 blob이 있다고 가정할 때,

![2d1](../../images/2023-03-19-dankSharding/2d1.png)

이에 대해 2차원 다항식을 활용하여 

![2d kzg scheme](../../images/2023-03-19-dankSharding/2d kzg scheme.png)

한 차원에서는 row, 한 차원에서는 해당 row의 데이터를 지나는 다항식을 정의하고, row에 따라 commitment도 확장됨으로써 다음과 같이 

![2d2](../../images/2023-03-19-dankSharding/2d2.png)

하나의 큰 Blob으로 재구성하고, DankSharding에서는 해당 Blob을 통해 다음과 같이 검증자가 행, 열 단위로 데이터를 샘플링하는 DAS를 수행하여 

![2d das](../../images/2023-03-19-dankSharding/2d das.png)

{: .align-center}

대용량 데이터에 대한 데이터 가용성 증명이 가능하도록 하였다.





# Summary & 향후 연구

---

따라서 정리하자면 DankSharding은

다음과 같이 각 샤드에서 올라온 Blob을 Builder가 2-Dimensional KZG Scheme을 통해 하나의 큰 Blob형태의 블록으로 만들어 헤더와 bid를 공개하고, 

![image-20230328180912135](../../images/2023-03-19-dankSharding/image-20230328180912135.png)

Proposer는 Builder들이 만든 블록 중 자신의 Beacon Block에 포함할 블록을 선택 및 포함하여 Beacon Block으로 제안함으로써,

![image-20230328180954034](../../images/2023-03-19-dankSharding/image-20230328180954034.png)

이렇게 생성된 Beacon Block에 대해 검증자들이 검증 전 blob형태의 Builder Block으로 DAS를 통해 데이터 가용성 증명을 할 수 있게 된다.

![image-20230328181142177](../../images/2023-03-19-dankSharding/image-20230328181142177.png)

이에 현재 연구 사항은 Proto-DankSharding이라고 불리는 EIP-4844로, Blob을 포함하는 트랜잭션 형식과 DankSharding을 도입하기 위한 로직을 연구 중이며, Blob을 포함하는 트랜잭션 형식은 다음과 같이

![image-20230329160941803](../../images/2023-03-19-dankSharding/image-20230329160941803.png)

{: .align-center}

기존의 트랜잭션 형식에 파란색 부분이 추가된 것으로, 각 요소는

- blobs: 트랜잭션 데이터에 대한 blob
- blob_kzgs: blobs에 대한 KZG Commitment
- blob_versioned_hashes: Version(1byte) + KZG commitment Hash(32bytes => 마지막 31bytes)
  - Version은 추후 다른 커밋 방식 사용 대비
  - EVM 연산 호환성을 위해 32bytes

로 구성되고, Blob을 포함하는 트랜잭션 형식에서 다음과 같이 

![img](../../images/2023-03-19-dankSharding/GgfLkvo.png)

EVM이 SignedBlobTransaction에만 접근가능하게함으로써 blobs, blob_kzgs의 가스비를 줄여, 롤업에서 기존에 Batch를 올리기 위해 사용하던 트랜잭션 영역인 calldata 대신 비교적 저렴하게 사용할 수 있도록 하였다. 또한, blobs와 blob_kzgs는 이더리움 전체 블록 용량이 과도하게 증가하는 것을 방지하고자 30일 이후 삭제되도록 하였다.

이러한 Blob을 포함하는 트랜잭션을 활용하여 DankSharding을 구동하기 위한 로직은 롤업에서 다음과 같이

![img](../../images/2023-03-19-dankSharding/EgS4xvn.png)

롤업의 시퀀서가 올린 Blob을 포함하는 트랜잭션은 이더리움의 트랜잭션 풀에 있다가 Proposer에 의해 선택되어 전달되고, Proposer는 Blob을 포함하는  트랜잭션의 SignedBlobTransaction부분은 Execution payload로, 나머지 부분은 Blobs sidecar로 나누어 이웃 검증인 노드에게 전파하고

이후 이더리움의 검증인 노드들은 Execution Payload 안의 blob_versioned_hashes가 Blobs sidecar의 blob_kzgs와 대응되는지 확인함으로써 commitment을 검증한 후, Execution Payload는 이더리움에 계속 저장되고, Blobs sidecar는 롤업의 검증 및 저장을 위해 롤업 검증자에게 전달되는 구조이다.

그러나 EIP-4844에서는 아직 DAS가 도입되지 않아 데이터 가용성 증명을 위해 전체 blob data를 다운로드해야하며, DankSharding의 PBS, 2-Dimensional KZG Scheme 또한 향후 연구과제로 남아있다.







<div class="notice">
  <h5>Reference</h5>
  1) <a>What you can do for Ethereum 2.0 a.k.a. sharding - Hsiao-Wei Wang</a>
  <br>
  2) <a>Dude, What's the Danksharding situation? Workshop - Dankrad Feist</a>
  <br>
  3) <a>https://ethereum-magicians.org/t/a-rollup-centric-ethereum-roadmap/4698</a>
  <br>
  4) <a>Thibault, Louis Tremblay, Tom Sarry, and Abdelhakim Senhaji Hafid. "Blockchain scaling using rollups: A comprehensive survey." IEEE Access (2022).</a>
  <br>
  5) <a>Lavaur, Thomas, Jérôme Lacan, and Caroline PC Chanel. "Enabling Blockchain Services for IoE with Zk-Rollups." Sensors 22.17 (2022): 6493.</a>
  <br>
  6) <a>https://members.delphidigital.io/reports/the-hitchhikers-guide-to-ethereum/#in-protocol-proposer-builder-separation</a>
  <br>
  7) <a>Scalability and Asynchronous Programming - Vitalik vbuterin</a>
  <br>
  8) <a>https://vitalik.ca/general/2021/01/05/rollup.html</a>
  <br>
  9) <a>https://github.com/ethereum/research/wiki/A-note-on-data-availability-and-erasure-coding</a>
  <br>
  10) <a>https://ethereum.org/en/developers/docs/data-availability/</a>
  <br>
  11) <a>Al-Bassam, Mustafa, Alberto Sonnino, and Vitalik Buterin. "Fraud and data availability proofs: Maximising light client security and scaling blockchains with dishonest majorities." arXiv preprint arXiv:1809.09044 (2018).</a>
  <br>
  12) <a>https://ethereum.org/en/developers/tutorials/merkle-proofs-for-offline-data-integrity/</a>
  <br>
  13) <a>https://hackmd.io/@vbuterin/sharding_proposal</a>
  <br>
  14) <a>https://ethresear.ch/t/blob-serialisation/1705</a>
  <br>
  15) <a>What are Reed-Solomon Codes? – vcubingx</a>
  <br>
  16) <a>https://dankradfeist.de/ethereum/2020/06/16/kate-polynomial-commitments.html</a>
  <br>
  17) <a>https://ethereum.org/en/layer-2/#what-is-layer-2</a>
  <br>
  18) <a>https://ethereum.org/en/developers/docs/scaling/optimistic-rollups/</a>
  <br>
  19) <a>https://ethereum.org/en/developers/docs/scaling/zk-rollups/</a>
  <br>
  20) <a>https://hyun-jeong.medium.com/h-3c3d45861ced</a>
  <br>
  21) <a>https://medium.com/a41-ventures/research-pbs%EC%97%90-%EA%B4%80%ED%95%9C-%EA%B0%9C%EB%A1%A0-fb2c38bc865e</a>
  <br>
  22) <a>https://ethereum.org/en/developers/docs/mev/</a>
  <br>
  23) <a>https://xangle.io/insight/research/635f8c1f6c4f32e243e4349c</a>
  <br>
  24) <a>https://ethresear.ch/t/two-slot-proposer-builder-separation/10980</a>
  <br>
  25) <a>Danksharding Workshop - Ethereum Foundation</a>
  <br>
  26) <a>Data availability commitments with distributed reconstruction thanks to 2d KZG comm. - Dankrad Feist</a>
  <br>
  27) <a>https://hackmd.io/@vbuterin/in_protocol_PBS</a>
  <br>
  28) <a>https://hackmd.io/@protolambda/eip4844-implementation</a>
  <br>
  29) <a>https://medium.com/slcf/ethereum-2-0-3-eip-4844-proto-danksharding-82df4ffec7e1</a>
  <br>
  30) <a>https://medium.com/a41-ventures/eip-4844-%EC%8B%9C%EB%A6%AC%EC%A6%88-2-proto-danksharding-a-deep-dive-7b734f0fdb92</a>
  <br>
</div>