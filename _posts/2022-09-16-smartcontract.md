---
title: "스마트 컨트랙트"
categories:
 - BEB
tags: [스마트 컨트랙트, 비트코인에서의 스마트 컨트랙트, 이더리움에서의 스마트 컨트랙트, 프라이빗 블록체인에서의 스마트 컨트랙트] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기





---



# 스마트 컨트랙트

------

#### 특징

- 스마트 컨트랙트는 중개인의 존재 여부와 상관없이, 조건이 충족된다면 계약이 이행되고, 조건이 충족되지 않는다면 이행되지 않는다
  - 이러한 특성은 블록체인의 탈중앙성과 적합하고, 중개인이 없기 때문에 거래 수수료를 절감할 수 있다.
- 블록체인의 특성상 배포 및 자동화는 누구든지 가능하며, 검증도 제약없이 자유로우며 변경이 불가하고 거래 내역이 투명하다.
- 스마트 컨트랙트는 코드에 의해 실행되기 때문에 코드에 따라 계약이 실행됨을 보장하는 결정론적인 상태이다.



#### 장점

- 보안
  - 스마트 컨트랙트는 블록체인 위에서 실행되기 때문에, 모든 노드가 스마트 컨트랙트 내용과 이행 결과를 가짐에 따라 계약에 대한 중앙화된 공격 지점이 없고, 계약 내용이나 결과를 변조할 위협이 없다.
- 신뢰성
  - 스마트 컨트랙트 로직이 조건에 부합해 계약이 이행되면 블록체인 네트워크에 있는 노드들에 의해 여러 번 수행되고 검증되기 때문에, 위변조가 매우 어렵고 정확도가 높다.
- 공평함
  - 블록체인으로 계약 조건을 공유하고 강제하기 때문에 특정 이득을 취하는 중개자가 없다.
- 효율성
  - 계약 이행의 자동화로 계약의 프로세스 검증이 필요 없다.



#### 한계점

- 컨트랙트 배포 이후, 컨트랙트가 보안적 취약점을 가진다는 등 코드의 문제가 있더라도 수정할 수 없다.
- 컨트랙트 코드는 블록체인 안에서 동작하기 때문에 외부 데이터를 스스로 취득할 수 없다.



## 비트코인에서의 스마트 컨트랙트

비트코인에서는 UTXO로 코인을 관리한다. 여기서 UTXO가 동작하는 과정이 스마트 컨트랙트로 이루어진다.



### UTXO 동작 원리

------



#### 비트코인 트랜잭션 구조

![i1](../../images/2022-09-16-smartcontract/i1.png)

{: .align-center}

- 트랜잭션 버전(Transaction Version) : 트랜잭션 유형을 지정하는 버전 번호로, 노드가 트랜잭션을 읽을 때, 버전 번호를 확인하고 트랜잭션을 어떻게 읽어야 하는지 파악한다.
- 잠금 시간(Lock Time) : 잠금 시간으로, 트랜잭션은 잠금 시간이 지나야 포함할 수 있다.
- 입력(Input)과 출력(Output)
  - 입력 : 입력은 포인터(Pointer)와 해제키(Unlocking)가 들어있으며, 여기서 포인터는 이전 트랜잭션 출력을 가리키고, 키는 포인터로 가리키고 있는 이전 출력을 해제하는 데 사용한다.
  - 출력 : 출력은 잠금(Lock)과 값(Value)로 구성되어 있으며, 입력에서 잠금을 해제할 수 있는 키가 존재하며, 값은 출력 내에 잠겨 있는 사토시의 양을 의미한다.



#### UTXO

트랜잭션 구조를 보면 결국 입력과 출력으로, 출력에는 자산이 잠겨있고, 입력으로 출력을 해제하여 값을 꺼내, 사용한 후, 새로운 출력에 자산을 담는다. 여기서 입력에 의해 해제되지 않은 트랜잭션을 UTXO라고 하며, 말 그대로 미사용 트랜잭션 출력값이다.

이를 그림으로 나타내면 다음과 같다.

![i2](../../images/2022-09-16-smartcontract/i2.jpeg)

{: .align-center}

이러한 UTXO 모델은 확장성이 좋고, 트랜잭션 로직이 단순하기 때문에 병렬적으로 검증이 가능하다.



