# http 서버 구성

## 1. Listen & Accept

요청을 받아들일 포트 번호만 정하면 된다.  
포트만 개방해놓으면 다른 클라이언트가 알아서 그 포트로 요청하러 온다.

자바에는 `ServerSocket`이라는 클래스가 별도로 있다.
`ServerSocket`은 요청을 듣기만 하는 클래스다.

```java
int port = 8080;
ServerSocket listener = new ServerSocket(port);

// 클라이언트 요청이 오면 요청 소켓을 리턴한다.
Socket socket = listener.accept();
```

위 의 코드 상태에서 클라이언트가 요청을 보내보면  
접속이 되는 듯 하나가 연결이 끊긴다.  
서버가 연결을 한번 받아주고 바로 소켓이 닫힌것이다.

> `listener.accpet()` 에서 프로그램은 멈추고 요청을 기다리게됨  
> IO 에서는 이렇게 기다리는걸 Blocking이라고 한다.

## 3. Request <-> Response

### Request

서버 입장에서는 Request를 받는 입장이므로 먼저 읽어 줘야함

```java
Reader reader = new InpuStreamReader(socket.getInputStream());
CharBuffer charBuffer = CharBuffer.allocate(1_000_000);

reader.read(charBuffer);
charBuffer.flip();

system.out.println(charBuffer.toString());

```

### Response & Close

요청을 받았다면 응답 메시지를 만들어서 보내주면 된다.

제대로된 헤더를 구성하려면 `content-type`이랑 `content-length`는 넣어주는게 좋다.  
`content-length`가 없는 경우에 Body 끝에 빈 줄이 없으면 문제가 될 수도 있다.  
길이를 명시해주던지, 빈 줄을 포함시키던지 둘중 하나는 해야한다.

보통의 경우에 시작라인의 두번째 항목(URL)을 참조하여  
다른 동작을 하도록(메서드 구현) 만드는 것이 보편적이다.

```java
String body = "Hello, world!";
byte[] bytes = body.getBytes();
String message = "" +
"HTTP/1.1 200 OK\n" +
"Content-Type: text/html; charset=UTF-8\n" +
"Content-Length: " + bytes.length + "\n" +
"\n" +
body;

Writer writer = new OutputStreamWriter(socket.getOutputStream());
writer.write(message);
writer.flush();

socket.close();
listener.close();
```
