---
title: "[데이터베이스] 비관계형 데이터베이스 - NoSQL Database : MongoDB"
categories:
 - BEB
tags: [Non Relational Database, NoSQL, MongoDB, Atlas, Compass] 
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



## MongoDB 사용하기 with Atlas

[MongoDB 설치 및 서비스 확인](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-os-x/)



### Import & Export

----------



#### Export

- BSON & JSON : mongodump

```shell
mongodump --uri "<Atlas Cluster URI>"
# Atlas Cluster URI : mongodb+srv://<your name>:<your password>@<your cluster>.mongodb.net/database_name"
 
```
- - 여기서 your cluster는 atlas의 connect shell에서 확인 가능하다.
  - Ex. cluster0.clusterkey.mongodb.net

![img1](../../images/2022-08-14-nonrelationalDatabase/img1.png)

{: .align-center}

![img2](../../images/2022-08-14-nonrelationalDatabase/img2.png)

{: .align-center}



- JSON : mongoexport

```shell
mongoexport --uri "<Atlas Cluster URI>"
						--collection=<collection name>
						--out=<filename>.json
```

![img3](../../images/2022-08-14-nonrelationalDatabase/img3.png)

{: .align-center}

![img4](../../images/2022-08-14-nonrelationalDatabase/img4.png)

{: .align-center}



#### Import

- BSON & JSON : mongorestore

```shell
mongorestore --uri "<Atlas Cluster URI>"
						 --drop <dump dir>
```

- - 여기서 drop을 통해 기존에 있던 데이터를 삭제한다.(선택)

![img5](../../images/2022-08-14-nonrelationalDatabase/img5.png)

- JSON & CSV

```shell
mongoimport --uri "<Atlas Cluster URI>"
						--drop <filename>.json
```

- - 마찬가지로 drop을 통해 기존에 있던 데이터를 삭제한다.(선택)
  - --collection 옵션을 통해 컬렉션을 지정할 수도 있다.

![img6](../../images/2022-08-14-nonrelationalDatabase/img6.png)

{: .align-center}



### CRUD

------------



#### Create : insert

```shell
db.collection.insert({
	# some content ex. {...}
})
```

- MongoDB 도큐먼트는 모든 도큐먼트가 _id 필드를 기본값으로 반드시 가지고 있어야 한다.
- _id 필드는 중복을 허용하지 않으며, 각 도큐먼트를 구별하는 역할을 한다.
- _id : ObjectId(12byte, 24char)
  - Ex. ObjectId("59b99db4cfa9a34dcd7885b6")
- 다수의 도큐먼트의 경우 []로 감싼다.
  - 이 경우 배열 안의 리스트 된 순서대로 진행되기 때문에 
  - {ordered: false}로 순서와 상관 없이 고유한 _id를 가진 도큐먼트는 모두 컬렉션에 삽입될 수 있다.
- 존재하지 않는 컬렉션에 insert할 경우 컬렉션이 만들어지면서 데이터가 삽입된다.

- Ex.

```shell
db.products.insert(
   [
     { _id: 20, item: "lamp", qty: 50, type: "desk" },
     { _id: 21, item: "lamp", qty: 20, type: "floor" },
     { _id: 22, item: "bulk", qty: 100 }
   ],
   { ordered: false }
)
```

