# 3

## React Hook

리액트 훅은 v16.8에서 새로 추가된 기능으로, 함수형 컴포넌트에서 사용되는 몇 가지 기술들을 말한다.  
리액트 훅 등장이전에는 state관리 혹은 Life Cycle method를 사용해야 할 때  
클래스형 컴포넌트를 사용해야 했었다.

그러나 클래스형 컴포넌트는 코드가 길고 복잡해지며, 재사용이 어려운 등 여러가지 단점이 존재한다.  
이를 보완하여 함수형 컴포넌트에서 클래스형 컴포넌트의 기능을 사용할 수 있도록 해준다.

### 훅 규칙

공식 문서에 따르면 리액트 훅을 사용할 때 2가지 규칙을 준수해야 한다.

1. 최상위 레벨에서만 훅을 호출

   반복문, 조건문 혹은 중첩 함수 내부에서 훅 호출을 피해야 한다.  
   이 규칙을 준수해야 렌더링 될 때의 훅 호출 순서를 보장할 수 있다.  
   이를 통해 `useState` 혹은 `useEffect` 등이 여러번 호출되어도 상태를 올바르게 관리할 수 있다.

2. 오직 리액트 함수 내에서 훅을 호출해야 함

   일반적인 JavaScript 함수 내에서는 훅을 호출하지 말아야 한다.  
   그대신 리액트 컴포넌트 함수에서 호출하거나, 커스텀 훅을 작성할 때 리액트 훅을 호출해야 한다.

## Hooks

대표적인 리액트 훅의 종류는 아래와 같다.

### useState

컴포넌트에서 상태(State)를 관리하기 위해서 사용하는 훅이다.

useState를 호출하면 배열을 반환하며 내부에서 구조분해할당을 할 수 있다.  
첫번째 요소는 현재 상태, 두번째 요소는 상태를 업데이트하는 함수이다.

```js
import React, { useState } from 'react';

// 초기값이 0인 count 상태 생성
const [count, setCount] = useState < number > 0;

// count의 값을 +1
setCount(count + 1);
```

### useEffect

컴포넌트 안에서 데이터를 가져오거나 직접 DOM을 조작하는 작업을 "Side Effect"라고 한다.  
해당 작업이 다른 컴포넌트에 영향을 줄 수 있고, 렌더링 과정을 벗어난 작업이기 때문에 이런 이름이 붙었다.  
useEffect 훅은 화면이 렌더링 될 때마다, 특정 작업을 실행 할 수 있도록 하는 훅이다.  
컴포넌트가 mount, unmount, update 시 특정 작업을 처리 할 수 있다.  
즉 생명주기 메서드를 함수형 컴포넌트에서 사용이 가능하다.

기본적인 호출 방법은 `useEffect( () => { 익명함수 }, [의존성 배열])` 이다.  
의존성 배열에 담기는 값에 따라 다르게 동작한다.

- 의존성 배열 없을경우 -> 페이지가 렌더링 될 때 마다 함수 실행
- 의존성 배열에 빈 배열을 넣을 경우 -> 첫 렌더링에만 함수 실행
- 의존성 배열안에 props 속은 state를 지정할 경우 -> 해당 값이 변화하면 함수 실행

### useContext

먼저 Context란 컴포넌트 여러개에 거쳐 데이터를 공유할 수 있도록 해주는 개념이다.  
일반적으로 컴포넌트 사이 데이터 전달은 props로 이루어지지만,

Context를 사용하면 중간 컴포넌트를 거치치 않고 데이터를 전달 할 수 있다.  
원래 라면 `<AppContext.Consumer>` 와 같은 형태로 불러와서 context를 사용을 해야 했다.  
그러나 useContext를 사용하면 쉽게 context를 불러와서 사용할 수 있다.

```js
import React, { useContext } from 'react';
import { AppContext } from './App';

const Children = () => {
  // useContext를 이용해서 따로 불러온다.
  const user = useContext(AppContext);
  return (
    <>
      <h3>AppContext에 존재하는 값의 name은 {user.name}입니다.</h3>
      <h3>AppContext에 존재하는 값의 job은 {user.job}입니다.</h3>
    </>
  );
};

export default Children;
```

### useLayoutEffect

