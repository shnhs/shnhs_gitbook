# 로그인 로그아웃

## CSRF(Cross Site Request Forgery)

CSRF 공격이란 웹 어플리케이션의 보안 취약점 중 하나이다.

사용자(희생자)가 자신의 의지와는 무관하게 공격자의 의도에 따른 행동(리소스 수정, 삭제, 등록)을  
특정 웹사이트에 하도록 하는 공격이다.

공격자는 특수한 패턴을 가진 악성 웹사이트로 사용자를 유도하여  
사용자의 인증정보를 가로채, 원하는 행동을 하게 된다.

CSRF를 예방하기 위해 대표적으로 Referrer 검증, Security Token 사용등의 방법을 사용한다.

## (Java) Date vs LocalDateTime

### Date 클래스

Date 클래스는 특정시점을 날짜 형태가 아닌 밀리초 단위로 표현하는 날짜 관련 클래스이다.  
게다가 기준 시점은 1900년을 기준으로 하고, 달(Month)를 표시하는 오프셋도 0부터 시작하는 등  
사용하기에 혼동이 있고 불편한 점이 여럿 있다.

Java 8.0 부터는 사용을 권장하지 않는다. 사실상 거의 사용도 하지 않는다.

### LocalDateTime

날짜와 시간을 모두 복합적으로 표현 가능한 클래스이다.  
같은 시기에 추가된 LocalDate와 LocalTime을 같이 가지는 클래스라고 볼 수도 있다.

LocalDateTime은 "2023-04-15T11:30:00" 와 같이 나노초 단위의 정밀도로 날짜와 시간을 표현할 수 있다.

## Epoch Time

프로그래밍에서 시각을 나타내는 방식 중 하나이다. UNIX 시간 혹은 POSIX 시간이라고 부르기도 한다.

Epoch Time의 정의는 아래와 같다.

> 1970년 1월 1일 00:00:00 UTC(세계협정시) 부터의 경과시간을 초로 환산한 정수 값.  
> 단 윤초는 무시한다.

유닉스 계열의 운영체제나 다른 운영체제, 혹은 파일 형식 등에서 사용하며,  
어떤 일의 발생 순서를 판단하는 지표가 될 수 있다.  
눈에 익숙한 날짜, 시간 형식이 아닌 숫자 형태로 되어있어 가독성은 떨어진다.

## Spring Security PasswordEncoder

사용자의 비밀번호를 입력받아, DB에 평문으로 저장할수는 없다.  
때문에 암호화 하여 저장하여야 하는데, Spring Securtiy에서는 비밀번호의 안전한 암호화를 위해  
`PasswordEncoder` 인터페이스와 그에 대한 구현체들을 제공한다.

Spring Security 5.3.3을 기준으로 아래와 같은 구현 클래스를 지원한다.

    BcryptPasswordEncoder : BCrypt 해시 함수를 사용해 비밀번호를 암호화
    Argon2PasswordEncoder : Argon2 해시 함수를 사용해 비밀번호를 암호화
    Pbkdf2PasswordEncoder : PBKDF2 해시 함수를 사용해 비밀번호를 암호화
    SCryptPasswordEncoder : SCrypt 해시 함수를 사용해 비밀번호를 암호화

위에 PasswordEncoder 중 Argon2를 사용하기를 권장한다.

Argon2는 2015년에 Password Hashing Competition에서 우승한 검증된 암호화 해시 함수이다.
Argon2는 옵션에 따라 salt, memory cost, time cost 등을 추가하여 더욱 안전한 암호문을 생성할 수 있다.
