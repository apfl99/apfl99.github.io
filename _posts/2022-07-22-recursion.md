---
title: "[자료구조/알고리즘] 재귀"
categories:
 - BEB
tags: [자료구조, 알고리즘, 재귀, JS] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기
---

# 재귀

---------------------------

재귀 : 동일한 구조의 더 작은 문제를 해결함으로써 주어진 문제를 해결하는 방법

- Ex. 일반 함수를 이용한 배열의 합 구하기

```js
function arrSum(arr) {
  let sum = 0;
  for(let i=0;i<arr.length;i++) {
    sum += arr[i];
  }
  return sum;
}
```

- Ex. 재귀 함수를 이용한 배열의 합 구하기

```js
function arrSum(arr) {
  if(arr.length === 0) { //배열의 길이가 0일 경우
    return 0;
  } else if(arr.length === 1) { //배열의 길이가 1일 경우 배열의 요소를 리턴
    return arr[0];
  }

  return arr[0] + arrSum(arr.slice(1)); // 첫번째 요소와 그 밖의 요소로 문제를 나눈다.
}
```

이와 같이 재귀를 통해서 기존의 문제에서 출발하여 더 작은 경우를 생각하고, 문제가 더 작아지지 않을 때까지 더 작은 경우를 생각하여 대입한 후, 문제가 간단해져서 바로 리턴값을 얻을 수 있게 되는 경우 간단한 문제부터 해결할 수 있다.



## 재귀는 언제 사용하는 것이 좋을까?

사실 모든 재귀 함수는 반복문으로 표현 가능하다. 그러나 재귀를 적용하게 되면 코드가 간결하고 이해하기 쉽게 된다. 따라서 아래와 같은 경우 재귀를 사용하는 것이 적합하다.

1. 주어진 문제를 비슷한 구조의 더 작은 문제로 나눌 수 있는 경우
2. 중첩된 반복문이 많거나 반복문의 중첩 횟수를 예측하기 어려운 경우



## 재귀의 사용

```js
function recursive(input, ...) {
  //Base Case : 문제를 더 이상 쪼갤 수 없는 경우, 문제가 간단해져서 바로 리턴값을 얻을 수 있게 	 되는 경우
  if(Base Case) {
    return 단순한 문제에 대한 리턴값;
  }
  //Recursive Case
  //문제가 아직은 복잡한 경우
  return 더 작은 문제로 새롭게 정의; // 이 정의에는 재귀이기 때문에 자신의 함수를 호출한다.
}
```

위 코드와 같이 재귀는 먼저 문제의 간단한 문제의 정도와 결과 값의 도출 방법을 정의하고, 이후 재귀를 활용하므로써 더 간단한 문제를 다음 재귀로 보내기 위해 어떻게 문제를 간단하게 만들 것인지 정의하는 느낌이다.



### 재귀 예시

---------------------------

- 배열 형태 재귀
  - 뭔가 첫번째 요소 + 나머지 요소의 반복로 잘라서 생각하는 것이 편한 것 같다.
  - Ex. 배열의 평탄화

```js
function flattenArr(arr) {
  return arr.reduce(function (result, item) { //reduce를 활용한 각 요소 접근 및 return 할 배열 정의
    if(Array.isArray(item)) { //현재 요소 값이 배열이라면 평탄화를 하고 result와 합치기
      const flattened =flattenArr(item);
      return [...result, ...flattend];
    } else {	// 현재 요소가 배열이 아니라면 그냥 합치기 : Base Case
			return [...result,item]
    }
  },[])
}

let output = flattenArr([[1],2,[3,4],5]);
console.log(output); // -< [1,2,3,4,5]

output = flattenArr([[2,[[3]]],4,[[[5]]]]);
console.log(output); // -> [2,3,4,5]
```

- 객체 형태의 재귀
  - 객체는 주로 자식 노드에 대한 접근으로 진행되며, 재귀의 간단한 문제 제한(Base Case), 역시 자식 노드가 더 존재하지 않을때로 많이 사용한다.
  - Ex. 러시아 인형, 조건에 맞는 객체 찾기

```js
function findMatryoshka(matryoshka, size) {
  //Base Case : matryoshka가 더 이상존재하지 않을 경우
  if(matryoshka == null || Object.keys(matryoshka).length == 0) {
    return false;
  }
  if(matryoshka.size === size) { // 조건에 맞는 객체를 찾은 경우
    return true
  }
  
  //재귀 : 자식 노드에 대한 접근
  return findMatryoshka(matryoshka.matryoshka,size)
}

const matryoshka = {
  size: 10,
  matryoshka: {
    size: 9,
    matryoshka: null,
  },
};

let output = findMatryoshka(matryoshka, 10);
console.log(output); // --> true

output = findMatryoshka(matryoshka, 8);
console.log(output); // --> false
```

[재귀를 활용한 JSON.stringify 구현 예제](https://github.com/apfl99/im-sprint-stringify-json)

[재귀를 활용한 Tree UI 구현 예제](https://github.com/apfl99/im-sprint-tree-ui)

