---
title: "[React] Effect Hook"
categories:
 - BEB
tags: [] 
toc: true
author_profile: true #profile sidebar 감추기
# sidebar:
#     nav: "docs" # 목차 사이드바
search: true #검색 피하기
---



# Side Effect(부수 효과)

------------------------

Side Effect : 함수 내에서 어떤 구현이 함수 외부에 영향을 끼치는 경우 해당 함수는 Side Effect가 있다고 말한다.

```js
function SingleTweet({writer, body, createdAt}) {
  return <div>
    <div>{writer}</div>
  	<div>{createdAt}</div>
  	<div>{body}</div
}
```

위와 같은 함수는 단순히 props가 입력으로 JSX Element가 출력으로 나가기 때문에 어떠한 Side Effect도 없다. 그러나 보통 React 애플리케이션을 작성할 때는, AJAX 요청이나 API를 사용하게 된다. 이는 React 입장에서 모두 Side Effect이며, 이를 다루기 위해 Effect Hook를 제공한다.



# Effect Hook

------------------------

```js
import React, { useState, useEffect } from 'react';
function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => { //Side Effect 처리
    document.title = `You clicked ${count} times`;  
  });
  
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

위 코드와 같이 React에서는 Side Effect 처리를 위해 useEffect()라는 함수를 제공한다.



## useEffect

- useEffect는 해당 컴포넌트가 렌더링 이후에 어떤 일을 수행해야 하는 지 명시한다.
- useEffect는 effect를 통해 state, prop에 접근이 용이하게 하기 위해 component안에서 불러낸다.
- 기본적으로 useEffect는 모든 렌더링 이후에 수행되며, effect가 수행되는 시점에서는 이미 DOM이 업데이트된 상태이다.
  - React 랜더링 시점
    - 컴포넌트 생성(부모 or 자신)
    - 컴포넌트에 새로운 props의 전달
    - 컴포넌트의 상태 변화



### 조건부 실행(Effect 성능 최적화)

------------------------

useEffect의 두 번째 인자는 배열로, 해당 배열은 조건을 담는다. 여기서 조건은 if문과 같은 boolean 형태가 아닌 해당 배열에 변경이 일어날 때 useEffect를 조건부 실행할 수 있다.

(useEffect안에 setState함수를 넣어 사용할 때 렌더링이 무한루핑할 때, 해당 조건을 걸어줌으로써 루핑 방지가 가능하다. )

Ex.

```js
const [text,setText] = useState("test")

useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]); // count가 바뀔 때만 effect를 재실행합니다.
```

위 코드와 같이 useEffect의 조건이 아닌 text가 변경되어 리렌더링 될 때에도 useEffect의 조건인 count의 값이 변화하지 않았기 때문에   useEffect는 실행되지 않는다. 

- 두 번째 인자의 배열 값이 여러 개일 경우?
  - 하나만 바뀌어도 React는 useEffect를 실행한다.

- 두 번째 인자가 빈 배열([])일 경우 ?
  - 컴포넌트가 처음 생성될 때만 useEffect를 실행한다.
- 두 번째 인자에 아무것도 넣지 않았을 경우
  - 기본 형태로, 렌더링시마다 실행된다.



[Effect Hook를 활용한 예제](https://github.com/apfl99/im-sprint-statesairline-client)

[State Hook를 활용한 예제](https://github.com/apfl99/im-sprint-cmarket-hooks)
