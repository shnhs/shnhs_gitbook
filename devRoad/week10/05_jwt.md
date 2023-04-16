# JWT

## JWT (Json Web Token)

JWT는 인터넷 표준 인증 방식 중 하나이다.  
인증에 필요한 정보들을 Token 형태로 담고, 이를 암호화 하여 사용하는 토큰이다.

다른 쿠키와 세션같은 인증과 구별되는 특징으로는 비밀 키로 서명된 토큰으로,  
좀 더 안전한 인증을 할 수 있다.

### 구조

JWT는 Header, Payload, Signature의 구조로 되어 있고,  
각 구성요소는 . 으로 구분된다(URL-safe 하도록 + / \_ 같은 부분 미포함)

> aaaaaaaa.bbbbbbb.ccccccccc  
> (Header).(Payload).(Signature)

Header에는 토큰의 타입이나, 어떤 암호화 알고리즘으로 서명을 생성했는지 표기된다.

Payload에는 토큰에 담을 Claim을 Key-Value 형태로 저장한다.  
클레임이란 토큰에 저장할 사용자의 인증 정보를 나타낸다. 토큰에 여러개의 클레임이 포함될 수 있다.

Signature에는 Secret Key를 포함한 서명이 암호화 되어 포함된다.  
Header, Payload를 대상으로 해싱한 후 이를 대상으로 비밀키로 서명하여 암호화 한다.  
-> 비밀키로 서명하고 공개키를 사용하여 검증한다.

### 특징

JWT는 이미 토큰으로, 자체가 인증되었기 때문에 별도의 인증 저장소가 필수로 필요하지 않다.  
세션과는 다르게, 클라이언트 상태를 서버에 저장하지 않아도 된다.  
비밀키 서명을 통해 더욱 보안성을 유지할 수 있다.

## @Secured

Spring Security에서 권한관리(인가)를 위해서 사용하는 어노테이션이다.

메서드 혹은 클래스단위에 어노테이션을 작성하고  
어노테이션에 명시된 권한이 있을 경우에만 접근이 가능하다.

```java
@Secured("ROLE_ADMIN") // Admin User만 접근가능
public String admin(){
 return "hello Admin!";
}

@Secured({"ROLE_ADMIN", "ROLE_USER"}) // User 권한까지 접근가능
public String user()){
 return "hello user!";
}
```
