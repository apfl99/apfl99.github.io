---
title: "Docker로 Ubuntu Geth실행"
categories:
 - BEB
tags: [Docker, Ubuntu, Geth] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기


---



# Docker로 Ubuntu Geth 실행



## Docker로 Ubuntu 실행하기

[Docker 설치](https://docs.docker.com/desktop/install/mac-install/)

도커를 설치한 이후 우분투 이미지를 받아온다.

```shell
$ docker pull ubuntu
```

이미지를 받아오면 이미지를 확인하고 컨테이너를 생성한다.

```shell
$ docker image ls # 이미지 확인
$ docker create -it --name con_ubuntu ubuntu # con_ubuntu라는 컨테이너 생성
```

![i1](../../images/2022-09-14-dockergeth/i1.png)

{: .align-center}

컨테이너 생성이 완료되면, 컨테이너를 시작하고 컨테이너가 구동되었는지 확인한다.

```shell
$ docker start con_ubuntu # con_ubuntu 시작
$ docker ps # Ps 확인
```

![i2](../../images/2022-09-14-dockergeth/i2.png)

{: .align-center}

위와 같이 도커 프로세스가 확인되면, 해당 컨테이너로 연결을 진행한다.

```shell
$ docker attach con_ubuntu
```

![i3](../../images/2022-09-14-dockergeth/i3.png)

{: .align-center}



## Ubuntu 컨테이너 셋팅 및 Geth 설치

먼저 apt update와 PPA를 추가하거나 제거할 때 사용하는 software-properties-common을 설치해준다.

- PPA : Personal Package Archive, Ubuntu에서 소프트웨어를 배포하기 위한 저장소를 만들고, Ubuntu 레포지토리를 통해 사용할 수 없는 최신 소프트웨어 버전이나 소프트웨어를 쉽게 얻을 수 있게 도와준다.

```shell
$ apt update -y && apt install -y software-properties-common
```

설치가 완료되면 PPA를 통해 Ethereum 레포지토리를 다운로드 한다.

```shell
$ add-apt-repository ppa:ethereum/ethereum
```

이후 파일 편집을 위한 Vim과 geth를 설치한다. 

```shell
$ apt-get install vim -y # vim 설치
$ apt update -y && apt install geth # 업데이트 및 geth 설치
$ apt-get install git -y # git 설치
$ cd ~ 
$ git clone https://github.com/ethereum/go-ethereum
 # go-ethereum 클론
$ apt-get install -y build-essential golang # go 설치
```

설치가 완료되면 마지막으로 geth 버전을 확인한다.

```shell
$ geth version
```

![i5](../../images/2022-09-14-dockergeth/i5.png)

{: .align-center}



<div class="notice">
  <p>본 포스팅은 코드스테이츠 BEB 과정을 수강하며 작성한 글입니다.</p>
  <h5>Reference</h5>
  <a>https://itsfoss.com/ppa-guide/</a>
  <br>
</div>