### 비트코인 스크립트

------

비트코인에서는 스크립트라는 스크립터 언어를 사용해 스마트 컨트랙트를 구현한다. 스크립트에서 사용하는 연산들은 Opcode에 해당하며, 스크립트는 트랜잭션에 연결되어 있어 네트워크의 모든 노드는 트랜잭션을 받을 때마다 자신의 로컬 컴퓨터에서 트랜잭션에 연결된 스크립트를 실행하여 이를 통해 비트코인 송금이 이루어진다.

결국 스크립트는 입출력에 의한 UTXO 잠금, 해제 메커니즘이다.



#### 입출력 구조

![i3](../../images/2022-09-16-smartcontract/i3.png)

{: .align-center}

- Input Structure
  - Prev. Tx ID, TxIndex : 해제하고자 하는 이전 출력을 가리키는 포인터
  - ScriptSig : 포인터가 가리키는 출력을 해제하는 키
- Output Structure
  - ScriptPubkey : 잠금, ScriptPubKey의 소유자만이 잠금을 해제하는 키인 ScriptSig를 생성할 수 있다.
  - Amount : 잠긴 비트코인의 양(단위 : 사토시)



#### 스크립트 동작 원리

스크립트는 스택 기반 튜링 불완전 언어이다. 즉, 스택 구조로 동작하며, 불완전 튜링이기 때문에 간단한 연산만 가능하다.(반복문 등은 불가능)

스크립트는 

- Opcode : 덧셈, 뺄셈, 곱셈과 같은 연산 작업
- 데이터 : OP_CODE가 아닌 모든 데이터로, 스택에 들어가게 된다.

의 두 종류 객체를 가지며, 스크립트는 이 두 종류의 객체를 일렬로 늘어놓은 것이다.

동작은 포인터가 일렬로 늘어진 Opcode와 데이터를 순서대로 가리키며, 포인터가 데이터를 가리킬 경우, 스택에 데이터를 넣고 opcode를 가르킬 경우 스택에서 데이터를 꺼내온다(FILO).

따라서 노드는 새로운 트랜잭션에 대해서 Script와 ScriptPubkey 필드를 추출하여 ScriptSig +ScriptPubKey의 스크립트 형태를 얻게 되며 해당 스크립트와 빈 스택 하나를 사용해 스크립트를 실행하고 실행 결과, 최상위 스택 요소가 1일 경우 트랜잭션이 유효하다 판단하고 주변 노드에게 전파하고 유효하지 않은 경우 전파하지 않는다.



#### 비트코인 스크립트 : Pay To PubKey(P2PK)

P2PK는 가장 간단한 종류의 스크립트로, 

![i4](../../images/2022-09-16-smartcontract/i4.png)

{: .align-center}

위와 같이 해제 키를 정의하는 데이터(입력)인 ScriptSig와 잠금을 정의하는 데이터(출력)인 ScriptPubkey, Opcode로 구성되어 있다. 동작은 다음과 같다.

1. 빈 스택에서 포인터가 서명(signature)를 가리킨다.
2. 서명(signature)은 데이터이므로 스택에 들어간다. 포인터는 그 다음 요소인 공개 키(public key)를 가리킨다.
3. 공개키는 데이터이므로 스택에 넣는다. 포인터는 그 다음 요소인 OP_CHECKSIG를 가리킨다.
4. OP_CHECKSIG는 스택에서 공개키와 서명을 꺼내고 ECDSA 알고리즘을 사용해 서명을 검증한다.
   - 성공 시, 스택에 1을 넣고, 실패 시 스택에 0을 넣는다.
5. 스택의 최종값이 1일 경우 트랜잭션 검증이 완료되고 UTXO가 해제되고, 0일 경우 여전히 잠금 상태이다.

그러나 해당 스크립트의 경우 수신자의 public key가 그대로 노출되기 때문에 보안에 취약하며, 오늘날에는 사용되지 않는다.



#### PayToPubKeyHash(P2PKH)

P2PKH는 P2PK에서 수신자의 공개 키 공개 문제를 개선한 스크립트로, ScriptPubkey가 공개키의 해시값을 갖는다.

