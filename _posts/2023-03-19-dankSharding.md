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



목차

- DankSharding 등장배경 
  - Sharding
  - Rollup

- DankSharding 이란
- DankSharding 관련 연구
  - Data Availability
    - Data Availability Problem
    - DAS(Data Availability Sampling)
    - Erasure Coding
    - KZG commitment

  - Rollup
    - optimistic Rollup
    - ZK Rollup

- DankSharding
  - PBS
  - 2D KZG Scheme

- 현 상황



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

데이터 가용성 측면에서 봤을 때, 기존의 샤딩은 왼쪽과 같이 실행 데이터 검증을 위해 위원회의 검증자들은 각 샤드마다 해당 샤드의 모든 데이터를 다운로드함으로써 데이터 가용성을 확인했다면, 

DankSharding은 블록을 만드는 빌더를 제안자와 분리하여 따로 두고, 빌더는 Beacon 블록과 함께 각 샤드의 Batch로 이더리움의 데이터 가용성 증명을 위한 연구를 활용한 Blob이라는 형태의 데이터를 추가하여 하나의 큰 블록을 생성하고 빌더가 생성한 Blob을 각 샤드 검증자들로 구성된 위원회가 데이터 가용성 증명을 효율적으로 검증할 수 있도록 하였다.





# DankSharding 관련 연구

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

그러나 여전히 검열되지 않은 데이터의 확률이 존재하며, 이를 보완하기 위해 데이터를 확장하는 Erasure Coding과 데이터가 올바르게 확장되었는지 검증하는 KZG commitment를 같이 사용한다.





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

이는 한 번의 라운드에서 데이터가 가용할 확률을 약 50%(**(k+1)/****2k** )로 낮추고, 예를 들어 30번의 라운드가 진행된다고 할 때, 확률은 **2^(-30)**(약 10억분의 1)로 DAS를 효과적으로 보완한다.





### KZG commitment

---

KZG commitment는 Prover가 commitment 값인 타원곡선 점을 Verifier에게 보낸 후, Prover는 작업 중인 다항식을 변경할 수 없기 때문에 이를 commitment라고 하며, 하나의 다항식에 대해서만 유효성 증명이 가능한 특성을 활용하여 Erasure Coding으로 확장된 데이터가 같은 다항식 내에 존재하는지 확인한다.

KZG commitment에는 암호학적 성질을 부여하기 위해 이미 정의된 페어링된 타원곡선을 사용하는데, 이는 다음과 같이 이미 정의된 페어링된 타원곡선의 성질을 이용하여, 

![kzgcommitment](../../images/2023-03-19-dankSharding/kzgcommitment.png)

Verifier는 Prover로부터 Commitment와 Proof를 받으면, 자신이 증명하는 데이터 z가 f(x)위에 존재하는지 증명할 수 있도록 할 수 있게 함으로써, Erasure coding에서 확장된 데이터가 올바르게 확장되었는지 증명이 가능하도록 한다.

이를 수식으로 나타내면, 

- Erasure Coding과 같은 다항식: P(x)
- 이미 정해진 다항식의 Generate Point: [G]<sub>1</sub>, [G]<sub>2</sub>
- 비밀 s를 서로 페어링된 타원곡선 1, 2에 대응시킨 점: [s]<sub>1</sub>,[s]<sub>2</sub> (이후 s는 파기)

가 먼저 설정되고, 다음과 같이 진행된다.

![kzg detail](../../images/2023-03-19-dankSharding/kzg detail.png)

![kzg detail6](../../images/2023-03-19-dankSharding/kzg detail6.png)





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

영지식 증명이란 정보를 공개하지 않고, 정보의 유효성을 증명할 수 있는 방법으로, 다음과 같은 요소로 구성된다.

- Prover: 자신이 가진 정보를 공개하지 않고, Verifier에게 자신이 정보를 알고 있다는 사실을 증명하고 싶은 참여자
- Verifier: Prover가 해당 정보를 알고 있음을 검증하고 싶은 참여자
- Secret: Prover가 알고 있다는 것을 증명하고 싶은 정보이자, 공개하지 않는 정보
- Challenge: Verifier가 Prover에게 Secret을 가지고 있는지 확인하기 위해 문제를 내는 과정

이를 대표적인 예시인 알리바바 동굴로 예를 들면,

![img](../../images/2023-03-19-dankSharding/1*bLQHTNQfqhn-cl6nEJpX2Q.png)

- Prover: 동굴 밖에서 A 혹은 B로 나오라고 Challenge를 하는 사람
- Verifier: 동굴 안에서 Challenge에 따라 나옴으로써 자신이 동굴 안의 문을 열 수 있는 비밀을 알고 있다는 것을 증명하고자 하는 사람
- Secret: 동굴 안의 문을 열 수 있는 비밀
- Challenge: Prover가 Verifier에게 A 혹은 B를 요구하는 과정

Prover는 Verifier에게 여러 번에 거쳐 Challenge를 함으로써 확률적으로 Verifier가 Secret을 알고 있음을 증명할 수 있다.

따라서 이러한 점을 활용하여 zk Rollup에서는 증명을 위해 모든 트랜잭션 데이터를 게시하는 Optimistic Rollup과 달리, 

- Prover: Layer 1의 Smart Contract
- Verifier: Aggregator
- Secret: 트랜잭션 데이터

의 구조를 가지고, Aggregator는 Layer 1의 Smart Contract에 게시하기 전, 트랜잭션 데이터에 대한 유효성 증명(Merkle Proof)을 수행하고, Pre-State Root로부터 트랜잭션 데이터가 포함되어 Post-State Root가 도출되었다는 영지식 증명을 게시하고 이를 활용하여 Pre-state Root로부터 Post-state Root가 유효한지 증명한다.





# DankSharding

---

DankSharding은 앞선 연구를 토대로, 다음과 같은 구조를 갖는데

![image-20230321112508272](../../images/2023-03-19-dankSharding/image-20230321112508272.png)

이는 Beacon 블록과 함께 각 샤드의 데이터로 데이터 가용성 증명을 위한 Blob이라는 형태의 데이터를 추가하여 하나의 큰 블록을 생성하고, 각 샤드의 검증자들이 Blob을 통해 각 샤드의 검증자들이 데이터 가용성 증명을 검증한다.

이러한 구조가 도입되기 위해 DankSharding에서는 두가지 개념을 도입하는데, 첫번째는 기존의 블록에서 Blob 데이터가 추가되어 블록 사이즈가 커지고 블록을 제안하기 위한 노드의 하드웨어 요구사항이 증가함에 따라, 블록 제안의 중앙화를 방지하기 위해 블록을 생성하는 Builder와 블록을 제안하는 Proposer를 따로 두는 PBS(Proposer-Builder Separation)과 두번째는 각 샤드의 데이터가 합쳐진 블록의 데이터에 대한 데이터 가용성 증명이 요구되어, 대용량 데이터 가용성 증명을 위해 Blob을 만드는 빌더가 해당 데이터에 대한 하나의 KZG commitment를 재구성하는 과정에서 생기는 과도한 연산을 피하기 위해 Erasure Coding을 통해 m개의 commitment를 2m으로 확장하는 2-Dimensional KZG Scheme가 있다.



## PBS(Proposer-Builder Separation)





## 2-Dimensional Scheme



















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
  21) <a></a>
  <br>
  22) <a></a>
  <br>
</div>