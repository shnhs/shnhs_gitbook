# Jackson 라이브러리

## Jackson 라이브러리란

요즘의 거의 모든 RESTful한 서비스는 데이터를 json형식으로 주고받는다.  
그러나 Java 자체에서는 Json <-> Json Object 변환을 지원하지 않는다.  
그래서 외부 라이브러리를 통해 변환을 자주 하게 되는데,  
그중 가장 유명한 라이브러리가 바로 Jackson이다.

Jackson은 JSON 처리를 위한 다목적 고성능 Java 라이브러리 이다.  
Java 객체를 JSON 표현으로 변환하는 데 사용할 수 있는 데이터 바인딩 기능을 제공합니다.  
JSON 문자열을 동등한 Java 객체로 변환하는 데 사용할 수도 있습니다.

## Jackson 라이브러리 사용

Jackson 라이브러리를 사용하면서 데이터 바인딩을 위해 지켜야할 제약이 몇가지 있다.

1. 클래스에 no-args의 기본 생성자가 있어야 한다.  
   매개변수가 없는 기본 생성자가 없다면, 에러가 발생한다.(JsonMappingException)

2. 개인 필드에 대해 getter가 필요하다.  
   단, 여기서 Jackson으로 DTO를 String으로 변환할 때 필드 값으로 따라가는 것이 아니라,  
   getter의 이름으로 명시한 이름을 따른다.
   > 필드 이름이 content이고, getter가 getContentText라고 하면  
   > toString에서 필드이름이 ContentText로 생성된다.  
   > 필드 이름을 강제하려면 @JsonProperty를 사용한다.

Spring DI를 통해 컨트롤러에서 Jackson ObjectMapper를 얻어서 사용할 수 있다.

> 스프링이 객체(Bean)을 관리하고 있고, 생성자를 명시하여 받아서 사용가능하다.  
> 사용 = 의존성 / 의존관계를 주입받는다. Dependency Injection  
> 얻어서 사용한다 → 그것에 의존하고 있다 → 의존성이 있다 → 의존관계가 있다.

```java
@RestController // @componant로 Spring이 알아서 처리해줌
public class PostController {
  private final ObjectMapper objectMapper; // 선언

  // 생성자 만들어줌
  public PostController(ObjectMapper objectMapper) {
  this.objectMapper = objectMapper;
}
```

뭔가 DTO 객체를 만들고 그 객체를 String으로 변환 ⇒ `writeValueAsString`

> 사실 근데 굳이 따로 objectMapper로 변환 안해도  
> SpringBoot 를 사용할때, DTO를 return타입으로 지정하면 알아서 json으로 리턴나간다.

Dto를 얻어 왔다면 `readValue`로 변환 할 수 있다.

---

> 참고자료
> <https://www.techiedelight.com/ko/serialization-java-objects-jackson-library/> > <https://mommoo.tistory.com/83>