[insert 공식문서](https://www.mongodb.com/docs/manual/reference/method/db.collection.insert/#mongodb-method-db.collection.insert)



#### READ : find, findOne()

- find : 데이터 조회
- .findOne() : 특정한 데이터 한 개만 조회(여러 개일 경우 무작위)

```shell
db.collection.find(query,projection)
# query Ex. {"state":"NY"}
```

- .pretty() : 데이터를 읽기 편하게 정렬하여 보여준다.
- .count() : 데이터 수 조회
- 이하 skip, limit, sort .. 등 find나 findOne 뒤에 붙이면 기능에 따른 조회를 할 수 있다.

- Ex.

```shell
db.bios.find( {
   birth: { $gt: new Date('1920-01-01') },
   death: { $exists: false }
} ).pretty()
```

[find 공식문서](https://www.mongodb.com/docs/manual/reference/method/db.collection.find/#mongodb-method-db.collection.find)

[findOne 공식문서](https://www.mongodb.com/docs/manual/reference/method/db.collection.findOne/#mongodb-method-db.collection.findOne)



#### UPDATE : updateOne(), updateMany()

- updateOne() : 주어진 기준에 맞는 다수의 도큐먼트 중 첫번째 도큐먼트 하나만 업데이트
- updateMany() : 쿼리문과 일치하는 모든 도큐먼트를 업데이트

```shell
db.collection.updateOne( {
	<filter>, #ex. {"field": "value"}
	<update>  #ex. {"$set" : {"population":6235}}
})
```

- $set : 해당 필드 값 수정
- $inc : 해당 필드 값에 값 추가
- $push : 배열로 이루어진 필드의 값에 요소를 추가하기 위한 연산자
- ...
- Ex.

```shell
   db.restaurant.updateMany(
      { violations: { $gt: 4 } },
      { $set: { "Review" : true } }
   );
```

[updateOne 공식문서]([**https://www.mongodb.com/docs/manual/reference/method/db.collection.updateOne/#mongodb-method-db.collection.updateOne**](https://www.mongodb.com/docs/manual/reference/method/db.collection.updateOne/#mongodb-method-db.collection.updateOne))

[updateMany]([**https://www.mongodb.com/docs/manual/reference/method/db.collection.updateMany**](https://www.mongodb.com/docs/manual/reference/method/db.collection.updateMany))



#### DELETE : deleteOne(), deleteMany() ,drop()

- deleteOne() : 주어진 기준에 맞는 다수의 도큐먼트 중 첫번째 도큐먼트 하나를 삭제
- deleteMany() : 쿼리문과 일치하는 모든 도큐먼트를 삭제
- drop() : 해당 컬렉션 삭제

```shell
db.collection.deleteOne(
   <filter>, #ex. {"field": "value"}
)
```

- Ex.

```shell
   db.orders.deleteMany(
       { "client" : "Crude Traders Inc." },
       { w : "majority", wtimeout : 100 }
   );
```

[deleteOne 공식문서](https://www.mongodb.com/docs/manual/reference/method/db.collection.deleteOne/#mongodb-method-db.collection.deleteOne)

[deleteMany 공식문서](https://www.mongodb.com/docs/manual/reference/method/db.collection.deleteMany/#mongodb-method-db.collection.deleteMany)

[drop 공식문서](https://www.mongodb.com/docs/manual/reference/method/db.collection.drop/#mongodb-method-db.collection.drop)



## + MongoDB 사용하기 with Atlas + Compass

위의 Import& Export, CRUD 과정은 Compass 툴을 통해서 GUI로 보다 쉽게 작업 가능하다.

![img7](../../images/2022-08-14-nonrelationalDatabase/img7.png)

{: .align-center}

[compass](https://www.mongodb.com/docs/compass/current/?_ga=2.265061519.1226231499.1660475059-1378108012.1660183316)



### Aggregation Framework

-----------------

- MQL보다 다양한 쿼리를 작성할 수 있다.
- 파이프라인의 각 단계 순서대로 데이터가 처리된다.
- 이 안의 데이터는 원본데이터를 수정하거나 변경하지 않는다.
- 각 단계의 이름 앞에는 "$"가 있고 그 뒤에는 실행할 작업에 대한 설명이 온다.

[Aggregation Framework](https://www.mongodb.com/docs/manual/reference/operator/aggregation/)



<div class="notice">
  <p>본 포스팅은 코드스테이츠 BEB 과정을 수강하며 작성한 글입니다.</p>
</div>