![i6](../../images/2022-09-16-smartcontract/i6.png)

{: .align-center}

위의 스크립트에서는 ScriptSig에 서명과 공개키가 존재하고, ScriptPubKey에는 여러 개의 Opcode와 수신자의 공개키를 해싱한 Hash 1 객체가 존재한다. 동작은 다음과 같다.

1. 빈 스택에서 포인터는 서명(signature)를 가리킨다.
2. 서명(signature)는 데이터이기 때문에 스택에 들어가고, 포인터는 다음 요소인 공개키(public key)를 가리킨다.
3. 공개키(public key)도 데이터이기 때문에 스택에 들어가고, 포인터는 다음 요소인 OP_DUP를 가리킨다.
4. OP_DUP는 스택 최상단 요소를 복사하는 Opcode로, 스택에 공개키(public key)가 하나 더 들어가고, 포인터는 다음 요소인 OP_HASH160을 가리킨다.
5. OP_HASH160은 스택 최상단 요소를 해싱하는 Opcode로, 최상단에 있는 공개 키가 해싱되어 Hash2로 들어가고, 포인터는 다음 요소인 Hash 1을 가리킨다.
6. Hash1은 데이터이기 때문에 스택에 들어가고, 포인터는 다음 요소인 OP_EQUALVERIFY를 가리킨다.
7. OP_EQUALVERIFY는 스택에 있는 두 요소가 같은지 확인하는 Opcode로 만약 두 요소가 같다면 해당 요소 두 개를 제거하고, 다르다면 실행에 실패한다. 즉 ScirptSig(입력)에 올바른 공개 키가 있었다면 ScriptPubkey에 들어있던 Hash 1과 공개키를 해싱한 값인 Hash 2가 동일하고, 스택에서 제거된다. 이후 포인터는 다음 요소인 OP_CHECKSIG를 가리킨다.
8. OP_CHECKSIG 이후로는 Pay To PubKey(P2PK)와 동일하게 동작한다.



### 비트코인 스마트 컨트랙트의 한계와 대안

------



#### 라이트닝 네트워크(Lightning Network)

