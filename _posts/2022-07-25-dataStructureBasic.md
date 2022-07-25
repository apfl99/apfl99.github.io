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
- 데이터를 넣는 것을 **"Push"**, 데이터를 꺼내는 것을 **"Pop"**이라고 한다.
- 데이터는 하나씩 넣고 뺄 수 있다.

```js
stack.push(data)
-------------------------
1 <- 2 <- 3 <- 4
-------------------------

stack.pop()
-------------------------
1 -> 2 -> 3 -> 4 ->  
-------------------------

//Stack은 메모리가 위로 쌓이는 구조라고 볼 수 있다.
//따라서 Stack의 크기(Size)는 현재 스택 메모리의 높이로 계산 가능
```



## 예시

### 브라우저의 뒤로 가기, 앞으로 가기 기능

- 페이지 접속
  - 현재 페이지 -> Prev Stack 보관
- 뒤로 가기
  - 현재 페이지 -> Next Stack 보관
  - Prev Stack -> 현재 페이지로
- 앞으로 가기 
  - 현재 페이지 -> Prev Stack 보관
  - Next Stack -> 현재 페이지로

```js
function browserStack(actions, start) {
  //prev, next 스택 및 현재 페이지 정의
  var prev_stack = [];
  var next_stack = [];
  var curr_page = start;

  //start값 검사
  if(typeof start != 'string') return false;

  for(let i=0;i<actions.length; i++) {
    //action이 문자열일 경우 => 페이지 접속
    //현재 페이지 -> prev_stack
    //현재 페이지가 새로운 문자열 -> next_stack 초기화
    if(typeof actions[i] == 'string') {
      prev_stack.push(curr_page);
      curr_page = actions[i];
      next_stack = [];
    }
    //action이 -1일 경우 => 뒤로가기 이고 Prev Stack에 값이 존재할 경우
    //1. 현재 페이지 -> next_stack
    //2. prev_stack -> 현재 페이지
    else if(actions[i] === -1) {
        next_stack.push(curr_page);
        curr_page = prev_stack.pop();
    }

    //action이 1일 경우 => 앞으로 가기 이고 Next Stack에 값이 존재할 경우
    //1. 현재 페이지 -> prev_stack
    //2. next_stack -> 현재 페이지
    else if(actions[i] === 1 && next_stack.length != 0) {
        prev_stack.push(curr_page);
        curr_page = next_stack.pop();
    }
  }

  //리턴값
	return [prevStack, current, nextStack];
}

const actions = ["B", "C", -1, "D", "A", -1, 1, -1, -1];
const start = "A";
const output = browserStack(actions, start);

console.log(output); // [["A"], "B", ["A", "D"]]

const actions2 = ["B", -1, "B", "A", "C", -1, -1, "D", -1, 1, "E", -1, -1, 1];
const start2 = "A";
const output2 = browserStack(actions2, start2);

console.log(output2); // [["A", "B"], "D", ["E"]]
```



# Queue

Queue : Stack과 마찬가지로, 데이터를 순서대로 쌓지만 데이터가 입력된 순서대로 처리할 때 주로 사용



## 특징

- 입력과 출력이 두 방향으로 이루어지는 접근
- FIFO(First In First Out) -> 선입선출이다.
- 데이터를 넣는 것을 **"Push"**, 데이터를 꺼내는 것을 **"Shift"**이라고 할 수 있다.
- 데이터는 하나씩 넣고 뺄 수 있다.

```js
//queue.enqueue(data)
queue.push()
출력 <--------------------------< 입력
1 <- 2 <- 3 <- 4
//queue.dequeue(data)
queue.shift()
출력 <--------------------------< 입력
1 <- 2 <- 3 <-4

//Queue는 단방향 구조이기 때문에 배열에서 맨 뒤 인덱스를 가르키는 값과 맨 앞 인덱스를 가르키는 값의 차를 통해 Queue의 크기를 계산 가능하다.
```



## 예시

### 일반적으로 버퍼링을 해결하고자 Queue를 도입 Ex. 동영상 스트리밍, 프린터

```js
function queuePrinter(bufferSize, capacities, documents) {
    var queue = [];
    var queue_bufferSize = 0; //새로운 문서가 들어올 때 예상 버퍼 사이즈
    var count = 0; // 실행 시간
    
    //처음 문서 처리 : 무조건 버퍼 사이즈는 통과
    // bufferSize만큼 queue 초기화
    for(let i=0;i<bufferSize;i++) {
      queue.push(0);
    }
    
    //documents에서 맨 앞거 shift 후 currDocument에 대입 -> queue에 -> queueBufferSize도 업데이트
    var currDocument = documents.shift();
    queue.push(currDocument);
    queue.shift(); //처음 값 shift 하나 들어왓으니까 하나 나가야 댐
    queue_bufferSize += currDocument;

    count++;
  
  

    //반복 : queue_bufferSize가 0일때 까지 -> 모두 출력 때 까지
    while(queue_bufferSize >= 0) {
      //curr 정의
      currDocument = documents.shift();

      //queue에서 나갈 요소 미리 빼기
      queue_bufferSize = queue_bufferSize - queue[0];
      

      //여기서 cur + 현재 queue의 값이 capacities보다 클 경우 cur은 다시 documents에 들어가고 queue는 한칸 앞으로
      if(currDocument + queue_bufferSize > capacities) {
        documents.unshift(currDocument);
        queue.shift();
        queue.push(0)
      }
      // 작을 경우 cur은 queue에 들어가고 queue_bufferSize + queue 한칸 앞으로
      else {
        queue.push(currDocument);
        queue_bufferSize += currDocument;
        queue.shift();
      }

      count++;
    }
    //마지막 나가는 거 : 버퍼에 남은 데이터들 출력
    while(queue.shift() !== undefined) {
      count++;
      console.log(queue);
    }

    return count;
}

let bufferSize = 2;
let capacities = 10;
let documents = [7, 4, 5, 6];

let output = queuePrinter(bufferSize, capacities, documents);
console.log(output) // 8
```



# *Push, Pop, unShift, Shift

## Push

```js
var arr = [0,1,2,3,4]
arr.push(5)
console.log(arr) // -> [0,1,2,3,4,5]
```

![IMG_2B54A8632672-1](../../images/2022-07-25-dataStructureBasic/IMG_2B54A8632672-1.jpeg)

{: .align-center}

[Push MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/push)



## PoP

```js
var arr = [0,1,2,3,4]
arr.pop()
console.log(arr) // -> [0,1,2,3]
```

![IMG_06A80C79EB32-1](../../images/2022-07-25-dataStructureBasic/IMG_06A80C79EB32-1.jpeg)

{: .align-center}

[PoP MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/pop)



## unShift

```js
var arr = [0,1,2,3,4]
arr.unshift(5)
console.log(arr) // -> [5,0,1,2,3]
```

