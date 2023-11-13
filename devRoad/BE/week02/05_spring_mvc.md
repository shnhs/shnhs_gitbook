# Spring Web MVC 구현

## 다시 한번 MVC

이전에 MVC가 무엇인지에 대한 정리를 한번 했었지만  
한번 더 정리해보도록 한다.

MVC는 Model, View, Controller의 약자이다.  
View 사용자가 보는 시각적인 화면,  
Model은 어떤 데이터를 처리하고자 하는 형태,  
Controller는 View와 Model의 상호 이벤트나 프로그램 동작을 하는 비즈니스 로직을 말한다.

## Spring MVC REST API Annotation

Spring MVC에서 자주 사용하는 어노테이션에 대한 정리이다.

### @RestController

Spring 프레임워크 4.x 버전 이상부터 사용가능한 어노테이션으로  
@Controller와 @ResponseBody와 결합된 어노테이션이다.

@Controller 와 @RestController의 주요한 차이점은 Http Response Body가 생성되는 방식이다.  
@Controller는 Model 객체를 만들어 데이터를 담아 View를 반환하는것이고,  
@RestController는 단순 객체를 반환하고, json과 같은 형식의 데이터를 Http Response Body에 담아 응답한다.

@RestController 어노테이션을 적용하면 @Controller에 일일히 @ResponseBody 어노테이션을 적용할 필요가 없다.

### @RequestBody, @ResponseBody

클라이언트와 서버 사이에 데이터를 주고받을 때 json형식과 자바 객체 형태로의 변환이 필요하다.  
클라이언트 요청의 경우 json -> 자바 객체로의 변환  
서버의 응답의 경우 자바 객체 -> json으로의 변환 과정을 거친다.  
이런 변환과정을 위의 어노테이션이 쉽게 처리해준다.

@RequstBody 어노테이션은 HttpRequest의 본문 requestBody 내용을 자바 객체로 매핑한다.  
DispatcherServlet에서 먼저 해당 HttpRequest의 미디어 타입을 확인하고,  
타입에 맞는 MessageConverter를 통해 요청 본문인 requestBody를 통째로 변환해서 메서드로 전달해준다.

> 주의할 점은 GET 방식 요청의 경우 Body로 요청데이터가 전달되는것이 아니라  
> URI 파라미터를 통해 전달되므로 @RequestBody로 요청 내용을 받을 수 없다.  
> GET 요청의 경우 @PathVariable 혹은 @RequestParam으로 요청을 전달 받는다.

### @RequestMapping

보통 REST controller를 구성할 때 한 리소스에 대한 단위로 묶어 Controller를 작성하게 된다.
예를 들어 게시물에 대한 controller를 구성한다고 생각해보자

GET /posts  
GET /posts/{id}  
POST /posts  
...

앞서 정리한 컬렉션 패턴을 기반으로 post라는 리소스를 중심으로 구성하게된다.  
그러나 위의 작성한 API를 보면 /post 라는 URI는 계속 중복되게 된다.  
공통적인 URI의 경우는 class에 @RequestMapping으로 설정하고,  
그 아래 @GetMapping, @PostMapping, @PutMapping, @DeleteMapping에서는 생략하여 간단하게 URI를 설정할 수 있다.

### @Pathvariable

위에서 한번 언급한 어노테이션이다. 주로 URI값에 가변형 변수를 전달하는 방식에 사용한다.  
<http://127.0.0.1/users?userId={$userId>}  
<http://127.0.0.1/users/{userId>}  
위와 같이 사용할 수 있다. userId를 받아서 처리를 할 수 있다.

### @ExceptionsHandler

controller 계층에서 발생하는 에러를 잡아서 메서드로 처리하는 어노테이션이다.  
추후에 배울것 같지만 Service, Repository에서 발생하는 에러는 제외한다.

@ExceptionHandler의 value값으로 어떤 Exception을 처리할 것인지 넘겨줄 수 있다.  
value를 설정하지 않으면 모든 Exception을 잡아 처리하기 때문에 구체적인 Exception을 넘겨주는것이 좋다.

## API 설계

보통 @RequestMapping으로 어떤 리소스에 대한 Controller인지 컬렉션을 묶고,  
 @GetMapping : 리소스 목록 조회  
 @GetMapping : 리소스 상세 조회  
 @PostMapping : 새로운 리소스 생성  
 @PatchMapping : 리소스 상태 업데이트  
 @DeleteMapping : 리소스 삭제  
 @ExceptionHandler : 에러처리  
정도로 컨트롤러가 너무 커지지 않도록 설계 하는것이 좋다.

> 참고자료  
> <https://blog.neonkid.xyz/220>  
> <https://doctorson0309.tistory.com/664>  
> <https://dev-coco.tistory.com/84>  
> <https://wildeveloperetrain.tistory.com/144>  
> <https://mungto.tistory.com/436>  
> <https://www.appletong.com/entry/Spring-PathVariable>
