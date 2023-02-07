# URI & MIME type

## URI

약자를 풀어쓰면 Uniform Resource Identifier  
쉽게 말하면 리소스를 식별하기 위한 방법이다. 인터넷에 있는 자원을 나타내는 유일한 주소이다.

URI는 크게 두 종류로 나뉜다.

- URL(Uniform Resource Locator)  
  리소스의 위치를 표현한다. 우리가 자주 쓰는 그 주소창에 웹 주소 바로 그거다.  
  ”어떤 위치에 가면 어떤 리소스가 있다” 라는 개념으로 이해하자.  
  단 위치 변경에 취약하다. 실제로 주소에 한글자라도 잘못쓰거나 하면 위치를 찾을 수 없다.
- URN(Uniform Resource Name)
  리소스의 유니크한 이름, 사실상 거의 안쓴다.

> 자바에서 URI 클래스가 있다.  
> URL 과 URN을 둘 다 지원하기는 한다.

URN은 사실상 사용하지 않는 개념에 가깝기 때문에 보통 URI와 URL을 혼용하여 사용하곤 한다.  
그러나 둘이 완전히 동일하다고 할 수는 없는 듯 하다.

'https://www.naver.com/index.html?page=1232950&id=776' 이 와같은 주소가 있다고 하자.  
? 뒤의 식별자를 통해 식별자가 달라지면 다른 페이지로 이동하게 될 것이다.
엄밀하게 구분하면 주소 전체는 URI,  
식별자 를 제외한 'https://www.naver.com/index.html' 까지를 URL 이라고 할 수 있을 것이다.

## MIME type

Multipurpose Internet Mail Extensions의 약자.
리소스의 대한 표현 중 하나로 쉽게 말하면 content type 혹은 media type을 의미한다.

클라이언트 전송 응답뿐만 아니라 서버에게 요청 할 때도 같은 개념을 사용한다.  
MIME의 종류가 뭐가 있는지는 [IANA](https://www.iana.org/assignments/media-types/media-types.xhtml)에 등록된 목록을 참고하자

보통 "type/subtype" 형태로 나타낸다. ex) application/json

보통 많이 사용하는 타입만 정리하면 아래와 같다.
application으로 시작하면 응용할 수 있는 객체를 보통 나타낸다.
|type|설명|
| --- | --- |
| text/plain | 보통 이메일에서 자주 사용 |
| text/html | 일반적인 웹 html 문서 |
| text/css | css 파일 |
| text/javascript | javascript 파일 |
| application/xml | 범용, 자기서술적 이기 때문에 상대적으로 어렵고 복잡하다. |
| application/atom + xml | 요즘 트렌드는 아니라서 잘 사용하지는 않는다. |
| application/json | 범용, 자기서술적으로 표현하기 굉장히 어렵다. |
| application/dns + json | |
