---
title: "[JS/Node] 비동기"
categories:
 - BEB
tags: [callback, promise, async/await, fs, fetch] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기
---

# 동기(sync) & 비동기(async)란

----------------------

작업이 시작되면 결과가 나올 때까지 다른 일로 넘어갈 수 없는 동기 방식과 다르게 비동기는 작업 요청 후, 다른 일을 하다가 결과가 나오면 결과를 받아 올 수 있다. 동작 방식은 다음과 같다.

![IMG_B87E69259944-1](../../images/2022-07-28-jsnodeasync/IMG_B87E69259944-1.jpeg)

{: .align-center}

이러한 비동기적 방식은 작업 프로세스에 있어 효율적이기 때문에 DOM의 Element 이벤트 핸들러, 타이머, 서버에 자원 요청 및 응답 등 많은 곳에 사용된다. 그러나 의문이 드는 점이 있었다. 그것은 "비동기적 방식이면 병렬처리로 진행되는 것과 같았는데, 싱글 스레드 프로그래밍 언어인 JS가 병렬적으로 작업 수행을 할 수 있는 것일까" 였다. 그렇다면 비동기 방식에서 어떻게 JS는 내부적으로 동작할까?



## JS 비동기 동작

먼저 JS는 싱글 스레드 프로그래밍 언어이다. 따라서 JS는 함수를 실행하기 위해 해당 함수를 스택에 넣고 함수에 리턴이 일어나면 스택의 가장 위쪽에서 해당 함수를 꺼내게 되는 방식으로 콜 스택이 이루어진다. (재귀함수를 사용하다보면 "RangeError: Maximum call stack size exceed"와 같은 오류를 볼 수 있는 데, 이 역시, 스택에 동일한 함수가 계속(많이) 쌓이기 때문에 Error가 발생한 것이다.) 그렇다면 이러한 특성을 가지고 있는 JS가 어떻게 비동기적 동작을 할 수 있을까?

  ![IMG_21B92223E35E-1](../../images/2022-07-28-jsnodeasync/IMG_21B92223E35E-1.jpeg)

위 그림은 좀.. 그렇긴하지만, setTimeout으로 비동기적 처리를 실행했을때 JS와 WebAPI의 프로세스이다. 순서를 설명하자면

1. Main()과 Log('Hi')가 Stack에 쌓이게 되고, Log('Hi')처리 결과를 리턴한다.(console.log는 내부적으로 리턴값이 존재한다.)
2. 이후 setTimeout  cb 함수(콜백 함수)가 스택에 쌓인다. setTimeout을 통해 비동기적 요청을 했기 때문에 webapis로 들어가 설정한 5초간 timer를 센다.
3. 그동안 main에 있는 Log('JSConfEU')가 Stack에 쌓이고, Log('JSConfEU')에 대한 처리 결과를 리턴한다.
4. 로그를 다 실행했기 때문에 main 함수도 리턴된다.
5. webapis에서 5초가 지나면 cb 함수(콜백 함수) task queue로 보낸다.
6. stack이 비어있고, task가 존재할 경우 stack으로 task queue에 있는 task를 stack으로 보내주는 역할을 하는 event loop를 통해 cb 함수가 stack에 쌓이게 된다.
7. stack에서 cb 함수(콜백 함수)를 처리 결과를 리턴한다.

와 같다. 따라서 결국 JS는 자체적으로 비동기적 처리를 하는 것이 아니라 WebAPI와 task Queue, event Loop 등을 통해서 비동기적 처리를 하게 되는 것이다.



## JS 비동기 동작 : 순서 제어(비동기의 동기적 처리)

비동기적 처리를 하게 되면 프로세스가 효율적으로 동작할 수 있다. 그러나 각각의 task가 실행되는 속도가 다르기 때문에 개발자의 의도대로 task 순서를 처리하는 것이 필요하다. 이는 어떻게 처리할까?



### 1. Callback

```js
const printString = (string, callback) => { // printAllNotUseCallback에서는 callback 사용 X
  setTimeout(
  	() => {
      console.log(string)
      callback() //printAllNotUseCallback에서는 사용 X
    },
    Math.floor(Math.random() * 100) + 1
  )
}

const printAllUseCallback = () => {
  printString("A", () => {    // 1. return "A"
    printString("B", () => {  // 2. return "B"
      printString("C",() => { // 3. return "C"
        // Callback Hell
      })
    })
  })
} // 해당 함수는 함수안에 callback으로 

const printAllNotUseCallback = () => {
  printString("A") // ?. return "A"
  printString("B") // ?. return "B"
  printString("C") // ?. return "C"
} // 해당 함수는 뭐가 먼저 끝날지 모르기 때문에(random) 실행마다 처리 순서가 다름

printAll()
```

