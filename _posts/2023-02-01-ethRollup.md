---
title: "DankSharding의 등장 배경"
categories:
 - DankSharding
tags: [Ethereum 2.0, A rollup-centric ethereum roadmap, Rollup, Vitalik Buterin’s Endgame] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기





---

# DankSharding의 등장 배경

------



## Ethereum 2.0 기존의 로드맵

2018년 타이페이에서 열린 연구 워크숍에서 결정한 "Ethereum 2.0" 로드맵은 다음과 같다.

![e2 roadmap](../../images/2023-02-01-ethRollup/e2 roadmap.png)

{: .align-center}



위의 다이어그램은 로드맵의 각 단계별 내용을 보여주는데, 주요 요소는

- 비콘 체인 : 샤드 체인과 메인 체인 사이의 브리지 역할을 하며, 스테이커(PoS 블록의 유효성 검사자)를 관리한다.
  - Consensus-Layer
    - Random Number Generation
      - Selecting block proposer
      - Selecting notarization committee
- 샤드 체인 : 메인 체인을 64개로 분할하여 네트워크 부하를 분산한다.
  - Data-Layer
    - Only Data-Consensus
      - Block bodies are just blobs, no state root in the header
      - Data availabliity
- VM : 각 샤드가 자체적으로 트랜잭션을 처리하고 스마트 계약을 실행할 수 있게 하고, 교차 샤드 트랜잭션을 처리하기 위해 서로 통신할 수 있다.
  - State-Layer
    - Ethereum flavored WebAssembly(eWASM)



이 중 현재 진행 사항은 비콘 체인과 메인 체인이 병합된 "The Merge"로, 원래 작업 증명 메커니즘(PoW)에서 지분 증명 메커니즘(PoS)으로 업그레이드를 완료했다.



