# 3

## Redux

[Redux(리덕스)란? (상태 관리 라이브러리) - 하나몬 (hanamon.kr)](https://hanamon.kr/redux%EB%9E%80-%EB%A6%AC%EB%8D%95%EC%8A%A4-%EC%83%81%ED%83%9C-%EA%B4%80%EB%A6%AC-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC/)

Redux는 JavaScript의 상태 관리 라이브러리이다. 메타가 설계한 FLUX 규격에 맞춰서 구성되어 있다.  
Redux는 애플리케이션의 상태를 중앙 집중화하고, 이를 통해 undo/redo, 상태 지속성 등의 강력한 기능을 가능하게 한다.

왜 굳이 상태 관리가 필요할까?  
리액트에서 상태란 기본적으로 컴포넌트 내에서 관리되기 때문에 자식 컴포넌트 사이에 직접 데이터 전달이 불가능하다.  
때문에 자식 컴포넌트에서 부모 컴포넌트로 상태를 올려보내고 다시 내려받아야 한다.  
이때 자식 컴포넌트가 구조가 복잡하거나 많아지면 내려받을 상태가 많아지고 굳이 필요하지 않아도 상위 컴포넌트에서 계속 내려 받아야한다(Props drilling 이슈)  
이때 상태관리 툴을 사용하여 전역적인 상태 저장소를 만들어 이슈를 해결하고자 하였다.

Redux는 예측 가능한 상태 관리 제공, 중앙화 된 상태 관리 저장소 제공, 많은 Addon 기능 제공 등의 특징을 가지고 있다.

## Reflect

Reflect API는 JavaScript의 내장 API로,  
객체의 메서드, 프로퍼티, 생성자 등을 객체 내부 구조에 직접 접근하여 조작할 수 있도록 하는 API이다.

Reflect API는 ECMAScript 2015에서 처음 도입되었다.  
일반적으로 JavaScript에서 객체의 메서드, 프로퍼티, 생성자 등을 조작하려면 해당 객체로 접근하여 사용해야 했다.  
이는 간단하지만 객체의 내부 구조를 알고 있어야 가능한 방법이다.  
Reflect API를 사용하면 객체 내부의 프로퍼티에 직접 접근하여 사용을 할 수 있다.

```js
const object = {
  name: "홍길동",
  age: 30,
};

// Reflect.get( ) 으로 프로퍼티 직접 가져옴
const name = Reflect.get(object, "name");

console.log(name); // "홍길동"
```
