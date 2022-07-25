---
title: "자료 구조 기초 : Stack, Queue"
categories:
 - BEB
tags: [Data Structure, Stack, Queue] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기
---

# Stack

Stack : 데이터를 순서대로 쌓는 자료구조



## 특징

- 입력과 출력이 하나의 방향으로 이루어지는 제한적 접근
- LIFO(Last In First Out) 혹은 FLIO(First In Last Out) -> 선입후출이다.
- 데이터를 넣는 것을 **"Push"**, 데이터를 꺼내는 것을 "Pop"이라고 한다.

```js
stack.push(data)
-------------------------
1 <- 2 <- 3 <- 4
-------------------------

stack.pop()
-------------------------
1 -> 2 -> 3 -> 4 ->  
-------------------------
```



## 예시