printAllUseCallback과 같이 Callback 함수를 통해 리턴 결과를 받는다면 다음 함수를 실행하는 방식으로 순서를 제어할 수 있다. 그러나 Callback 함수를 사용하게 되면 printAllUseCallback의 주석 Callback Hell과 같이 예를 들어 알파벳 전부를 순서대로 출력하게 하려면 저러한 구조가 계속되면 지저분하고, 가독성도 좋지 않을 것이다. (이를 callback hell이라고 부른다.) 따라서 이후에 나오는 promise와 async/await를 주로 사용한다.



### 2. Promise

```js
const printString = ((string) => {
  return new Promise((resolve,reject) => { //여기서 resolve는 함수의 성공적인 반환값을 리턴하고 reject는 오류 발생이나 예외처리에 사용된다.
      setTimeout(
      () => {
        console.log(string)
        resolve()
      },
      Math.floor(Math.random() * 100) + 1
    )
  })
})

const printAll = () => {
  printString("A")
  .then(() => { //printString("A")의 return이 되면
    return printString("B")
  })
  .then((data) => { //data에는 return 시킨 printString("B")의 값이 전달될 수 있다.
    return printString("C")
  })
  //reject가 리턴 될 경우 .catch()로 예외 처리를 해줄 수 있다.
}

printAll()
```

위 코드와 같이 promise를 사용하면 동작은 callback과 같지만 .then으로 비동기의 순서 제어가 가능하기 때문에 비교적 깔끔한 코드를 작성할 수 있다. 그러나 마찬가지로 promise 역시 위와 같이 chaining을 활용하지 않는다면 .then이 누적되면서 promise hell이 발생할 수 있다. 

**Ex. Promise Hell**

```js
function goToSchool() {
  return new Promise((resolve,reject) => {
    setTimeout(() => {
      resolve('1. go to school')
    },100)
  })
}
function sitAndCode() {
  return new Promise((resolve,reject) => {
    setTimeout(() => {
      resolve('2. sit and code')
    },100)
  })
}
function eatLunch() {
  return new Promise((resolve,reject) => {
    setTimeout(() => {
      resolve('3. eat lunch')
    },100)
  })
}
function goToBed() {
  return new Promise((resolve,reject) => {
    setTimeout(() => {
      resolve('4. go to bed')
    },100)
  })
}

//Promise Hell
goToSchool()
.then(data => {
  console.log(data)
  sitAndCode()
  .then(data => {
    console.log(data)
    eatLunch()
    .then(data => {
      console.log(data)
      goToBed()
      .then(data => {
        console.log(data)
        // ...
      })
    })
  })
})
```





### 3. Async/await

Async/await는 NodeJs의 ES7부터 제공하는 함수로, 암시적으로 Promise를 사용하여 결과를 반환한다. 이를 사용하게 되면 앞서 사용한 callback과 promise보다 깔끔하고 간결한 코드를 작성할 수 있다.

```js
function goToSchool() {
  return new Promise((resolve,reject) => {
    setTimeout(() => {
      resolve('1. go to school')
    },100)
  })
}
function sitAndCode() {
  return new Promise((resolve,reject) => {
    setTimeout(() => {
      resolve('2. sit and code')
    },100)
  })
}
function eatLunch() {
  return new Promise((resolve,reject) => {
    setTimeout(() => {
      resolve('3. eat lunch')
    },100)
  })
}
function goToBed() {
  return new Promise((resolve,reject) => {
    setTimeout(() => {
      resolve('4. go to bed')
    },100)
  })
}

// async/await
const result = async () => { // await는 항상 async 함수 내부에만 쓸 수 있다!
  const first = await goToSchool();
  console.log(first)
  const second = await sitAndCode();
  console.log(second)
  const third = await eatLunch();
  console.log(third)
  const fourth = await goToBed();
  console.log(fourth)
}

result()
```



[Callback, Promise, async/await를 활용한 fs, fetch API 사용](https://github.com/apfl99/im-sprint-async-and-promise)

