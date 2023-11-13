# CRUD

## CRUD란?

API 설계에서 자주 쓰이는 개념? 인 CRUD를 먼저 정리해보자.  
Create, Read, Update, Delete 의 앞자를 따서 CRUD라고 부른다.  
위의 4가지 요소로 데이터에 취할 수 있는 모든 기능을 분류할 수 있다.  
API가 갖추어야 할 기능(데이터 참조, 검색, 갱신)을 가리키는 용어로 사용할 수도 있다.

Create : 데이터를 새로 생성한다.  
Read : 데이터의 현재 상태를 읽어온다.  
Update : 데이터를 상태를 새롭게 갱신한다.  
Delete : 데이터를 제거한다.

## CQS

CQS은 소프트웨어 디자인 패턴 중 하나이다.  
디자인 패턴이란 소프트웨어 개발 중 복잡성을 줄이고 재사용성을 좋게 하는 등  
여러 장점이 있는 설계 패턴을 말한다.  
그중 위에서 설명한 CRUD와 유사한 패턴인 CQS 패턴을 정리한다.

CQS는 Command Query Seperation 의 약자이다.  
단어 그대로 `Command` 와 `Query`로 2개 중 하나로 구분하는 디자인 패턴이다.  
Command는 어떤 작업을 수행하는 메서드, Query는 어떤 데이터를 반환하는 개념이다.

### Command

CRUD 중 Create, Update, Delete가 Command 개념에 해당된다.  
Command는 상태가 변화시킨다. 그러나 어떤 값을 반환하지는 않는다.  
객체의 상태를 변화시키는 메서드이기 때문에 안전하지 않다.

### Query

CRUD 중 Read가 Query에 해당한다.  
Query는 상태를 변화시키지는 않는다. 그리고 어떤 결과 값을 반환한다.  
상태를 변화시키지 않기 때문에 부작용으로 부터 자유롭다.

CQS 패턴으로 가지는 장점으로는 읽기 로직과 쓰기(수정) 로직을 분리하여  
성능을 최적화 시키고, 유지보수를 편하게 할 수 있다.  
CQS 패턴으로 인한 단점은 오히려 복잡한 코드 구현을 해야 할 경우가 생길 수 있다.  
특정 기능중에 조회와 쓰기(수정)을 동시에 할 경우가 있을 수 있다.  
예를들면 글을 읽으면 조회수가 +1 되는? 경우가 있을 수 있다.  
이런경우 CQS 패턴을 유지하기 위해 간단한 로직을 오히려 2개의 조각으로 나누어 복잡하게 구현해야 할 수도 있다.

## HTTP 메서드 적용

이전에 한번 정리한 HTTP 메서드도 CRUD에 대응하여 행동을 지정할 수 있다.

GET : Read, 리소스를 읽어오기  
POST - Create, 리소스를 생성하기  
PUT, PATCH - Update, 리소스를 갱신하기  
DELETE - Delete, 리소스를 제거하기

> 이전에 한번 설명한 것 처럼 현업에서는 POST를 새로운 리소스(데이터) 생성을 하는데 사용한다.
> 그리고 PUT과 PATCH를 같이 리소스 수정으로 묶었으나,
> PUT은 전체 업데이트, PATCH는 부분 업데이트로 엄밀하게 구분하고 있음을 유의하자.

보통 API를 설계할 때 Http 메서드와 컬렉션 패턴을 사용하여  
`리소스 단위`로 설계하게 된다.  
그리고 특정 요청 효과에 대해 컬렉션 리소스에 대한 요청인지,  
개별 항목(elements) 에 대한 요청인지에 대해 다르게 설계 할수 있다.

아래에 고객과 주문에 대한 API설계 예시이다.
|리소스|GET|POST|PUT|DELETE|
|------|---|---|--|--|
|/customers|모든 고객 검색|새로운 고객 생성| 고객 (대량) 업데이트 | 모든 고객 삭제
|customers/1|고객 아이디가 1인 고객 세부정보 검색 |-| 고객 아이가 1인 고객 정보 업데이트 | 고객 아이디가 1인 고객 제거
|customers/1/orders|고객 1의 모든 주문 검색|고객 1의 새로운 주문 생성|고객 1의 주문 (대량) 업데이트 | 고객1의 모든 주문 제거

> bulk 단위(대량)로 업데이트 하거나 삭제하기도 하는데,  
> 개별 단위를 여러번 처리할 수도 있고, collection으로 처리할 수도 있다.  
> 하나의 방식으로 정의하고 API 명세에 정확히 기록하는 것이 좋다.

## 로그인 로그아웃 리소스 표현

추가로 생각해 볼 수 있는 개념으로 로그인과 로그아웃이 있다.  
로그인과 로그아웃은 어찌보면 추상적인 개념이다.  
프로그램에서는 세션이라는 개념을 활용하여 표현 할 수 있다.

- POST /session  
  새로운 세션을 생성한다. 즉 로그인 한다 라고 볼 수 있다.  
  꼭 세션이 아니라도 토큰같은것을 사용할 수도 있다.  
  세션키나 토큰을 주고받으면서 누군지 식별한다.

- DELETE /session.  
  세션을 삭제한다. 즉 로그아웃 한다 라고 볼 수 있다.  
  존재하던 세션을 삭제하여 더 이상 해당 세션키에 대한 정보를 식별할 수 없게되어  
  로그아웃 하는 상태를 구현 할 수 있다.

> 참고자료  
> <https://learn.microsoft.com/ko-kr/azure/architecture/best-practices/api-design#organize-the-api-design-around-resources>  
> https://learn.microsoft.com/ko-kr/aspnet/core/data/ef-mvc/crud?view=aspnetcore-7.0  
> https://velog.io/@dev_bomdong/TIL-CRUD와-HTTP-요청-메소드  
> https://medium.com/@su_bak/cqs-command-query-separation-pattern-이란-f701eabf8754