[라이트닝 네트워크](https://apfl99.github.io/blockchain/lightingnetwork/)





#### 루트스탁(Rootstock, RSK)

루트스탁은 비트코인에 스마트 컨트랙트 기능을 탑재하는 사이드 체인 프로젝트로, 비트코인에서도 스마트 컨트랙트 구현이 가능하지만, 기본적으로 연산 수수료가 비싸고, 튜링 불완전이기 때문에 사용성 측면에서 제약이 있어 등장하게 되었다.

루트스탁은 2-Way peg(비트코인을 lock하고 루트스탁 체인에서 같은 양의 토큰을 unlock함으로써 마치 비트코인이 사이드체인으로 전송되는 것처럼 동작하는 것)를 이용해 비트코인에 튜링 완전한 스마트 컨트랙트를 지원하는 블록체인을 쌍방향으로 연결하며, 병합 채굴을 통해 비트코인 채굴 노드가 사이드체인 블록까지 채굴할 수 있도록 연결하고 이를 통해 비트코인에서도 튜링 완전한 스마트 컨트랙트를 실행할 수 있게 되었다.



#### 탭루트(TapRoot)

[탭루트](https://apfl99.github.io/beb/dlt2/#tap-root)



## 이더리움에서의 스마트 컨트랙트

이더리움은 개발자들이 dApp을 만들 수 있도록 튜링 완전한 언어인 솔리디티를 제공하였으며, 이더리움 네트워크에 올라간 솔리디티 코드는 EVM(Ethereum Virtual Machine)을 통해 실행된다.



#### 비트코인과 이더리움의 차이점

- 비트코인은 암호화폐 거래만 제공하는 반면, 이더리움은 암호화폐 거래 + 스마트 컨트랙트를 활용한 dApp을 만들 수 있도록 솔리디티 언어와 EVM을 지원한다.(VM의 시초)
- 비트코인은 무허가 퍼블릭 트랜잭션만을 허용하지만, 이더리움은 허가 트랜잭션과 무허가 트랜잭션을 모두 허용한다.
- 비트코인에서는 채굴 노드가 블록 생성에 대한 보상을 받지만, 이더리움에서는 블록 생성에 대한 보상을 제공하지 않고, 트랜잭션 수수료를 받는다.



### EVM

------

![i7](../../images/2022-09-16-smartcontract/i7.png)

{: .align-center}

EVM은 코드와 이더리움 블록체인 사이에 있는 가상머신으로 블록체인에서 코드가 실행될 수 있도록 한다.

dApp은 솔리디티라는 언어로 작성되고 솔리디티는 컴파일러를 통해 EVM이 읽을 수 있는 바이트 코드로 변환된다. 

이후 EVM은 바이트코드를 실행하게 되고, 이 실행과정에서 Opcode로 변환하여 실행한다. 이러한 솔리디티로 작성된 스마트 컨트랙트는 EVM에서 동작하기 때문에 특정 운영체제나 하드웨어에 종속되지 않는다.

![i8](../../images/2022-09-16-smartcontract/i8.jpg)

{: .align-center}



#### EVM 동작

[EVM 동작](https://apfl99.github.io/blockchain/evm/)





### 솔리디티

------

솔리디티는 스마트 컨트랙트를 실행하는 객체 지향, 정적 타입, 고급 스크립트 언어로 EVM에서 실행된다. 정적 타입 스크립트 언어이기에, 런타임 언어와는 달리, 컴파일 시 제약 조건을 확인하고 적용한다.

또한 튜링 완전으로 반복문 Opcode를 지원하며, 무한 반복으로 인한 공격은 Opcode에 대한 gas 부과로 방지한다.



#### 솔리디티 개발 도구

- Remix IDE
  - 솔리디티를 사용한 dApp 개발을 도와주는 통합 개발 환경으로, js로 만들어져 브라우저에서 사용가능하다.
- solc
  - 솔리디티 컴파일러다.
- Ganache
  - 개발 단계에서 시뮬레이션 테스트 환경을 구성해주는 도구이다.(로컬)
- TestNet
  - 실제 이더리움과 비슷한 테스트 네트워크로, Ropsten, Kovan, Rinkby가 있다.
- 프레임워크 : Truffle, Embark, Dapple
  - 솔리디티 코드에 대해, 테스트, 디버깅, 컴파일, 배포를 제공하는 프레임 워크이다.



## 프라이빗 블록체인에서의 스마트 컨트랙트

프라이빗 블록체인은 허가형 블록체인으로 채굴 과정과 합의 알고리즘에 대한 권한을 중앙 기관이 개발하고 유지한다.

이러한 구조 때문에 퍼블릭 블록체인과는 다른

- 네트워크 접근 권한 제어로 인한 높은 효율성
- 완전한 개인 정보 보안
- 중앙 기관으로부터의 인증으로 인한 불법적 활동 제한
- 트랜잭션 비용 적음

의 특징을 가진다.



## 대표적인 프라이빗 블록체인



### 하이퍼레저(Hyperledger)

------

하이퍼레저는 리눅스 재단에서 진행하는 스마트 컨트랙트를 지원하는 프라이빗 블록체인 프로젝트이다. 



#### 목표

산업 간 블록체인 기술을 고도화하여 협력사 간 책임성과 투명성, 신뢰가 보장되도록 한다.



#### 구성

신원 서비스, 정책 서비스, 블록체인과 트랜잭션, 스마트 컨트랙트로 구성되어 잇으며, 각 모듈에 대한 API를 제공함으로써 기존 시스템에 하이퍼레져 모듈을 이식할 수 있는 플러그 앤 플레이(Plug and Play) 방식을 지원한다.



#### 특징

- 프라이버시와 기밀성
  - 채널이라는 제한된 메시지 경로를 제공함으로써 트랜잭션 프라이버시와 기밀성을 보장해준다.
- 효율적인 처리
  - 트랜잭션 실행은 트랜잭션 정렬 및 커밋과 분리되어, 네트워크에 동시성과 병렬성을 제공한다.
  - 노드는 노드 유형에 따라 수행하는 역할이 정해져 있다.
- 체인코드
  - 체인코드는 자산과 자산을 변경하기 위한 트랜잭션 명령을 인코딩하는 데 사용한다.
- 모듈식 설계
  - 모듈식 아키텍쳐를 구현하여 네트워크 설계자에게 기능적 선택권을 제공한다.



### R3

------

R3는 블록체인을 금융 산업에 적용하고 실행하는 플랫폼을 구현하는 블록체인 컨소시엄이다. 기존의 블록체인 플랫폼의 공개적인 문제, 확장성 문제, 법적인 문제, 완결성 문제를 보완하여 블록체인 기술 및 플랫폼인 Corda를 출시하였다.



### 넥스레저(Nexledger)

------

넥스레저는 삼성SDS가 개발한 프라이빗 블록체인 플랫폼이다.



#### 목표

금융뿐만 아니라 다양한 산업에서 범용적으로 사용할 수 있는 기업용 블록체인 플랫폼



#### 특징

- BaaS를 구독하여 별도의 인프라를 구축하지 않고도 쉽게 기업용 블록체인을 사용할 수 있도록 한다.
- 블록체인 기술 API를 자체 제공하여, 넥스레저 플랫폼 위에 원하는 기술 API를 이식할 수 있다.



## 하이퍼레저 패브릭과 체인코드



### 하이퍼레저 패브릭의 주요 개념

------



#### 피어(Peer)

피어는 원장과 스마트 컨트랙트를 호스팅하는 노드이다. 네트워크의 각 피어에는 디지털 인증서가 할당되며, 디지털 인증서는 피어가 참여하는 채널에 해당 피어를 식별하도록 해준다.(권한 관리)

- 커밋 피어(Committing Peer) : 채널의 모든 피어 노드는 커밋 피어로, 생성된 트랜잭션 블록을 수신하고 블록이 네트워크에 의해 검증되면 추가 작업을 통해 원장의 사본을 커밋한다.
- 보증 피어(Endorsing Peer) : 스마트 컨트랙트를 체결한 모든 피어는 보증 피어가 될 수 있다. 보증 피어가 되기 위해서는 디지털 서명된 트랜잭션 응답을 생성하는 어플리케이션에 의해 실행되어야 한다.



#### 채널(Channel)

하이퍼레저 패브릭 채널은 둘 이상의 특정 네트워크 간의 통신을 위한 프라이빗 서브넷으로, 프라이빗 트랜잭션을 수행하기 위한 목적으로 사용된다.

채널은 멤버, 공유 원장, 체인코드 어플리케이션 및 정렬 서비스 노드에 의해 정의되며, 모든 트랜잭션은 채널에서 실행된다.

또한, 각 당사자는 트랜잭션을 수행하기 위한 인증을 거쳐야 하며, 피어는 MSP가 제공하는 고유한 식별자를 가지며, 각 피어를 해당 채널과 서비스에 인증한다.



### 스마트 컨트랙트와 체인 코드

------

스마트 컨트랙트는 전역 상태에 포함된 비즈니스 객체의 상태를 변경시키는 트랜잭션 로직을 정의하지만, 체인 코드는 블록체인 네트워크에 배포하기 위해 패키징된 스마트 컨트랙트의 집합이다.

체인 코드를 통해 아키텍트나 개발자가 하나의 블록체인 네트워크 안에 있는 서로 다른 조직 간 공유되어야 하는 데이터와 비즈니스 프로세스를 정의한다.



#### 보증

모든 체인코드에는 모든 스마트 컨트랙트에 적용되는 관련 보증 정책이 있다. 이 정책은 트랜잭션의 유효성을 위해 트랜잭션에 서명해야 하는 블록체인 네트워크 내 조직들을 의미한다.



#### 유효한 트랜잭션

스마트 컨트랙트들은 트랜잭션 제안서라 불리는 입력 파라미터의 집합을 취하여 스마트 컨트랙트의 프로그램 로직과 함께 사용하여 원장을 읽고 쓴다.

전역 상태의 변화는 읽은 상태와 트랜잭션이 유효할 경우 새로운 상태를 포함하는 읽기-쓰기 집합으로 구성된 트랜잭션 제안 응답으로 캡처된다. 전역상태는 스마트 컨트랙트가 실행될 때 변하지 않고, 최종적으로 유효해지면 업데이트된다.

트랜잭션 검증은 

1. 보증 정책에 따라 지정된 조직에서 서명된 거래인지 확인
2. 전역 상태의 현재 값이 보증 피어 노드에 의해 서명되었을 때 트랜잭션의 읽기 집합과 일치하는지 확인

의 과정으로 구성되며, 유효하지 않은 트랜잭션 또한 블록체인 이력에 추가되지만 전역 상태를 업데이트하지 않는다.



### 이더리과 하이퍼레저 패브릭의 차이점

------



#### 플랫폼의 목적

- 이더리움은 EVM 위에서 스마트 컨트랙트를 실행하는 목적을 위한 만들어졌다.
  - B2C
- 하이퍼레저는 고성능과 신뢰성이 높은 블록체인 개발을 위한 산업 전반의 협업을 가속화하기 위해 도입되었다.
  - B2B



#### 작동 방식 & 기밀성

- 이더리움은 퍼블릭 블록체인이다.
  - 트랜잭션 접근에 제약이 없다.
- 하이퍼레저 패브릭은 프라이빗 블록체인이다.
  - 허가된 멤버만 트랜잭션에 접근할 수 있다.



#### 피어 역할

- 이더리움은 피어마다 역할이 있는데, 트랜잭션이 완료되면 이를 위해 여러 노드가 참여해야 하므로 확장성, 프라이버시, 효율성 등의 문제가 발생할 수 있다.
- 하이퍼레저는 네트워크 내의 각 피어에게 트랜잭션을 수행하기 위해 정보를 제공할 필요가 없는 DLT(Distributed Ledger Technology)를 제공한다.



#### 합의 메커니즘

- 이더리움은 PoS를 사용한다.
- 하이퍼레저 패브릭은 합의하지 않거나, 다양한 합의 프로토콜 중 하나를 선택할 수 있다.



#### 프로그래밍 언어

- 이더리움은 솔리디티를 사용한다.
- 하이퍼레저는 Go언어를 사용한다.



## 스마트 컨트랙트 활용

### 스마트 컨트랙트를 활용한 다양한 서비스들

---


#### DeFi(Decentralized Finance)


DeFi는 스마트 컨트랙트를 활용해 금융 시장 옵션, 스테이블 코인, 거래소, 자산 관리 등 전통적인 금융 상품과 서비스를 재창조하고, 여러 서비스를 결합하여 새로운 금융 원형을 만들어내는 어플리케이션이다.



#### 게임과 NFT


블록체인 기반 게임은 게임 내 액션의 변조 방지를 위해 스마트 컨트랙트를 사용한다. 또한, 한정판 NFT는 공정한 배포 모델을 가질 수 있으며, 모든 사용자가 디지털 자산을 얻을 공정한 기회를 가질 수 있다.



#### DAO(Decentralized Autonomous Organization)


DAO는 컴퓨터 프로그램으로 인코딩되고, 조직 구성원들이 통제하며, 중앙 정부의 영향을 받지 않는 규칙들로 구성된 탈중앙화 자율 조직으로, 영리 및 비영리 기업을 조직하기 위한 새로운 분산형 비즈니스 모델을 제공한다.

조직의 규칙은 코드에 내장되어 있어 관리자가 필요하지 않으며, 계층적 구조가 없다.



#### ICO(Initial Coin Offering)


ICO는 IPO와 비슷한 개념으로 새로운 암호화폐를 만들기 위해 투자자들에게 투자금을 받고, 그 댓가로 코인을 나눠주는 것이다. 



#### 브릿지


브릿지는 토큰이나 임의의 데이터를 하나의 체인에서 다른 체인으로 전송할 수 있게 하는 연결을 의미한다. 브릿지는 양쪽에서 안전하게 상호 운용할 수 있는 호환 가능한 방법을 제공한다.



### 스마트 컨트랙트 도입 분야

---



#### 일정한 형식의 반복적인 계약이 많은 경우

특정 조건을 만족시키면 계약에 대한 보상금이 지급되도록 스마트 컨트랙트를 작성하여, 조건 성취 시 자동으로 보험금을 지불하도록 한다.

- Ex. 보험



#### 원격자 간 계약 체결이 필요한 경우

자산을 공유하는 경우, 계약 조건을 정하고 이에 따라 금전을 지급하거나 서비스를 제공하도록 한다.

- Ex. 자동차 렌탈 서비스



#### 제품의 유통 추적이 필요한 경우

유통에 참여하는 당사자들이 유통 물품에 대한 정보를 확인하고, 계약 조건이 만족하면 스마트 컨트랙트 상에서 대금을 지불한다.

- Ex. 물류 유통



#### 그외 : 저작권 등

저작권에 대한 소유증명서를 발급하고, 사용자 정보를 저장하여 저작권 인증을 한다.



[DApp](https://dappradar.com/)



### 오라클 문제

---

오라클은 Web API나 마켓 데이터 피드와 같은 방식을 통해 블록체인과 스마트 컨트랙트용 외부 데이터를 검색하고 검증하는 것을 의미한다.(외부에서 오는 데이터는 결정적이지 않기 때문에 해당 정보를 블록체인이 이해하고 특정 조건에서 실행할 수 있도록 한다.)

- Ex. 가격 정보, 날씨 정보, 게임 난수 생성



#### 문제

오라클 문제는 서드 파티 오라클과 스마트 컨트랙트의 무신뢰성 실행 간 보안, 인증, 신뢰 충돌 문제에 관한 것이다. 

스마트 컨트랙트는 데이터에 대한 판단 능력을 갖추지 않고 있기 때문에 데이터에 대한 신뢰성에 의문이 제기될 수 있다.



#### e.g., 블록체인에서의 난수 생성 문제

블록체인에서 난수를 생성하는 방식에는 다음과 같은 방식을 생각할 수 있다.

1. 물리적인 현상을 가져오기
   - 이 경우, 노드가 자신에게 유리한 값이 나올 때까지 물리 현상을 관측하고 난수 생성을 조작할 수 있다.
2. 여러 사용자가 가져온 값을 사용하기
   - 여러 사용자 중 악의적인 사용자가 다른 사용자들의 값을 참고하여 자신에게 유리한 값이 나오는 값을 보내 난수 생성을 조작할 수 있다.
3. 블록체인 내부의 값 사용하기
   - 블록 해시 등을 사용할 수 있지만 이 역시도, 채굴 노드가 자신에게 유리한 블록 해시값이 나올 때까지 해시값을 조작할 수 있다.



-> 따라서 진짜 난수를 만들 때는 이 난수값이 정말로 조작되지 않았는지 증명할 수 있어야 한다.



#### 블록체인에서의 난수 생성 해결

1) Commit Reveal Scheme

![i9](../../images/2022-09-16-smartcontract/i9-3497094.png)

{: .align-center}

Commit Stage에서는 자신이 보내려는 값을 암호화하고 일정 금액의 토큰과 함께 스마트 컨트랙트를 보낸다.

이후 일정 기간이 지나면 참여자들은 자신이 제출하려고 했던 원래 값을 스마트 컨트랙트를 보내 스마트 컨트랙트는 사용자가 보낸 원래 값을 암호화하여 이전에 보낸 암호화된 값과 비교함으로써 유효성 검사를 하고 유효하다면 토큰을 돌려준다. 

그리고 스마트 컨트랙트는 주어진 값들을 사용해 난수를 생성한다.

-> 이러한 과정을 통해 서로의 값을 모른 채로 값을 제출하게하여 난수 조작 불가에 대한 증명을 한다.



2) BLS Scheme

![i10](../../images/2022-09-16-smartcontract/i10.png)

{: .align-center}
BLS Scheme는 임계값 서명의 한 종류로, N명의 사용자가 개인키 S를 쪼개어 가지고 있다가, 난수를 생성해야 할 때 참여자들이 개인키 조각을 제출하고 이 때 개인키를 K명이상 제출하면 K개의 개인키 조각을 가지고 난수를 생성한다. 

-> Commit Reveal Scheme는 한 두명이 정직하지 안더라도 난수 생성이 불가능해질 수 있지만, BLS Scheme는 K명의 사람이 정직하면 난수를 생성할 수 있는 장점이 있다.







<div class="notice">
    <p>본 포스팅은 코드스테이츠 BEB 과정을 수강하며 작성한 글입니다.</p>
  <h5>Reference</h5>
    <a>https://ethereum.org/en/developers/docs/evm/</a>
  <br>
</div>


​						
​				
