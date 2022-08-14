---
title: "[데이터베이스] 비관계형 데이터베이스 - NoSQL Database : MongoDB"
categories:
 - GroundX
tags: [Non Relational Database, NoSQL, MongoDB] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기

---



# NoSQL Database : MongoDB

--------------

MongoDB는 NoSQL 데이터베이스로, 데이터를 도큐먼트 형태로 저장한다. 도큐먼트는 컬렉션에 저장되며, 따라서 MongoDB는 도큐먼트 데이터베이스이다.



## Atlas Cloud

MongoDB에서는 Atlas로 클라우드에 데이터베이스를 설정한다. 이는 GUI와 CLI로 데이터 시각화, 분석, Import, Export, Build에 사용가능하다. Atlas의 사용자는 클러스터를 배포할 수 있으며, 그룹화된 서버에 데이터를 저장한다.



### 클러스터 배포

------------------

- 클러스터 : 각 인스턴스(데이터베이스)들의 모임을 클러스터라고 하며, 하나의 시스템 처럼 동작한다.
- 레플리카 세트 : 데이터 사본을 저장하는 인스턴스의 모임으로 인스턴스 중 하나가 문제가 발생하더라도 데이터는 그대로 유지되며, 나머지 레플리카 세트에 저장된 데이터로 작업이 가능하다.



### MongoDB Document

------------------

- 도큐먼트(Document) : 필드-값 쌍으로 저장된 데이터
- 필드(Field) : 데이터 포인트를 위한 고유한 식별자
- 값(Value) : 주어진 식별자와 연결된 데이터
- 컬렉션(Collection) : MongoDB의 도큐먼트로 구성된 저장소, 도큐먼트 -> 컬렉션 -> 데이터베이스
- Ex. Field : value

```js
{
  <field> : <value>,
  "name" : "kim",
  "age" : "30"
}
```

 

### JSON vs BSON

------------------



#### JSON

shell을 이용하여 도큐먼트를 조회하거나 업데이트할 때, 도큐먼트가 출력되는 형식으로 형식은 다음과 같다.

```js
{
  "id":"123123123123123123",
  "date":ISODate("2022-08-14T05:00:00Z"),
  "name":"kim",
  "password":"1234",
}
```

- 이러한 JSON 형식은 읽기 쉽지만 텍스트 형식이기 때문에 파싱이 느리고 메모리 사용이 비효율적이다.
- 또한, JSON 형식은 기본 데이터 타입만 지원하기 때문에 데이터 타입에 제약이 있다.



#### BSON(Binary JSON)

컴퓨터의 언어에 가까운 이진법에 기반을 둔 표현이다.

```js
{"hello": "world"} →
\x16\x00\x00\x00           // total document size
\x02                       // 0x02 = type String
hello\x00                  // field name
\x06\x00\x00\x00world\x00  // field value
\x00                       // 0x00 = type EOO ('end of object')
```

- JSON 보다 메모리 사용이 효율적이며, 빠르고, 가볍고, 유연하다.
- 더 많은 데이터 타입을 사용할 수 있다.
- 따라서 MongoDB 내부에서는 속도, 효율성, 유연성의 장점이 있는 BSON으로 데이터를 저장, 사용한다.



### Importing & Exporting

------------



#### Export

- BSON

```shell
mongodump --uri "mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/database_name"
```



