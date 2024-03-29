# TCP/IP 통신

원래는 인터넷 프로토콜 스위트 이지만 흔히 TCP/IP라고 부른다.  
인터넷 프로토콜 스위트에 여러 프로토콜이 있으나  
전송제어프로토콜(TCP; Transmission Control Protocol)과  
인터넷 프로토콜(IP; Internet Protocol)이 제일 유명하고 널리 쓰이기 때문이다.

OSI 7계층상 TCP/IP를 기반에서 HTTP가 올라간다.

## 전송계층

OSI 7계층 중 4계층을 전송계층이라고 한다.  
여러 종류가 있다만, 제일 유명한거는 TCP와 UDP가 있다.

### TCP

서로 연결하고 데이터를 보내는 방식이다.  
`연결`하고 전달하는 과정에서 `순서를 보장한다.`

쉽게 예를 들면 전화와 같다.  
전화는 서로 통화를 걸고 받아야 (연결되어야) 서로 대화가 가능하고  
서로의 대화 과정에서 대화의 순서가 보장된다.

### UDP

아무튼 데이터를 보내는 방식이다. TCP와 다르게 연결이 꼭 필요하지는 않다.  
그렇기 때문에 전달이 제대로 되는지, 순서가 맞게 갔는지 보장하지 않는다.

UPD는 예를들면 편지와 같다.  
보내는 사람이 꼭 받는 사람과 시간을 맞춰야(연결되어야) 편지를 보낼 수 있는게 아니라  
그냥 일단 편지를 보낼 수 있다. 물론 편지가 가는 와중에 각 편지의 순서가 보장되지는 않는다.

웹에서는 TCP를 보통 사용한다.  
그러나 전달 여부와 순서를 보장하는 방식으로 프로그래밍하여 UDP방식을 사용 할 수도 있다.

## 소켓

일단 소켓과 소켓API를 구분해야 한다.

- 소켓(네트워크 소켓)  
  엔드포인트(프로세서의 종착지라고 이해라고 생각하면 편하다)를 의미한다.

- 소켓API  
  소켓(엔드포인트)끼리 어떻게 통신할 지에 대한 API 혹은 그에 대한 개발을 의미한다.

`소켓API`를 통해서 `소켓`을 다룬다 라고 정리 할 수 있다.

소켓은 마치 파일과 유사하다.  
파일처럼 열고 읽고 쓰고 닫을 수 있다.  
자바에서는 키보드 입력, 화면 출력, 파일 입출력 과 마찬가지로 `stream`을 통해서 소켓을 다룰 수 있다.

## TCP 통신 순서

마치 전화 걸고 받는것과 유사하다.  
클라이언트는 전화를 거는 입장, 서버는 전화를 받는 입장이다.

1. Listen (서버가 전화 받을 준비)  
   서버는 접속요청을 위한 소켓을 오픈한다. (단 이 소켓은 단지 요청을 받기위한 소켓이다)  
   접속 요청을 받는다.  
   쉽게? 말해 포트를 개방한다고 생각하면 된다.

2. Connect (클라이언트가 전화 걸기)  
   클라이언트가 소켓을 오픈하고 서버에 접속을 요청한다.

3. Accept (서버가 전화 받기)
   서버는 접속 요청을 받아, 클라이언트와 통신할 소켓을 따로 만든다.  
   앞서 1단계에서 준비한 소켓과는 다른 소켓임을 유의해야 한다.  
   1단계에서 만든 소켓은 요청을 받는 소켓일 뿐 통신용은 따로 만들어야 한다.

4. Send & Recieve 반복 (클라이언트 - 서버 대화)  
   소켓을 통해 서로 데이터 주고받음  
   데이터를 한 번만 주고받을수도 있고, 여러번 주고 받을 수도 있다.

5. Close (전화 끊기)  
   통신이 끝나면 소켓을 닫는다.  
   서버나 클라이언트 둘 중 아무나 소켓을 닫으면  
   반대쪽 Recieve에서 해당 사실을 알고 알아서 통신이 종료된다.

## http 클라이언트 코드로 구현해보기

### 서버와 연결

기본적으로는 소켓을 먼저 만들고 연결하는 개념이지만  
자바에서는 소켓을 생성하면서 바로 접속요청을 한다.  
만약 접속에 실패하면 connectedException이 발생한다.

```java
// 호스트와 포트번호를 지정하여 소켓을 생성한다
Socket socket = new Socket("example.com", 80);
```

## 서버에 요청

요청 메시지를 만들자 TCP로 전송한다.

먼저 start line 부터 생성한다.

```java
// 맨 마지막에 빈 줄이 있음을 유의하자(밑에서 따로 설명)
String message = """
 GET / HTTP/1.1
 Host: example.com

""";
```

위애서 생성한 소켓을 통해 `OutputStream`을 얻을 수 있다.  
자바에서 `writer`를 통해 문자열을 바로 전송 할 수 있다.

```java
OutputStreamWriter writer = new OutputStreamWriter(socket.getOutputStream());

writer.write(message);
writer.flush(); // writer는 내부적으로 버퍼가 있기 때문에 마지막에 flush를 꼭 해야한다.
```

## 응답받기

소켓을 통한 `InputStream`을 받아와서 쓸 수 있다.  
byte배열을 선언하고 배열에 데이터를 받아온다.  
실제라면 응답의 크기가 얼마일지 예상 할 수 없기 때문에 정해진 크기(Chunk)만큼 반복하여 읽어온다.

`reader`를 사용하면 훨씬 편하게 받아올 수 있다.

```java
// InputStream 받아와서 버퍼에 담기
InputStreamReader reader = new InputStreamReader(socket.getInputStream());
CharBuffer charBuffer = CharBuffer.allocate(1_000_000);

reader.read(charBuffer); // 버퍼 읽기
charBuffer.flip();   // 버퍼 내용을 뒤집는다.

System.out.println(charBuffer.toString());
```

## 소켓닫기

기본적으로는 통신이 종료되는 때에 수동으로 소켓을 닫아주는게 맞다.  
사실 그냥 둬도 나중에 GC가 소켓을 닫기는 하나, 명시적으로 닫아주자.

socket도 try-with-resources 문으로 자동으로 close 할 수 있다.

```java
try (Socket socket = new Socket(host, port)) {
   // Request
   // Response
}
```
