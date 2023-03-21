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

- DankSharding 이란
- DankSharding 등장배경
- DankSharding 이전 연구
  - Data Availability Problem
  - DAS(Data Availability Sampling)
  - Erasure Coding
  - KZG commitment
- DankSharding
  - PBS
  - 2D KZG Scheme

- Summary & Advantages, 현 상황



# DankSharding이란

----

DankSharding이란 다음과 같이 이더리움이 확장성 개선을 위해 도입 예정이었던 VM 실행 데이터를 처리하는 데이터 샤딩에서 롤업 중점의 로드맵으로 전환함에 따라 롤업 데이터의 데이터 가용성 증명을 위해 제안된 샤딩 디자인이다. 

![image-20230319232717880](../../images/2023-03-19-dankSharding/image-20230319232717880.png)

이와 같이 샤딩 디자인의 역할이 실행 데이터 처리에서 데이터 가용성 증명으로 바뀌자

![image-20230315175327381](../../images/2023-03-19-dankSharding/image-20230315175327381.png)

왼쪽과 같이 기존의 샤딩은 실행 데이터 검증을 위해 검증자들은 각 샤드마다 해당 샤드의 모든 데이터를 확인함으로써 데이터 가용성을 확인했다면, DankSharding은 블록을 만드는 빌더를 따로 두고, 빌더는 Beacon 블록과 함께 각 샤드의 데이터로 데이터 가용성 증명을 위한 Blob이라는 형태의 데이터를 추가하여 하나의 큰 블록을 생성하고 빌더가 생성한 Blob을 통해 각 샤드의 검증자들이 데이터 가용성 증명을 효율적으로 검증할 수 있도록 하였다.





# DankSharding 등장 배경

---

DankSharding 이전의 샤딩은 이더리움의 확장성 솔루션으로, 다음과 같이 

![image-20230320001431002](../../images/2023-03-19-dankSharding/image-20230320001431002.png)

각 샤드마다 Main Chain의 State를 나누어 처리하기 때문에 검증 노드들은 자신이 소속한 샤드의 데이터만 검증하면 되어 데이터 처리량의 이점을 가질 뿐만 아니라, 각 노드가 검증을 위해 저장해야 할 데이터양이 분할되기 때문에 검증 노드의 하드웨어 요구사항을 줄여 더 많은 사람들이 블록체인에 참여하여 탈중앙화의 이점을 가질 수 있었다.

그러나 다음과 같이

![image-20230320002942785](../../images/2023-03-19-dankSharding/image-20230320002942785.png)

 외부 체인(Layer 2)에서 실행한 압축된 트랜잭션 모음과 증명(롤업 데이터)을 Ethereum의 트랜잭션 Calldata 영역에 게시함으로써 이더리움의 보안을 상속받는 솔루션인 롤업이 등장하고, 

- 현재 이더리움 블록당 가스 한도: 1250만개

- CallData의 바이트당 가스 비용: 16Gas

- 롤업 검증 데이터(서명이나 증명) 비용: 50만Gas(추정)

- 롤업에서 하나당 트랜잭션 용량: 12bytes

-  평균 블록 생성 시간: 13초

=>  **하나의 블록에서 롤업 검증 데이터를 제외한 트랜잭션이 들어갈 수 있는 용량****: (1250-50)****만****/ 16 = 75****만** **bytes**

=> **하나의 블록 안에 들어갈 수 있는 트랜잭션 개수****: 75****만****/12 = 62,500****개**

=> **TPS: 62,500/13 = ~4807TPS**

위와 같이 확장성 성능 지표인 TPS(Transaction Per Second)에서 우수한 결과를 보여주면서 이더리움은 롤업 중점의 로드맵을 예정했다.

이에 샤딩의 역할은 데이터 처리가 아닌, Layer 1에서 검증을 위한 롤업 데이터가 가용한가에 대한 데이터 가용성 문제에 중점을 두었고,  데이터 가용성 문제는 이더리움에서 롤업이 아니더라도 최신 블록 헤더에만 동기화하고 블록 전체 데이터를 가진 풀 노드에서 다른 정보를 요청하는 이더리움 노드인 라이트 클라이언트가 전체 노드로 부터 받은 데이터에 대한 가용성 증명을 연구하고 있었기에 이러한 개념을 도입하여 데이터 가용성 증명을 효율적으로 할 수 있는 샤딩 디자인인 DankSharding이 등장하였다.





# DankSharding 이전 연구

---



## Data Availability Problem


이더리움에서 최신 블록 헤더에만 동기화하고 블록 전체 데이터를 가진 풀 노드에서 다른 정보를 요청하는 이더리움 노드인 라이트 클라이언트는 풀 노드로부터 받은 데이터를 사기 증명을 통해 블록 유효성을 검증한다.

여기서 사기 증명이란 다음과 같이 데이터 C를 증명하고자 할 때, 

![image-20230320141049509](../../images/2023-03-19-dankSharding/image-20230320141049509.png)

루트값을 계산하기 위한 H(D), H(C-D), H(A-B), H(E-H)를 풀 노드로부터 제공받아 계산 결과값과 자신이 가진 블록 헤더인 H(A-H)값이 일치하는지를 통해 증명한다.

