# 기본 인증

## Identifier

앞서 많이 사용했던 개념으로 식별자 라는 뜻이다.  
식별자란 어떤 엔티티를 고유하게 식별할 때 사용하는 일련의 문자이다.

앞에 두글자만 따서 간단하게 ID라고 많이 부른다.  
특히 우리나라에서는 어딘가 회원가입 혹은 로그인에 ID와 Password라고 부르는 경우가 많은데,  
회원가입 혹은 로그인에 (우리나라가 지칭하는) ID는 영어로 표현할 때 UserName이라고 부르는 경우가 더욱 일반적이다.  
개발할 때 엔티티 식별자같은 경우에 ID라는 단어를 많이 사용하기 때문에 혼용하여 사용하지 않도록 주의해야 한다.

## OAuth 2.0

로그인 처리 혹은 사용자의 다른 플랫폼에 있는 리소스를 어떤 애플리케이션에서 사용할 수 있도록  
인증해 줄 수 있는 프로토콜 이다.

더 빠르게 이해할 수 있도록 예시를 들어 말하면,  
어떤 애플리케이션을 만든다고 할 때 사용자 로그인 처리를 사용자의 구글 아이디를 통해 하고 처리한다고 할 때,  
애플리케이션에서 사용자의 구글 아이디, 비밀번호를 직접 받아서 관리하는것이 아니라,  
사용자는 구글을 통해 로그인하여, 애플리케이션에게 토큰과 권한을 위임하고, 애플리케이션은 해당 토큰으로  
사용자 리소스에 접근하도록 하는 것이다.

## Token이란

인증/인가 분야에서 토큰이라고 하면 '지금 이 요청을 누가 보내고 있다' 라고  
인증 할 수 있도록 유니크한 식별값을 담은 암호화 객체 라고 정리할 수 있다.

주로 사용자가 로그인하면 토큰을 발행해 주고, 해당 사용자는 해당 토큰을 http 헤더에 담아 요청함으로써  
자신을 인증 할 수 있다.

토큰은 Http 헤더에 아래와 같은 형태로 담겨져서 요청보내진다.

```bash
Authorization: <type> <credentials>
```

## SecurityFilterChain

프로그램의 인증, 인가 처리를 간단하게 구현하기 위해 Spring에서는 Spring Security라는 프레임워크를 제공한다.

Spring Security의 `SecurityFilterChain` 모듈을 통해 애플리케이션에 들어오는 요청들에 대한  
보안 필터를 설정할 수 있다. 필요하다면 여러개의 보안 정책을 가진 필터들을 체인처럼 연결하여 구성할 수 있다.

```java
@Configuration // 스프링 컨텍스트로 관리되기 위한 어노테이션
@EnableWebSecurity // 스프링 보안 구성을 위한 어노테이션
public class WebSecurityConfig {
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http)
            throws Exception {

         // 체인에 인증 필터 추가

        http.authorizeHttpRequests().anyRequest().authenticated();

        return http.build();
    }
}
```

애플리케이션에서 요청이 들어오면 설정된 각 필터를 순서대로 통과하게 된다.  
각 필터에서 필요한 대로 응답을 받아와서 인증,인가 처리를 할 수 있고  
이상이 없다면 계속 진행하도록 허용하면 되고, 필요시 요청을 거부하고 에러를 반환할 수 있다.

## OncePerRequestFilter (내용 추가 및 더 이해 필요)

위에서 간략하게 정리한 것 처럼 Spring Security에서 필터란  
어플리케이션에 대해서 들어오고 나가는 Http 요청과 응답을 처리하는데 사용한다.

사용할 수 있는 필터에는 그냥 범용 `Filter` 인터페이스와 대표적으로 좀 더 사용하기 쉽게 Spring에서 제공하는  
`OncePerRequestFilter` 클래스가 있다.

둘의 가장 큰 차이점은 Filter는 단일 요청에 여러번 적용이 가능하지만,  
OncePerRequestFilter는 요청당 한 번만 필터가 적용되도록 할 수 있다.

코드적인 측면에서도 Filter 인터페이스를 직접 구현하여 사용하려면 인터페이스에 정의된 3개의 메서드를 구현해야 하지만,  
OncePerRequestFilter를 사용하면 doFilterInternal이라는 메서드 하나만 구현하면 된다.

```java
@Component
public class AccessTokenAuthenticationFilter extends OncePerRequestFilter {
   @Override
   protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain)
            throws ServletException, IOException {
      // TODO: 인증 처리
      filterChain.doFilter(request, response);
   }
}
```

## Authentication

사용자의 인증 정보를 저장하는 인증 객체?를 의미한다.

### Authentication 구조

Principal: 사용자 아이디 혹은 User 객체  
Credentials: 사용자 비밀번호  
authorities: 인증된 사용자의 권한 목록  
details: 인증 부과 정보  
Authenticated: 인증 여부

대표적인 구현체인 `UsernamePasswordAuthenticationToken` 을 통해 간단하게 Authentication 객체를 생성할 수 있다.

## SecurityContext

인증된 Authentication 객체가 저장되는 보관소이다.  
ThreadLocal에 저장되어 아무 곳에서나 참조가 가능하다.  
인증이 완료되면 HttpSession에 저장되어 어플리케이션 전반에 걸쳐 전역적인 참조가 가능하다.

```java
@Override
protected void doFilterInternal(HttpServletRequest request,
                                HttpServletResponse response,
                                FilterChain filterChain)
        throws ServletException, IOException {
    Authentication authentication =
            UsernamePasswordAuthenticationToken.authenticated(
                    "User", null, List.of());

    SecurityContextHolder.getContext()
            .setAuthentication(authentication);

    filterChain.doFilter(request, response);
}
```
