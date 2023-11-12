# spring web MVC

## 스프링 MVC란?

MVC는 아키텍처 패턴에 속한다.

1. view  
   화면, GUI 표현에 관한 것

2. controller  
   입력에 대한것  
   뭔가 버튼을 누르면 거기에 대한 반응, 처리를 의미한다.

3. model
   그외 모든것  
   모델에서 핵심 비즈니스 로직를 구현한다

예를 들면

> 화면의 버튼(view)을 누를 때(controller) 불빛의 상태를 토글(model)한다.

## Controller

스프링을 사용하면 지금까지 구현했던 내용들을 아주 쉽게 구현할 수 있다.

```java
@RestController
public class WelcomeController {
    @GetMapping("/")
    public String home() {

        return "hello world";
    }

    @GetMapping("/hi")
    public String hi() {
        return "hi world";
    }
}

```

(사실 스프링버전이 나에게는 제일 익숙하다.)  
(어노테이션 관련된 부분만 나중에 시간나면 더 공부하자)
