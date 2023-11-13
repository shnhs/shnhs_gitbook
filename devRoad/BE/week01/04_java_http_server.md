# java http server

## 개요

이전에 구현했던거는 저수준 API였다.  
java에서 고수준의 API로 비교적 간단하게 웹서버를 구현 할 수있도록 지원한다.  
내부적으로 NIO을 사용하고 있어, 더 효율적인 서버를 구현이 가능하다.

## 서버 객체 준비

`HttpServer` 타입의 서버 객체를 생성한다.  
파라미터로 `InetSocketAddress` 객체를 생성하여 설정한다.  
보통 포트만 지정해 주면 된다.

```java
InetSocketAddress address = new InetSocketAddress(8080);
// 리스너 생성
HttpServer httpServer = HttpServer.create(address, 0);
```

위처럼 서버만 listen상태로 열어두고 클라이언트 요청을 보내보면  
무언가 응답이 나온다. 자바에서 기본적으로 구현해 놓았다.

## URL 핸들러 지정

정확히는 path에 대한 핸들러(처리)를 지정한다.  
서버 객체에 `createContext 를 사용한다.  
해당 메서드는 context와 핸들러를 파라미터로 받고,  
핸들러는 람다식으로 구현할 수 있다.

> Handler  
> 인터페이스 중에서 메서드가 하나 밖에 없는거는 람다를 쓸수있다...??  
> (람다 조건이 따로 있는거는 몰랐다. 찾아보고 공부가 필요하다.)

```java
  httpServer.createContext("/", exchange -> {
    // 무언가 처리 후 응답
  });
```

핸들러를 지정하고 request에 대한걸 처리하고 응답하면 된다,

## 응답하기

바디에 넣을 데이터를 바이트로 준비한다.  
시작라인에 넣을 상태코드와 응답길이를 지정해야한다.

```java
// 응답을 바이트로
String content = "hello world\n";
byte[] bytes = content.getByte();

exchange.sendResponseHeaders(200, bytes.len);
OutputStream outputStream = exchange.getResponseBody();
outputStream.write(bytes);

outputStream.flush();
```
