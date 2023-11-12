# Multipart/form-data

## 파일 다루기

FE 웹에서 파일을 넘겨줄 때 가장 보편적으로 사용하는 방식은\
request의 content-type을 multipart/form-data로 넘겨주는 것이다.\
이때 http method는 일반적으로 POST로 넘겨주는것이 일반적이다.

다른 메서드에서 동작을 안 하는것은 아닌듯 하나,\
파일을 넘겨준다는 것은 파일을 '제출했다' 라고 볼 수 있을 것 같은데\
그렇다면 HTTP 프로토콜 에서 POST를 써주는게 맞기도 할 것 같고,\
이미 거의 모든 다른 애플리케이션도 이를 따라서 설계되어서, multipart/form-data = POST로 생각하면 될 듯 하다.

## @ModelAttribute vs @RequestParam vs @RequestBody

사실 제목으로 쓴 3개가 사용처든 개념이든 구분이 뚜렷하긴 할 것이다.\
그러나 개인적으로 머리에서 잘 정리가 되지 않아 같이 묶어놓고 구분하여 정리하고자 한다.

### @RequestParam

http에서 넘어오는 파라미터 '1개'를 받을 때 사용한다. 1:1 매핑이다.\
만약 여러개 파라미터가 넘어오는데 나는 @RequestParam만 사용하겠다 라고 한다면\
파라미터 개수 만큼 컨트롤러에서 어노테이션으로 필드를 잡아주면 될 것이다.\
단 이 경우에 단점은, 기본적으로 @RequestParam은 선언하면\
http에서 해당 파라미터를 넘겨줘야만 하므로, 필드가 추가/삭제 될 때\
컨트롤러, 서비스의 비즈니스 로직 등에서 수정할 부분이 늘어나게 될 수 있다.

### @RequestBody

http의 요청 본문 값(보통 Json)을 자바 객체로 '변환'해주는 어노테이션이다.

> 변환 과정에서 뭐 ObjectMapper를 사용해서 기본생성자를 통해 객체를 만들고,\
> Setter는 없어도 Getter만 있으면 알아서 이름을 유추해서 변환 해준다 같은 내용은 일단\
> 간단하게만 알아만 두기로 한다.

주의할 점은 @RequestBody는 http Body(본문)을 대상으로 작동하는 어노테이션이기 때문에,\
GET방식과 같이 URI를 통해 파라미터가 넘어오는 경우는 @RequestParam으로 처리해줘야 한다.

### @ModelAttribute

http에서 넘어오는 파라미터'들'을 자바 객체에 '바인딩' 해준다.\
@RequestParam이 1개의 파라미터를 대상으로 한다면, @ModelAttribute는 여러개의 파라미터들을 대상으로 한다.\
특히 가장 큰 특징은 @ModelAttribute는 form형식'만' 으로 넘어오는 데이터를 처리 할 수 있다.

정리하자면(사실과 다를 수 있음)

* @RequestParma vs @ModelAttribute\
  @RequestParma은 1개의 파라미터를 받기위한 어노테이션\
  @ModelAttribute는 여러개의 파라미터를 받기위한 어노테이션
* @RequestBody vs @ModelAttribute\
  @RequestBody는 http body로 넘어오는 json형식 본문을 자바 객체로 변환해주는 어노테이션\
  @ModelAttribute는 http body로 넘어오는 form 형식의 본문은 자바 객체에 매핑해주는 어노테이션



### 강의 간략 후기

이전에 회사에서 @ModelAttribute냐 @RequestBody냐 가지고 꽤나 고생한 기억이 있는데,\
그때는 그냥 사실 '아 하나는 되고 얘는 뭐가 안되나보다, 되는걸로 해놓고 넘어가' 하고 지나갔는데\
이번 강의에서 키워드가 나온김에 좀 찾아보고 나름 머리에서 정리가 된 것 같다.

또 회사에서 지금 팀이 맡고있는 제품이 파일관련 처리는 많이 없다보니,\
BE에서 파일 다루는건 아직은 생소하고, 뭔가 복잡하고 어렵다는 느낌을 가지고 있었는데\
강의를 듣고나니 그런 막연한 생각을 좀 덜 수 있었던것 같다.
