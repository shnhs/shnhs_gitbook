# 1

## REST & GraphQL

### REST API란

먼저 API란 무엇인가 부터 정리를 해본다.

API란 `Application Programing Interface`의 약자로,  
어플리케이션 혹은 소프트웨어 사이의 통신하기 위한 규약을 의미한다.  
더 쉽게 말해 정해진 형식으로 요청을 주면 알맞게 처리하여  
원하는 응답을 리턴하겠다 라고 정한 약속, 정의 같은 개념을 의미한다.

그렇다면 REST는 무엇일까.  
REST는 `Representational State Transfer`의 약자로,  
웹 개발에서 사용되는 소프트웨어 아키텍처 중 하나이다.  
REST는 리소스를 표현(Represent)하여 그 자원에 대한 상태(State)를 주고받는 것을 기본으로 한다.

REST에서 사용하는 몇 가지 개념을 간단하게 정리하면 아래와 같다.

- 자원(Resource)  
  데이터로 나타낼 본질적인 것을 의미한다.  
  예를들어 사용자 목록을 받아오는 REST API가 있다고 가정할 때, '사용자'를 API가 다루는 리소스라고 볼 수 있을 것이다.
- 표현(Represent)  
  자원의 상태를 주고받을 떄 사용되는 형식을 의미한다.  
  대부분의 경우 JSON 혹은 XML 형식을 사용한다.
- URI  
  자원을 고유하게 식별하는 주소를 의미한다. 고유한 식별자를 통해 원하는 자원을 주고받을 수 있다.  
  API 명세 중 /users, /books/{id} 와 같은 것을 URI라고 한다.
- Method  
  HTTP 프로토콜의 GET, POST, DELETE, PUT 등 메서드를 사용한다.  
  보통 GET을 어떤 자원을 조회하는데 사용하고, POST는 자원을 생성할 떄 사용하는것이 보편적이다.

### GraphQL이란

GraphQL은 API를 위한 쿼리 언어이다.  
REST API가 정해진 형식의 응답을 받을 수 밖에 없으나,  
GraphQL은 클라이언트에서 필요한 응답을 명시적으로 요청할 수 있어  
불필요한 데이터를 호출하지 않아 효율적인 데이터 관리가 가능하다.

GraphQL의 주요 특징은 아래와 같다.

- 구체적 요청  
  API에 쿼리를 보낼 때 원하는 데이터를 구체적으로 명시하여 요청할 수 있다.  
  때문에 GraphQL은 항상 예측 가능한 결과를 반환한다.
- 단일 요청  
  REST API에서는 원하는 리소스별로 여러개의 엔드포인트가 필요하다.  
  그러나 GraphQL은 하나의 엔드포인트만 두고 원하는 데이터를 명시적으로 요청한다.
- 타입 지정  
  GraphQL 쿼리에서 각 데이터에 대한 타입과 구조를 미리 지정할 수 있다.  
  타입을 지정하여 요청을 명확하게 하고, 에러를 미리 예측할 수 있다.

## JSON

JSON(JavaScript Object Notation)은 데이터 교환을 위한 경량의 데이터 형식이다.  
사람과 기계 모두가 이해하기 쉽도록 설계된 텍스트 기반의 데이터 형식으로,  
주로 웹에서 데이터를 전송하거나 구조화된 데이터를 저장하기 위해 사용된다.

JSON은 아래와 같이 Key:Value 쌍으로 구성된다.

```json
{
  "name": "John",
  "age": 30,
  "city": "New York"
}
```

## React Component

리액트의 주요 특징 중 하나가 바로 '컴포넌트' 베이스로 되어있다는 점이다.  
컴포넌트란 앱을 구성하는 최소한의 단위로, 재사용 가능한 UI조각을 의미한다.  

개별적인 컴포넌트는 JavaScript 함수와 유사한 구조이다.  
화면에 보여줄 JSX, 스타일을 위한 css,  
그리고 약간의 로직이 필요할 경우 JavaScript로 구성된다.  
props를 통해 input을 받고, 화면에 어떤 표시를 보여줄지 나타내는 React 엘리먼트를 반환한다.  

```ts
import type Product from '../types/Product';

export default function ProductRow({product}: {product: Product}) {
 return (
  <tr>
   <td>
    <span style={{color: product.stocked ? '#000' : '#F00'}}>
     {product.name}
    </span></td>
   <td>{product.price}</td>
  </tr>
 );
}
```

리액트에서 컴포넌트는 '선언적 방식'으로 만들어진다.  
어떻게 해라 라고 명시하는 명령적 방식 대신에 최종적으로 원하는 목표 상태와  
어떤 상태에서는 어떻게 로직을 처리해야하는지를 정의하여 리액트에게 알려주는 것이다.  

### prorps

컴포넌트를 재사용할 때나 컴포넌트 사이의 데이터 전달이 필요할 경우 사용하는 개념.  
어떤 컴포넌트를 호출 할 때 props로 데이터를 넘겨주면,  
해당 컴포넌트 내에서 넘겨준 데이터를 가지고 뭔가 처리하여 화면에 보여주거나, 혹은 다시 자식 컴포넌트로 값을 넘겨줄 수 있다.  
