# 1

## HTML DOM API

HTML에서 div, span, input 등의 요소를 DOM(Document Object Model) 이라고 한다.  
DOM API란 HTML DOM을 JavaScript로 제어하기 위한 API를 말한다.

화면에서 좀 더 다양한 동작을 구현하기 위해 직접 DOM을 조작하는 API를 사용한다.

HTML DOM API는 아래와 같은 기능 영역을 포함한다.

- DOM을 통한 HTML 요소에 대한 접근 및 제어
- Form 데이터에 대한 접근 및 조작
- HTML 미디어 요소(\<audio> 및 \<video>)에 연결되는 미디어 관리
- 웹 페이지에서 콘텐트 Drag & Drop
- 브라우저 탐색 기록에 대한 접근
- 다른 API에 대한 연결 지원

## Location

window.location 속성에 접근하여 Location 객체에 접근할 수 있다.  
해당 객체로 부터 현재 페이지의 위치와 관련된 정보를 얻을 수 있다.

`window.location.href` 은 현재 페이지의 URL을 반환한다.

`window.location.hostname`은 웹 호스트의 도메인 이름을 반환한다.

`window.location.pathname`은 현재 페이지의 URL 경로를 포함한 문자열을 반환.  
 경로가 없다면 빈 문자열을 반환.

```js
// 현재 URL 아래와 같다면
// https://developer.mozilla.org/en-US/docs/Web/API/Location/pathname#examples
console.log(location.pathname); // '/en-US/docs/Web/API/Location/pathname'
```

`window.location.protocol`은 사용된 웹 프로토콜(http: 또는 https:)을 반환한다.

`window.location.assign()`은 새 문서를 로드한다.

Location 객체에 완전한 URL 문자열을 할당할 경우 해당 주소로 브라우저를 이동시킬 수 있다.

```js
window.location = 'https://naver.com'; // 네이버로 이동
```

## History API

[자바스크립트의 History API와 클라이언트 단 라우팅 | Engineering Blog by Dale Seo](https://www.daleseo.com/js-history-api/)

브라우저의 세션 기록, 페이지 방문 이력을 제어하기 위한 웹 표준 API  
`history` 전역 객체를 통해 방문기록을 앞뒤로 탐색하거나, 특정 지점으로 이동할 수 있다.

현재 URL을 조작할 수 있도록 `pushState` 와 `replaceState` 메서드를 제공한다.  
해당 메서드는 클라이언트 라우팅에 큰 역할을 하게된다.

서버에 새로운 요청을 보내지 않고 URL을 바꿔주므로, 스크립트로 화면을 새롭게 업데이트할 수 있게 해준다.