그러나 해당 상황에서 만약 악의적인 풀 노드가 H(E-H)를 제외하고 라이트 클라이언트에게 전송한다면, 라이트 클라이언트는 우선 전달받은 데이터로 유효하지 않은 검증을 할 뿐만 아니라 DoS와 같은 자원낭비 문제를 초래할 수 있다.

이와 같이 전체 데이터가 아닌 일부 데이터를 가진 노드가 현재 사용되는 데이터가 사용가능한 데이터인지 알 수 없는 문제를 데이터 가용성 문제라고 하며, 이를 Fisherman's Dilemma로 확장하면 다음과 같이 

![image-20230320141924079](../../images/2023-03-19-dankSharding/image-20230320141924079.png)

제 3자인 노드는 전체 데이터를 가지고 있지 않아, 악의적인 노드가 블록 안의 특정 데이터를 빼놓고 정상적인 노드에 의해 경보 및 적발되어 다시 정상 데이터를 올리는 Case 1과 정상적인 노드가 정상 데이터를 올리고 악의적인 노드가 거짓 경보를 발생시키는 Case 2를 구분할 수 없는 문제이다.

이를 해결하기 위해 이더리움에서는 DAS(Data Availability Sampling)을 고려했다.





## Data Availability Sampling(DAS)


DAS란 데이터 가용성 샘플링으로, 

![image-20230319182059567](../../images/2023-03-19-dankSharding/image-20230319182059567.png)

위와 같이 데이터 가용성 증명을 하는 클라이언트가 전체 데이터를 다운로드 받는 것이 아닌

![blob](../../images/2023-03-19-dankSharding/blob.png)

위와 같이 이더리움에서 데이터를 다루는 데 사용하는 RLP인코딩을 통해 직렬화된 데이터를 이진 데이터로 나타내어 임의의 인덱스를 여러 라운드에 거쳐 다운로드함으로써 통계적인 보증을 얻는 방법이다.

그러나 여전히 검열되지 않은 데이터의 확률이 존재하며, 이를 보완하기 위해 데이터를 확장하는 Erasure Coding과 데이터가 올바르게 확장되었는지 검증하는 KZG commitment를 같이 사용한다.



## Erasure Coding


Erasure Coding이란 원래 손실되거나 손상된 데이터를 재구성하는 데 사용할 수 있는 중복 데이터를 생성하여 데이터 손실이나 손상을 방지하는 데 사용되는 기술로, DAS에서는 원래 데이터를 확장함으로써 데이터 일부가 손실되어도 나머지 데이터로 원래 데이터를 복구할 수 있도록 하여 샘플링에 의해 검열되지 않은 데이터가 생길 확률을 줄인다.

Erasure Coding에는 Reed-Solomon Code를 활용하는데, Reed-Solomon Code는 

1. Lagrange Interpolation(라그랑주 보간)
2. Modular 연산

으로 구성되어,  원래 데이터를 통해 Lagrange Interpolation으로 원래 데이터가 지나는 다항식을 구하여 해당 다항식 위의 점을 확장 데이터로 사용하고 Modular 연산을 통해 확장 데이터 값을 제한함으로써, 연산을 단순화한다. 이는 원래 데이터가 지나는 다항식으로 동작하기 때문에 확장된 데이터 중 일부 데이터가 손실되더라도 원래 데이터의 길이만큼 데이터가 남아있다면, 원래 데이터를 복구할 수 있다.

따라서 만약 원래 데이터의 길이가 k, 확장된 데이터의 길이가 2k라고 가정할 때 원래 데이터 복구에 필요한 데이터는 k이상이기 때문에 악의적인 노드는 DAS에 의해 검열되지 않기 위해 k+1이상 데이터를 숨겨야한다. 이는 한 번의 라운드에서 데이터가 가용할 확률을 약 50%(**(k+1)/****2k** )로 낮추고, 예를 들어 30번의 라운드가 진행된다고 할 때, 확률은 **2^(-30)**(약 10억분의 1)로 DAS를 효과적으로 보완한다.



## KZG commitment


KZG commitment는 증명자가 commitment 값인 타원곡선 점을 검증자에게 보낸 후, 증명자는 작업 중인 다항식을 변경할 수 없기 때문에 이를 commitment라고 하며, 하나의 다항식에 대해서만 유효성 증명이 가능한 특성을 활용하여 증명자가 제공하는 데이터(확장된 데이터)가 같은 다항식 내에 존재하는지 확인한다.

동작은 

- 원래 데이터로 생성된 다항식: f(x) (라그랑주 보간)
- Commitment: C(f)

에서

1. 증명자는 commitment값인 C(f)를 배포
2. 검증자는 자신이 가지고 있는 데이터인 z가 f(x)에 존재하는지 확인하기 위해 f(z)=y 를 증명자에게 전송
3. 증명자는 검증자로부터 받은 f(z)=y와 commitment를 통해 증명 π(f,z)를 계산 후 증명 π(f,z)를 검증자에게 전송
4. 검증자는 C(f), π(f,z), z, y를 통해 f(z)=y 확인

으로 이루어지며, 이를 통해 확장된 데이터가 올바르게 확장되었는지 증명한다.





# DankSharding

---

DankSharding은 앞선 데이터 가용성 연구를 토대로, 



















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
  10) <a></a>
  <br>
  10) <a></a>
  <br>
  10) <a></a>
  <br>
  10) <a></a>
  <br>
  10) <a></a>
  <br>
  10) <a></a>
  <br>
</div>