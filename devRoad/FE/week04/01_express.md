# 1

## Nods.js

먼저 Node.js 가 뭔지 간략하게 정의한다.

> Node.js is an open-source, cross-platform JavaScript runtime environment.  
> Node공식 사이트

쉽게 말해 Node.js 는 JavaScript를 브라우저가 아니라,  
서버에서 사용할 수 있게 해주는 런타임(프로그램 실행 환경)이다.

즉 Node를 통해 다양한 JavaScript 애플리케이션을 실행할 수 있다.

## Express

> Node.js를 위한 빠르고 개방적인 간결한 웹 프레임워크  
> Express 공식 사이트

Express는 Node.js를 이용하여 쉽게 웹 어플리케이션,  
서버를 만들기 위한 틀(Frame)을 제공하는 라이브러리와 클래스의 집합이다.

단순 라이브러리가 아닌 프레임워크이다.  
웹 어플리케이션을 만들기 위한 각종 라이브러리 및 미들웨어가 이미 내장되어 있어 사용하기 편리하고.  
지켜야 하는 방식이 있기 때문에 일관된 구조를 유지할 수 있다.

## REST API

REST API(Representational State Transfer API)는 네트워크 상에서 자원(리소스)을 표현하고,  
이를 CRUD(Create, Read, Update, Delete) 기능을 통해 상태를 전송하는 아키텍처 스타일이다.  
HTTP 프로토콜을 기반으로 하며, 간결하고 확장 가능한 특징으로 웹 서비스를 구현하는 데 보편적으로 사용된다.

## URL 구조

먼저 URL(Uniform Resource Locator)는 리소스의 ‘위치’를 표시한다.  
”어떤 위치로 가면 어떤 리소스가 있다”를 나타내는 역할을 한다.  
e.g. '<https://www.naver.com/index.html?page=1232950&id=776>'

단 위치 변경에 취약하여 한 글자라도 잘못 쓰게 되면 리소스 위치를 찾을 수 없다.
특정 웹페이지에 접속하게 위해서는 프로토콜과 주소를 모두 올바르게 입력해야 접속이 가능하다.

URI(Uniform Resource Identifier)와 혼동할 수 있는 개념이다.
URI는 리소스를 식별할 수 있는 방법을 뜻한다.
URL은 URI의 한 종류라고 할 수 있다. 그러나 보통은 둘을 비슷한 의미로 혼용하여 사용한다.

URL 구조는 아래와 같이 구분할 수 있다.

![URL](/devRoad/FE/week04/URL.png)

## HTTP Method - CRUD

HTTP 메서드란 클라이언트와 서버간 이루어지는 통신에서,  
리소스에 대해 수행할 요청, 서버가 어떤 동작을 할지 지정하는 방법이다.

보통은 CRUD에 맞게 5개의 Method를 사용한다.

- GET - 리소스 조회(READ)
- POST - 요청 데이터 처리. 주로 새롭게 등록할 때 사용(CREATE)
- PUT - 리소스 덮어쓰기. 요청한 리소스가 없다면 새롭게 생성(UPDATE)
- PATCH - 리소스 부분 변경(UPDATE)
- DELETE - 리소스 삭제(DELETE)

이 밖에서 많이 사용하지는 않으나 몇 가지 메서드가 더 있다.

- HEAD - GET과 동일하나, Body를 제외한 상태 + 헤더 만 반환
- OPTION - 리소스에 대한 통신 가능 메서드를 반환
- TRACE - 요청을 다시 반환하는 루프백 테스트 기능