[The Merge](https://apfl99.github.io/danksharding/TheMerge/)





## A rollup-centric Ethereum roadmap

2020년 Vitalik Buterin은 "A rollup-centric Ethereum roadmap"을 게시하며, 롤업 중점의 이더리움 로드맵을 주장했다.

이러한 주장의 근거는 경제적 지속 가능성과 TPS이다.

경제적 지속 가능성은 Layer 2 프로토콜을 활용하는데에서 나오는 장점으로

1. Gitcoin Grants 또는 Ethereum Foundation과 같은 일반적인 공익 자금 조달 기관에서 처리할 수 있지만 이러한 메커니즘 규모는 이 수준의 자금 조달을 처리하기에 충분하지 않지만, 자체 토큰을 출시하는 Layer 2 프로젝트는 L2가 캡처한 미래 수수료 예측에 의해 자금 조달을 하면 충분하다.
2. 이더리움은 믿을 수 있을 만큼 중립적이어야 하는 필요성이 있어 프로토콜 내 공공재 자금 조달을 어렵게 하지만, L2는 자체 공공재 자금 조달 메커니즘을 가지고 있기 때문에 이더리움의 장기적인 경제적 지속 가능성을 위해 좋을 수 있다.
3. Layer 2의 플랫폼 프로젝트에서 자체적인 지역 경제의 활성화와 기술 자율성을 유지하는, 보다 다양한 사람들에게 명확한 기회를 제공한다.



TPS는 장기적으로 미래를 구상했을때, 

- 오늘날 이더리움은 ~15TPS를 가지고 있다.
- 모든 사람이 롤업으로 이동하면 곧 ~3000TPS가 된다.
- 1단계가 진행되고 롤업이 데이터 저장을 위해 eth2 샤드 체인으로 이동하면 이론상 최대 ~100,000TPS까지 올라간다.
- 결국 2단계가 진행되어 기본 계산 기능이 있는 eth2 샤드 체인을 가져와도 ~1000 ~ 5000TPS를 제공한다.

위와 같이 기존 로드맵에 따라 샤드 체인을 실행 계층으로 완료하더라도 샤드 체인을 롤업에서의 데이터 저장 공간으로 사용했을 때가 TPS가 더 높기 때문이다. 

또한, 기존 로드맵에 따라 샤딩 EVM 계산을 적용했을 때보다 샤딩 데이터 가용성을 사용했을 때, 샤딩된 EVM 계산의 부정직 다수 증명 검증에는 엄격하고 잠재적으로 위험한 두 에포크 동기 가정이 필요한 사기 증명이 필요하지만, 데이터 가용성 샘플링(ZKP 또는 다항식 약정으로 수행된 경우)는 비동기식에서 더 안전하기 때문에 더 적합하다고도 주장한다.



### Rollup

------

블록체인 시스템에서 각 네트워크 노드는 모든 트랜잭션을 검증해야 한다. 여기에는 많은 양의 컴퓨팅 성능, 저장 용량 및 시간이 필요한데, 롤업은 각 노드가 처리해야 하는 데이터의 양을 줄임으로써 트랜잭션을 검증하는 데 필요한 리소스와 시간을 줄이는 방법이다. 롤업은 메인 체인 외부에서 트랜잭션을 처리하는 행위자로 구성된 Layer 2 네트워크를 사용하여 제공되며, 트랜잭션 데이터는 Layer 1에 게시되는 배치로 롤업된다.

롤업 시스템은 Layer 1의 Smart Contract를 통해 구현되며, 롤업 시스템의 다양한 행위자(e.g., aggregators, sequencers, verifiers)는 이 Smart Contract와 상호작용한다. Aggregator는 해당 계약을 통해 트랜잭션 데이터 및 기타 정보(e.g., state root)를 게시할 수 있고, 검증자는 또한 필요할 때 거래에 대해 이의를 제기하는데 사용한다.

![thiba4-3200051-large](../../images/2023-02-01-ethRollup/thiba4-3200051-large.gif)

{: .align-center}

사용자는 롤업 Smart Contract로 거래하여 롤업에 참여하며, 먼저 자금을 입금한다. 이 자금은 사용자에게 비례 가치의 Layer 2 토큰의 양을 제공하고, 제공받은 토큰으로 사용자는 Layer 2에서 거래할 수 있다.

Layer 2에서 거래하기 위해 사용자는 직접하거나 Layer 1 Smart Contract를 통해 Aggregators에게 거래를 보낸다. 그런 다음 Aggregator는 받은 트랜잭션 집합을 선택하고 실행한 다음 압축된 트랜잭션 데이터(Rollup Data)를 Layer 1에 게시한다. 

이후 롤업 데이터는 공개되고 변경할 수 없게 되고, Layer 1의 토큰을 회수하기 위해 사용자는 Smart Contract로 거래하고 Layer 1 수수료를 한 번 더 지급한다.



**롤업 데이터**

![diag1](../../images/2023-02-01-ethRollup/diag1.png)

{: .align-center}

롤업에서는 트랜잭션 데이터를 위 그림과 같이 State Root로 압축 및 저장하는데, 

![diag2](../../images/2023-02-01-ethRollup/diag2.png)

{: .align-center}

검증을 위해 이전 상태 루트 및 새 상태 루트와 함께 고도로 압축된 트랜잭션 모음인 Batch를 게시한다. 이 때, Smart Contract는 Batch의 이전 상태 루트 및 현재 루트와 일치하는 지 확인하고 상태 루트를 새 상태 루트로 전환한다.

여기서 새 상태 루트에 대한 검증이 필요한데, 이 검증의 방법에 따라 사기 증명을 사용하는 Optimistic Rollup과 유효성 증명을 ZK Rollup으로 나뉜다.



#### Optimistic Rollup

------

Optimistic Rollup에서 Aggregators는 Off Chain Tx를 실행하고 트랜잭션 데이터와 새로운 상태 루트를 게시한다. 여기에는 유효성 증명은 포함되어 있지 않으며 계층 1 및 2 노드는 이 행위자가 실행한 계산이 유효하다는 가정한다. 따라서 Main Chain의 노드는 새로운 Batch가 게시될 때마다 모든 트랜잭션을 처리할 필요가 없어 Layer 1의 노드 부하가 줄어든다.

Optimistic Rollup에는 Aggregators와 Verifier라는 행위자가 존재하는데, Aggregators는 Layer 2에서 직접 또는 체인 간 스마트 계약을 통해 트랜잭션을 수집한다. 이후 Aggregators는 임의의 수의 트랜잭션을 선택하여 처리하고 압축된 트랜잭션 데이터와 상태 루트를 Layer 1 체인에 게시한다. 이때, 검증자는 집계자가 게시한 데이터를 지속적으로 모니터링하고 검증자가  트랜잭션이 실행될 때, 게시된 상태 루트로 이어진다는 데 동의하지 않으면 분쟁 단계를 시작한다. 

이 분쟁 단계에서 Aggregators는 부정직한 행위를 하고, Verifier가 사기 증명을 통해 Aggregators의 부정적한 행위를 증명하면 Aggregator의 채권 절반을 보상으로 받고, 채권의 나머지 절반은 소각된다.  반대로 분쟁 단계에 진입하지 않은 경우에는 일정시간이 지나면 거래의 완결성이 보장된다.



##### 분쟁 단계 : 사기 증명

------

![thiba6-3200051-large](../../images/2023-02-01-ethRollup/thiba6-3200051-large.gif)

![thiba7-3200051-large](../../images/2023-02-01-ethRollup/thiba7-3200051-large.gif)

첫 번째 그림이 유효한 상태 트리이고, 두 번째 그림과 같이 사기 트랜잭션이 발생했을 때, 검증자는 로컬 상태에서 게시된 트랜잭션을 시뮬레이션하고 결과 상태 루트를 비교한다. 또한 불일치가 발견될 경우 검증자가 사기 증명을 트리거한다.

사기 증명은 배치 자체(체인에 저장된 해시와 비교) 및 읽은 특정 계정을 증명하는 데 필요한 Merkle 트리의 일부(계정 주소 및 잔액나 스택 데이터)로 배치를 시뮬레이션하고 사후 상태 루트를 계산함으로써 진행되며,  계산된 사후 루트와 제공된 사후 상태 루트가 동일하지 않으면 사기 증명이 생성된다. 



#### ZK Rollup

------

ZK Rollup은 Optimistic Rollup과 다르게 Aggregator는 실행한 계산이 유효하다는 가정을 하지 않고, 트랜잭션 데이터와 함께 게시된 상태 루트가 정확하다는 증거를 제출한다. 이 증거는 ZK(Zero-Knowledge)를 활용한 암호화 도구를 사용하여 제작되며, Zero-Knowledge는 계산에 필요한 입력 데이터가 설득력을 갖기 위해 검증자와 공유할 필요가 없음을 의미한다. 따라서 검증자가 모든 계산을 스스로 수행하지 않고도 증명(Zero-Knowledge Proof)이 가능하다.



##### ZKP(Zero-Knowledge Proof)의 Property

---

Completeness : 어떤 조건이 참이라면, honest verifier는 honest prover에 의해 이 사실을 납득할 수 있다.

Soundness : 어떤 조건이 거짓이라면, dishonest prover는 거짓말을 통해 verifier에게 조건이 참임을 절대 납득시킬 수 없다.

Zero-Knowledge : 어떤 조건이 참일 때, verifier는 이 조건이 참이라는 사실이외의 정보를 아무것도 알 수 없다.



##### ZKP(Zero-Knowledge Proof) formal definition 

------

- Prover : 자신이 가지고 있는 정보가 무엇인지 공개하지 않고, Verifier에게 정보를 알고 있다는 것을 증명하고 싶은 참여자
- Verifier : Prover가 해당 정보를 가지고 있음을 검증하고 싶은 참여자
- Secret : Prover가 가지고 있음을 증명하고 싶은 정보이며, 모두에게 숨기고자 하는 정보
- Challenge : Verifier가 Prover가 Secret을 가지고 있는지 확인하기 위해 문제를 내는 과정
- Statement is true : Verifier가 Prover가 Secret을 가지고 있음을 검증한 상태



![1__uEWsd2qtyDbh6stbw86EQ](../../images/2023-02-01-ethRollup/1__uEWsd2qtyDbh6stbw86EQ.webp)

- P(x) : Prover
- V(x, z) : Verifier
- <--> : Prover와 Verifier의 Challenge 과정
- View : interactive proof 과정을 관찰하여 기록한 것
- z : Verifier의 Challenge Value

위 정의를 보면, Prover의 Secret(x)에 대하여 Verifier가 z값에 따라 challenge하면서 해당 증명 과정을 기록한다. 이는 계속해서 Prover가 Challenge과정에서 올바른 답을 제공했다면, Verifier는 확률적으로 Prover가 secret을 가지고 있다고 확신할 수 있다.



##### 이산 로그를 활용한 ZKP 구현

-----

**이산 로그 문제(Discrete Logarithm Problem, DLP)**

- "g, x, p를 가지고 y = g^x(mod p)를 구하는 것은 쉽지만, g, y, p를 가지고 x를 구하는 것은 어렵다" 라는 문제



**ZKP 구현**

- y: 주어진 값, p: larger prime(소수), g: generator

- Prover는 g^x(mod p) = y가 되는 x값(secret)을 알고 있으며, y값은 x값을 통해 구할 수 있다.
- Prover는 사전에 y값을 Verifier에게 공유한다.
- Prover는 모든 Verifier에게 x값을 알고 있다고 증명하고 싶지만, x값은 노출하고 싶지 않다.

이 상황에서 Prover는 다음과 같은 과정을 통해 Verifier에게 secret 정보를 알리지 않고, secret을 가지고 있음을 증명할 수 있다.

1. Prover는 y = g^x(mod p)를 계산하여 Verifier에게 y값을 준다.
2. Prover는 C = g^r(mod p)를 계산하여 C를 Verifier에게 주고, r값은 공개하지 않는다. (r : random number)
3. Verifier는 Prover에게 r을 요청하거나, (x+r)(mod(p-1))을 요청한다. (Challenge 0 or 1)



- Verifier가 r을 요청한 경우 Verifier는 C는 알고 있기 때문에 r값을 통해 C를 계산하여 값을 비교하고 Prover가 secret을 가지고 있음을 증명할 수 있다.

![1_9rnL987k8cgjw3UiyTsoMg](../../images/2023-02-01-ethRollup/1_9rnL987k8cgjw3UiyTsoMg-6013212.webp)

{: .align-center}

- Verifier가 (x+r)(mod(p-1))을 요청한 경우, 1번 2번을 통해 Verifier는 y값과 C값을 알고 있기 때문에 Verifier는 아래 식을 통해 Prover가 secret을 가지고 있음을 증명할 수 있는 동시에, (x+r)(mod(p-1))을 통해 Prover가 가진 x값을 유추하기 어렵게 만들 수 있다.

![1_7yXNpiZqux4HoF7eoCpdUA](../../images/2023-02-01-ethRollup/1_7yXNpiZqux4HoF7eoCpdUA-6013230.webp)

{: .align-center}



**위 식 증명(===을 합동기호 대신)**

1. a === b(mod m)이고 c === d (mod m)이면 ac = bd(mod m)이다.(모듈러 연산 곱셈 성질)
   - (C * y) = (g^x * g^r)(mod p)
   - (C * y) (mod p) = (g^x * g^r) (mod p)(mod p) = (g^x * g^r) (mod p) =>mod 연산 두 번은 한 번과 같음.
2. 모듈러 연산은 p를 주기로 해서 순환군을 가지기 때문에, 거듭 연산시 지수의 합에 mod(p-1)을 취하면 결과가 같다.

![p1](../../images/2023-02-01-ethRollup/p1.png)

{: .align-center}

- 위는 g가 2이고 p가 13인 요소와 거듭 연산 결과인데, 이는 p-1, 즉 12의 배수마다 항등원이 된다. (지수 e가 12의 배수라면 g^e는 항상 1)
- 따라서 p-1 = 12를 주기로 계속해서 계속 순환되기 때문에 g^a * g^b = g^((a+b)(mod p-1))이 성립된다.
  - 그러므로 1번을 대입하면  (g^x * g^r) (mod p)  = g^((x+r)(mod p-1)) (mod p) 이므로 2번에 의해 증명식은 성립된다.

![prf](../../images/2023-02-01-ethRollup/prf.jpg)

{: .align-center}



##### 트랜잭션 데이터 압축

------

원래 ETH 전송 트랜잭션은 ~110bytes를 사용하지만, Rollup에서의 트랜잭션은 ~12bytes를 사용할 수 있다.

| Parameter | Ethereum               | Rollup |
| :-------- | :--------------------- | :----- |
| Nonce     | ~3                     | 0      |
| Gasprice  | ~8                     | 0-0.5  |
| Gas       | 3                      | 0-0.5  |
| To        | 21                     | 4      |
| Value     | ~9                     | ~3     |
| Signature | ~68 (2 + 33 + 33)      | ~0.5   |
| From      | 0 (recovered from sig) | 4      |
| Total     | ~112                   | ~12    |

- Nonce : Nonce는 이중 지불 방지 문제를 해결하기 위한 파라미터이다. 그러나 롤업의 경우 이전 상태로부터 Nonce를 복원할 수 있기 때문에 전체 Nonce를 생략할 수 있으며, 만약 누군가 이전 논스로 이중 지불을 시도하여도, 최근의 논스를 포함하는 데이터와 대조하여 서명이 검증되기 때문에 해당 서명 검증은 실패한다.
- Gasprice : 이는 사용자에게 고정된 범위의 가스비를 지불하도록 하거나 아예 가스 지불을 롤업 프로토콜 외부로 옮겨서 사용자들이 별도 채널을 통해 배치 생성자들에게 지불할 수 있도록 할 수 있다.
- Gas : Gasprice와 비슷하다.
- To : 20bytes의 주소를 트리의 인덱스로 대체할 수 있다.
- Value : 과학적 표기법(너무 크거나 작은 수를 십진법으로 편하게 작성하여 표현하는 방법)으로 값을 저장할 수 있다. 대부분의 경우 전송에는 1-3개의 유효 숫자만 필요하다.
- Signature : BLS 집계 서명을 사용할 수 있다. 다수의 서명을 32~96bytes(프로토콜에 따라)의 단일 서명으로 결합할 수 있으며, 이 서명은 배치 내 전체 메시지와 발신자 집합에 대해 한꺼번에 검증 가능하다.



##### 롤업의 확장성 이점

------

기존 이더리움 체인에서 가스 한도는 1,250만개이며 트랜잭션의 1byte 비용은 16 Gas이다. 즉 블록에 단일 배치만 포함된 경우(zk rollup 증명 검증 : 50만 gas 가정), 해당 배치는 (1250-50)만 / 16 = 75만 bytes의 데이터를 가질 수 있다. 트랜잭션 데이터 압축에서 말한 것처럼 ETH 전송을 위한 롤업에는 사용자 작업당 12bytes가 필요하므로, 배치에 최대 75만/12 = 62500개의 트랜잭션이 포함될 수 있다. 이는 현재 13초의 평균 블록 시간에서 ~4807 TPS로 변환된다.

| Application                                       | Bytes in rollup                                              | Gas cost on layer 1                                          | Max scalability gain |
| :------------------------------------------------ | :----------------------------------------------------------- | :----------------------------------------------------------- | :------------------- |
| ETH transfer                                      | **12**                                                       | 21,000                                                       | 105x                 |
| ERC20 transfer                                    | **16** (4 more bytes to specify which token)                 | ~50,000                                                      | 187x                 |
| Uniswap trade                                     | **~14** (4 bytes sender + 4 bytes recipient + 3 bytes value + 1 byte max price + 1 byte misc) | ~100,000                                                     | 428x                 |
| Privacy-preserving withdrawal (Optimistic rollup) | **296** (4 bytes index of root + 32 bytes nullifier + 4 bytes recipient + 256 bytes ZK-SNARK proof) | [~380,000](https://etherscan.io/tx/0x6e311f84655af72614966705584569b52d6e314f2d61b965db91db41fd01b1e1) | 77x                  |
| Privacy-preserving withdrawal (ZK rollup)         | **40** (4 bytes index of root + 32 bytes nullifier + 4 bytes recipient) | [~380,000](https://etherscan.io/tx/0x6e311f84655af72614966705584569b52d6e314f2d61b965db91db41fd01b1e1) | 570x                 |



여기서 eth2 데이터 샤딩이 도입되게 된다면, 초당 ~1398kb/s로 기존 이더리움 체인의 ~60kb/s에서 23배 개선되어 최대 100,000 TPS까지 처리할 수 있을 것으로 예측된다.





## Data Sharding





















<div class="notice">
    <h5>Reference</h5>
    1) <a>https://docs.google.com/presentation/d/1G5UZdEL71XAkU5B2v-TC3lmGaRIu2P6QSeF8m3wg6MU/edit#slide=id.g3bd8f42e24_0_167</a>
    <br>
    2) <a>https://ethereum-magicians.org/t/a-rollup-centric-ethereum-roadmap/4698</a>
    <br>
    3) <a>https://ieeexplore.ieee.org/abstract/document/9862815</a>
    <br>
    4) <a>https://vitalik.ca/general/2021/01/05/rollup.html</a>
    <br>
    5) <a>https://hyun-jeong.medium.com/h-3c3d45861ced</a>
    <br>
    6) <a>https://medium.com/decipher-media/zero-knowledge-proof-chapter-1-introduction-to-zero-knowledge-proof-zk-snarks-6475f5e9b17b</a>
    <br>
    7) <a>http://blog.onther.io/ethereum/scalability/zkp/Zero-Knowledge-Proof-step1/</a>
<div>

​	