리액트에서 화면을 구성할 때 DOM Tree를 구성하는 `Render`와  
실제 화면에 Layout을 표시하고 업데이트 하는 과정인 `Paint`를 차례로 진행한다.

useEffect는 render와 paint가 모두 실행된 후에 동작하며, 작업은 비동기적으로 동작한다.  
paint가 된 이후에 실행되어 DOM이 실제 변경되는 작업이 있다면 화면이 깜빡이게 된다.

useLayoutEffect는 render가 되고 나서 실행된다. 그리고 그 다음으로 paint가 진행되며  
이 과정은 동기적으로 실행된다. 때문에 DOM을 변경하는 작업이 있어도 화면이 깜빡이지 않는다.

### useRef

컴포넌트 생애주기 전체동안 유지될 객체를 생성하는 훅.  
컴포넌트 마운트-언마운트 될 때 까지 동일 객체가 유지된다.

객체 자체가 동일한 것 뿐이지 값은 변경가능하다.  
즉 어떤 값을 참조할 수 있는 고정객체를 생성한다고 생각하면 될 것 같다.

어떤 값을 관리하기 위한 훅이라는 점에서 useState와 유사한 측면이 있긴하다.  
그러나 큰 차이점이 하나 있다. useState는 상태가 변경되면 해당 컴포넌트와 연관된 하위 컴포넌트까지  
다시 렌더링한다. 그러나 useRef로 참조되는 값은 변경되어도 렌더링에 영향을 주지 않는다.  
다만 어떤 이유로 다시 렌더링 되면 화면에는 객체의 현재 값이 반영되게 된다.

주로 input의 ID를 관리하는 등 컴포넌트 생명주기 동안 동일 값을 유지해야 하는 경우 사용한다.  
혹은 비동기 상황에서 현재 입력된 값을 가져와서 사용하고 싶은 경우 사용할 수 있다.

## React StrictMode

> `StrictMode`는 애플리케이션 내의 잠재적인 문제를 알아내기 위한 도구입니다.  
> 리액트 공식문서

이름에 느껴지듯이 리액트가 조금 더 깐깐하게 코드를 확인해주는? 모드이다.  
활성화하면 일부 문제나 잠재적인 오류를 더 쉽게 감지할 수 있다.

StrictMode 사용시 console.log( ) 등을 확인해보면 2번씩 기록되는 것을 볼 수 있다.  
이는 StrictMode에서 2번씩 렌더링 하면서 서로 비교하면서 체크하는 과정이 있는 것이므로 참고하자.

StrictMode는 개발 환경에서만 활성화 가능하며, 프로덕션 환경에서는 비활성화 된다.

```js
import React from 'react';

function ExampleApplication() {
  return (
  <React.StrictMode>
     <App />
  </React.StrictMode>,
  );
}
```

## usehooks-ts

리액트 훅으로 자주 사용하는 기능들을 이름을 좀 더 명확하게 하고,  
사용하기 쉽고 간단하도록 도와주는 라이브러리이다. 이름에서 알 수 있듯 타입스크립트로 만들어졌다.

[공식문서](https://usehooks-ts.com/)에서 어떻게 훅이 만들어져 있는지 모두 확인할 수 있다.

### useBoolean

True False 쉽게 토글할 수 있도록 도와주는 훅이다.  
value 와 toggle이라는 함수로 선언할 수 있다. 각 요소는 이름을 원하는 값으로 지정할 수 있다.

```jsx
// togglePlaying() 으로 playing 변수를 T/F 변화 가능
const { value: playing, toogle: togglePlaying } = useBoolean();
```

### useEffectOnce

이름에서도 알수 있듯이 useEffect를 마운트 시점에만 한번만 설정할 수 있다.  
원래라면 의존성 배열을 빈 배열을 지정을 해야 한번만 설정할 수 있으나, 해당 설정을 간단하게 할 수 있다.

### useFetch

url만 지정해서 간단하게 fetch API를 사용할 수 있다.

```js
export default function useFetchProducts() {
  const url = 'http://localhost:3000/products';

  const { data } = useFetch(url);

  if (!data) {
    return [];
  }
  return data.products;
}
```

### useInterval

setInterval 대신 사용할 수 있는 훅이다.
