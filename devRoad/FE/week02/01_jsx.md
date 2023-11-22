# JSX

## TL;DR

- JSX는 JavaScript의 확장문법
- JavaScript 코드 안에서 HTML 같은 XML 코드 사용가능
- 리액트에서 꼭 JSX를 써야하는건 아니지만 쓰는게 편하다.

## JSX - JavaScript XML

JSX는 React.createElement의 syntactic suger이다.  
좀 더 사람이 읽고 쓰기 편한 형태로 리액트 코드를 작성할 수 있게 해준다.  
JSX를 통해 개발자는 렌더링할 UI를 선언적으로 작성한다.

### 기본형태

아래와 같은 형태가 가장 기본적인 JSX 코드 형태이다.

```js
const element = <p>Hello, World!</p>;
```

JSX 코드는 결국 1:1로 자바스크립트 코드로 매핑된다.  
위의 JSX 코드를 자바스크립트로 변환하면 아래와 같다.

```js
const elemet = React.createElement('p', null, 'Hello, world!');
```

### React.createElement

JSX 코드를 자바스크립트로 컴파일하는 핵심함수이다. 리액트 엘리먼트를 생성한다.  
함수 형태는 아래와 같다.

```js
React.createElement(type, [props], [...children]);
```

- `type`에는 엘리먼트의 타입을 지정한다.
- `props`는 엘리먼트의 속성을 지정합니다.  
  선택적 매개변수로 사용하지 않는다면 null로 생략할 수 있습니다.
- `children`은 내부의 자식 엘리먼트를 나타내는 매개변수입니다.  
  가변인자로, 여러개의 자식 엘리먼트를 지정할 수 있습니다.

## JSX 문법

### 기본

기본적인 JSX 코드이다.  
최종적으로는 React.createElement 함수를 사용하는 자바스크립트 코드로 변환된다.

```jsx
// jsx
const element = <h1>Hello, World!</h1>;

// 변환된 자바스크립트
const element = React.createElement('h1', null, 'Hello, World!');
```

### 자바스크립트 표현식

`{ }` 중괄호를 사용하여 자바스크립트 표현식을 사용할 수 있다.  
유의할 점은 중괄호로 사용된 자바스크립트 표현식은 createElement에서 별도의 자식 엘리먼트로 취급된다.

```jsx
// jsx
const name = 'John';
const element = <p>Hello, {name}!</p>;

// 변환된 자바스크립트
const name = 'John';
const element = React.createElement('p', null, 'Hello, ', name, '!');
```

### 속성과 이벤트 처리

HTML과 유사한 방식으로 속성과 이벤트를 처리할 수 있다.

```jsx
// jsx
const handleClick = () => {
  alert('Button clicked!');
};

const element = <button onClick={handleClick}>Click me</button>;

// 변환
const handleClick = () => {
  alert('Button clicked!');
};
const element = React.createElement(
  'button',
  {
    onClick: handleClick,
  },
  'Click me'
);
```

### 여러개의 자식 엘리먼트

여러개의 자식 엘리먼트가 있다면,  
React.createElement 함수에 각 엘리먼트가 순서대로 인자로 붙게된다.

```jsx
// jsx
<div className="test">
  <p>Hello, world!</p>
  <Button type="submit">Send</Button>
</div>;

// 변환
React.createElement(
  'div',
  { className: 'test' },
  React.createElement('p', null, 'Hello, world!'),
  React.createElement(Button, { type: 'submit' }, 'Send')
);
```

---

## 자투리

### Syntactic sugar

한국말로 직역하면 문법적 설탕이다.  
기능은 그대로 이지만 사람이 읽고 쓰기 편하게 디자인된 문법을 의미한다.

### React Element

엘리먼트는 React 앱의 가장 작은 단위이다.  
각 엘리먼트에 화면에 표시할 내용을 작성하게 된다.

React DOM이 엘리먼트와 화면의 DOM을 일치하도록 관리하면서 업데이트 한다.

헷갈리는 개념으로 컴포넌트가 있다.  
공식 문서에 따르면 엘리먼트는 컴포넌트의 구성요소 중 하나라고 한다.  
정확히는 `컴포넌트는 화면에 어떻게 표시될지 기술하는 리액트 엘리먼트를 반환한다` 라고 생각하면 될 것 같다.

### 선언적 작성

선언적 프로그램이란 어떻게 해라 명령하는 것 대신에  
최종적으로 원하는 목표상태와 결과를 명시하는 프로그래밍 방식을 의미한다.  
코드에서 그 목표를 달성하는 과정이나 방식은 구체적으로 알려줄 필요가 없다.

리액트의 JSX가 대표적인 선언적 프로그래밍의 예시이다. JSX에서는 컴포넌트를 선언하고  
렌더링 이후에 우리가 원하는 최종적인 UI의 모습을 명시한다.  
명시된 모습으로 보고 리액트가 알아서 VDOM을 활용하여 실제 DOM을 업데이트 한다.
