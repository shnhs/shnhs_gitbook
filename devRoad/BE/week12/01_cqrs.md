# CQRS

## CQS란

CQS는 일종의 디자인 패턴중 하나이다.\
Commnad and Query Seperation. 한글로 풀어서 설명하면 '명령/조회 분리' 이다.

Command(명령)은 데이터의 상태를 변화시킨다. 단, 값을 반환하지는 않는다.\
Query(조회)는 상태를 변경하지 않고, 데이터만 조회하여 반환한다.

Command 와 Query를 분리한다 라는 패턴이므로\
CQS 패턴은 어떠한 메서드, 혹은 함수가 Command 나 Query 중 하나의 역할만 수행하게 하는 것이다.

## CQRS란

CQRS를 풀어서 쓰면 Command Query Responsibility Segregation이다.\
사실상 Command 와 Query를 분리하겠다 라는 개념은 동일한 것 같다.\
대신 CQS가 코드단위에서의 분리였다면, CQRS는 좀 더 큰 단위에서의 분리이다.

![CQRS](../../../devRoad/BE/week12/img/cqrs.png)

쉽게 말하자면 Command의 결과를 반영할 DB(그림의 Write Side쪽 DB)와\
Query를 통해 실제 사용자에게 보여줄 정보가 있는 DB(그림의 Read Side DB)를 분리한다.

분리된 각각의 DB에 대한 데이터 동기화는 Broker등을 이용해 누락없이 처리해주어야 한다.

## Materialized View

특히 Query는 한번 더 나아가서 Materialized View(MView)를 통해 좀 더 Query에 맞는 DB를 구성할 수 있다.\
MView란 쉽게 말해서 미리 계산한 테이블을 말한다.

논리적 View와는 다르게 실제 DB저장공간을 차지하는 물리적인 View 이다.\
조회할때 비용이 많이 드는 조인, 집계 작업을 미리 계산하고 그 결과를 별도의 테이블로 저장한 것이 MView 이다.

MView를 사용하면 Query요청에 대해 DB에서 조회하고 계산하는 시간이 단축되므로 성능상 이점을 가져갈 수 있다.

## OLTP와 OLAP

CQRS와 유사한것 같으면서도 다른 개념으로 OLTP와 OLAP가 있다.

### OLTP(Online Transaction Processing)

문자 그대로 풀이하면 온라인 트랜잭션 처리이다. 데이터 베이스의 일괄 트랜잭션 처리를 의미한다.\
사실상 우리가 보통 사용하는 트랜잭션 처리가 바로 OLTP라고 볼 수 있다.

동시에 일어나는 다수의 트랜잭션 처리를 위한 데이터 처리이다.\
큰 특징 중 하나로 트랜잭션 처리 과정중 어딘가에서 에러가 발생하면.\
해당 트랜잭션을 모두 무효로 하고 이전 상태로 RollBack 시킨다는 점이다.

OLTP에서는 주로 INSERT, UPDATE 같은 것을 처리하는것이 중점이다.

### OLAP(Online Analytical Processing)

문자 그대로 풀이하면 온라인 분석 처리이다.\
동일한 데이터에서 여러가의 인사이트를 뽑아낼 수 있도록, 데이터를 분석하기 쉽도록 도와준다.\
데이터 웨어하우스(전체 데이터)에서 필요한 정보들을 가지고 의미있는 정보로 치환할 수 있도록 해준다.

OLAP는 기능 자체 보다는, OLAP를 사용하는 목적과 주제에 큰 비중을 둔다.\
OLAP에서는 Command같은 처리 보다는 SELECT query를 통해 원하는 데이터를 뽑아오는 것이 주가 된다.

> DSS라는 개념이 있다. Decision Support System으로 사용자의 의사결정에 도움이 될 수 있는 시스템을 의미한다.\
> OLAP도 DSS의 한 종류 이다. DSS는 과정중에서 데이터를 새롭게 추가, 수정, 삭제해서는 안된다.\
> 현재 상태 그대로의 상태에서 조회, 결합 하여 진행되어야 한다.

## Event Sourcing

이벤트 소싱 이란 데이터의 저장방식 중 하나이다.\
보통의 경우에 비즈니스 로직에서 처리된 모든 이벤트 이후 최종의 데이터 상태만 저장되게 된다.\
하지만 이벤트 소싱은 순차적으로 발생한 모든 이벤트 자체를 저장한다.

사용자의 어떠한 행동, 요청을 이벤트라고 한다.\
그런 일련의 이벤트를 순서대로 이벤트 스토리지에 저장한다.\
그리고 이벤트 스토리지를 '이벤트 핸들러' 라는 것을 통해 현재 상태를 계산한다.

이벤트 소싱을 사용한다 하더라도 현재 상태는 기록된 이벤트를 통해서 계산되기 때문에\
데이터의 정합성에 문제가 있지는 않을 것이다.

이벤트 소싱에서 이벤트 기록이 너무 많이 쌓였다고 가정하면,\
현재 상태를 계산하는데 굉장히 많은 코스트가 들어갈 수 있다.\
때문에 이벤트 소싱은 '스냅샷'이라는 개념으로 특정버전 혹은 특정 기간마다 상태를 저장한다.\
그리고 어떤 시점의 상태가 필요하다 라고 하면 스냅샷을 한번 확인 후,\
없다면 가까운 스냅샷 이후의 이벤트를 처리하여 상태를 계산할 수 있다.



### 강의 간략 후기

CQRS와 이벤트 소싱 모두 실무에서 다뤄보지 않아 생소한 개념이었다.\
강의와 트레이너 분들의 언급에서도 실무에서 잘 사용하지 않을 수 있고,\
꽤 깊은 수준의 아키텍쳐에서 적용되는 어려운 내용이다 라고 하였다.

실제로 강의를 들으면서 한번에 머리에 와닿지는 않았다.\
실습도 따로 없어서 일단 따라 치고 이해한다 는 방법도 사용이 어려웠다.

그래도 여러 공식문서, 정리 글, 세미나 영상등을 보면서\
최소한 어떤 개념이다 정도의 감은 잡은 것 같다.
