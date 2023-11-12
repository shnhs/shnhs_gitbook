# CORS

## CORS란?

F/E에서 B/E REST API를 통해 통신하는 과정에서,  
꼭 한번 마주하는 문제가 바로 CORS이다.(~~물론 나도 그랬다.~~)

CORS란 Cross-Origin Resource Sharing으로 출처가 다른 자원들을 공유한다는 의미이다.

### 출처...?

먼저 출처(Origin)란 무엇인지 부터 알아야 할 것이다.

보통 우리가 알고있는 URL에서 Protocol + Host + Port 를 합쳐 Origin 즉 출처라고 한다.  
예를들어, `http://localhost:8080/item?id=1&page=1` 와 같은 형태의 URL이 있다면  
http:// (프로토콜 ) + localhost (host) + 8080 (port) 까지가 Origin이 된다.

## SOP

먼저 CORS이 발생하는 이유인 SOP에 대해 정리한다.  
SOP란 Same Origin Policy로 단어 그대로 동일 출처 정책 을 의미한다.  
풀어서 설명하면, "동일한 출처를 통해서만 리소스를 공유할 수 있다." 를 의미한다.

즉 동일한 출처를 통해서는 리소스를 자유롭게 가져올 수 있지만,  
다른 출처를 통해서는 리소스가 공유될 수 없다.

SOP 는 서버에서 처리되는 정책이 아니다.  
요청에 대해 서버는 정상적으로 처리하여 응답하였으나,  
웹 브라우저에서 처리해주는 보안 정책이다.

(사용자, 브라우저에서 보는중) ↔ (FE, localhost:3000) ↔ ( BE 서버, localhost:8080)
위와 같은 상황을 생각해보자  
사용자가 어떤 요청을 보내면 FE가 해당 요청을 BE로 보내 처리한다.  
이때 받아오는 응답 헤더의 내용에는
Host는 BE인 localhost:8080로 기록되고,  
Origin은 FE, localhost:3000으로 기록되어 있을 것이다.
브라우저 입장에서는 리소스의 출처와 요청에 대한 출처가 다르기 때문에 SOP에 위배된다고 판단하여,  
에러를 반환하게 된다.

### SOP가 필요한 이유는?

이런 보안 정책이 필요한 이유는 무엇일까?  
만약 SOP 없이 어플리케이션이 자유롭게 소통하게 된다면,  
나도 모르게 위험한 코드에 대한 요청을 자동으로 요청하고 응답 받을 수 있다.

따라서 다른 출처에서의 접근을 막기 위해 SOP 정책이 등장했다.

## 해결책으로서의 CORS

하지만 개발하는 상황에서 다른 어플리케이션과 통신하거나,  
다른 출처의 리소스를 필요로 하는 상황이 생기게 된다.

그렇다면 어떻게 SOP를 해결 할 수있는가 에 대한 해결 방안이 바로 CORS 이다.  
CORS 정책을 허용하는 리소스에 대해 다른 출처라도 응답을 받겠다는 의미이다.

백엔드 API 응답헤더에, ‘Access-Control-Allow-Origin’ 속성을 포함시킨다.  
서버쪽에서 어느어느 출처(FE Origin)로부터 요청한것이라면 괜찮다 를 알려주는 방식이다.

## Spring Web MVC CORS

우리가 자주 사용하는 Spring Web MVC에서 CORS를 적용하는 방법에 몇가지가 있다.

1. HttpServeletResponse 사용
   응답 헤더에 직접 'Access-Control-Allow-Origin' 속성을 포함시키는 방법이다.  
   다만 모든 응답 헤더에 코드를 직접 추가하는 것은 너무 비효율 적이다.

```java
HttpServeletResponse response
...
response.addHeader('Access-Control-Allow-Origin', 'localhost:3000')
// 와일드 카드 * 로 잡으면 모든 요청에 대해 CORS 허용
```

2. @CrossOrigin 어노테이션 사용  
   CORS을 설정할 대상에 어노테이션을 활용하는 방법이다.

```java
@CrossOrigin(originPatterns = "http://localhost:8080")
@RestController
public class LoginController {
    @GetMapping("/login")
    public String login() {
        return "로그인 성공! ID: jayon, PW: 1234";
    }
}
```

필요한 Controller 에 @CrossOrigin 어노테이션과 요청을 허용할 출처를 표시하여,
CORS 처리를 할 수 있다.

3. WebMvcConfigurer 설정
   WebMvcConfigurer 인터페이스에 대한 Spring Bean으로 환경 설정하는 방법도 있다.  
   @CrossOrigin 방식도 간단하긴 하지만, 여러군데에서 CORS 처리를 하려면 중복된 코드가 많아진다.
   글로벌 하게 config파일로 CORS 정책을 설정할 수 있다.

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("http://localhost:8080")
                .allowedMethods(HttpMethod.GET.name());
    }
}
```

addMappings을 통해 CORS를 허용할 URL path를 설정하고,
allowedOrigins을 통해 메서드를 허용할 출처를 설정한다.
추가로 allowedMethods를 통해 CORS를 허용할 메서드를 제한 할 수 있다.

---

> 참고자료  
> <https://escapefromcoding.tistory.com/724>  
> <https://inpa.tistory.com/entry/WEB-📚-CORS-💯-정리-해결-방법-👏>  
> <https://steady-coding.tistory.com/616>
