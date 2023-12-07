# 2

## Fetch API

Fetch API는 JavaScript에서 HTTP 통신을 위해 제공하는 기본 API 이다.  
Promise 기반으로 설계되어 간결하면서도 강력한 기능을 제공하며, XMLHttpRequest의 대안으로 사용된다.  
JSON 형식의 데이터를 주고받는 데 특히 유용하며, 브라우저 및 Node.js에서 사용할 수 있다.

## Promise

Promise는 비동기 작업을 처리 및 관리하기 위한 JavaScript 객체이다.

비동기 작업이란 특정 코드가 끝나는 것을 기다리지 않고, 나머지 코드를 먼저 실행하는 작업.  
빠른 페이지 로딩을 위해 웹 개발에서 비동기 작업을 사용한다.

Promise는 3가지 상태를 가진다.

- 대기(Pending) - 비동기 함수가 아직 시작하지 않음
- 성공(Fulfilled) - 비동기 함수가 성공적으로 완료
- 실패(Rejected) - 비동기 함수가 실패

Promise가 대기상태에서 상태가 변화하면 `then( )` , `catch( )` 함수를 사용해서  
각각 성공, 실패 상태를 처리할 수 있다.

## async / await

Promise를 좀 더 편하게 사용할 수 있도록 도와주는 문법이다.

function 앞에 `async` 키워드를 붙이면, 해당 함수는 항상 Promise를 반환한다.  
Promise가 아닌 값을 반환하는 함수여도 Promise로 감싸서 반환한다.

`await` 는 async 함수 안에서만 동작하는 문법이다.  
await를 만나면 Promise가 처리될때 까지 대기하게 된다. 그리고 결과는 그 이후에 반환한다.

## ReadableStream

읽히는 그대로 읽을 수 있는 Stream을 만들때 사용하는 클래스이다.

특히 fetch 로 얻어온 응답객체의 `body` 메서드를 통해  
ReadableStream으로 청크 단위로 읽어서 처리할 수 있다.

## Unicode

유니코드는 모든 문자를 컴퓨터에서 일관되게 표현하고 다룰 수 있도록 설계된 국제 표준 문자 인코딩 시스템이다.  
문자 집합을 비롯한 다양한 문자와 기호에 대한 코드 포인트를 할당하고 정의하는 표준이다.

## CORS

기본적으로 브라우저는 동일 출처 정책(Same Origin Policy) 에 따라  
다른 출처에서 얻은 리소스를 사용할 수 없도록 막는다.  
여기서 출처(Orgin)란 URL의 “스킴(프로토콜) + 호스트 / 도메인 + 포트” 로 정의된다.  
두 객체의 스킴, 호스트, 포트가 모두 일치할 경우에 같은 출처라고 말한다.

> 유의해야할 점은 동일 출처 정책에 의해 막혔었도, 실제 통신은 이미 완료된 상태라는 것이다.  
> GET으로 가져오는 정도는 문제가 없을 수도 있으나,  
> POST, DELETE 등의 요청 자체는 이미 전달되었음을 유의해야 한다.

때문에 다른 출처로부터 얻은 리소스를 사용하기 위해서는  
교차 출처 리소스 공유(Cross-Origin Resource Policy; CORS) 설정해야한다.  
추가적인 HTTP 헤더를 사용하여 다른 출처의 리소스에 접근 가능한 권한을 부여한다.

e.g. `https://domain-a.com`의 프론트 엔드 JavaScript 코드가  
`https://domain-b.com/data.json`을 요청하는 경우에 보안상의 이유로 교차 출처 HTTP 요청을 제한한다.

혹은 서버에서 요청에 대한 응답에 CORS 관련 헤더를 추가하여,  
브라우저가 안전하게 교차 출처 자원에 접근할 수 있도록 설정 할 수 있다.  
주요 헤더로는 **`Access-Control-Allow-Origin`**, **`Access-Control-Allow-Methods`**, **`Access-Control-Allow-Headers`** 등이 있다.
