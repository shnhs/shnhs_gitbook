# DTO란

## DTO란

Data Transfer Object. 뜻을 풀어서 쓰면 '데이터 전송 객체'이다.\
마틴 파울러의 정의에 따른다면

> An object that carries data between processes in order to reduce the number of method calls.

즉 서로 다른 프로세스 간에 데이터를 전달하기 위한 객체이고, 그 목적은 메서드의 호출 횟수를 줄이는것이다.

그러나 최근에는 단순히 데이터 전송의 역할을 하는 의마라면 폭 넓게 DTO라고 명명하기도 한다.

### Java record

Java 16부터 정식으로 지원하는 클래스 타입?이다.

DTO를 만들기 위해 toString, getter, setter등 여러 반복되는 코드를 직접 작성하거나,\
Lombok 같은 외부 라이브러리를 사용해야 했는데, 이를 자바에서 직접 지원하기 위해 고안된 문법이다.

레코드의 구조는 아래와 같다.

```java
public record Person(String name, int age) {}
```

class 대신 record 로 선언 할 수 있다. `name`, `age` 처럼 나열되는 필드를 컴포넌트 라고 부른다.\
해당 컴포넌트의 이름들을 참조하여 getter처럼 사용할 수 있다.\
body 부분에는 필요한 메서드를 추가로 작성 할 수도 있다.

record 타입을 사용하여, DTO처럼 쉽게 사용할 수도 있다.

## DAO는?

DTO와 세트로 많이 나오는 개념이 DAO다.\
DAO는 Data Access Object로 DB에 있는 데이터에 접근하기 위한 객체를 말한다.\
직접 DB에 접근하여 데이터dml CRUD를 조작한다.

Repository와 유사하다는 의견이 있다.\
이부분은 시간을 내 더 깊게 고민해 봐야겠다.\
[https://kkminseok.github.io/posts/Spring\_semina/](https://kkminseok.github.io/posts/Spring\_semina/)\
[https://velog.io/@power0080/java%EC%9E%90%EB%B0%94-record%EB%A5%BC-entity%EB%A1%9C#1-1-record-%EB%93%B1%EC%9E%A5-%EB%B0%B0%EA%B2%BD](https://velog.io/@power0080/java%EC%9E%90%EB%B0%94-record%EB%A5%BC-entity%EB%A1%9C#1-1-record-%EB%93%B1%EC%9E%A5-%EB%B0%B0%EA%B2%BD)



> 아래는 DTO와 관련된 개념들을 정리합니다.\
> 가지치기 처럼 개념이 나와 중구난방 일 수 있습니다.

## IPC

위에서 DTO를 프로세스 간 통신에 사용된다고 언급했다.\
프로세스 간 통신을 보통 IPC; Inter-Process communication 이라고 부른다.\
위키 백과 정의에 따르면 프로세스들 사이에서 서로 데이터를 주고받는 행위 또는 그에 대한 방법이나 경로를 말한다.\
아주아주 간단히 말해서 다른 프로그램이 서로 통신하는 것 이라고 생각하면 쉽다.

프로세스가 통신하는게 그렇게 특별한 일인가? 라고 생각할 수 있다.(물론 내가)\
프로세스 사이에 직접적인 통신이 가능한 방법은 없다. 직접 쉽게 서로 접근 가능하다면,\
프로세스의 코드 혹은 데이터가 외부 접근에 의해 쉽게 변경될 위험이 있기 때문이다.\
그러나 그럼에도 프로세스 간 데이터를 주고받는 것은 필요하기 때문에 IPC 기법이 나오게 된 것이다.

### IPC에서 사용가능한 기술

프로세스간 통신을 하기 위해 사용되는 몇가지 대표적인 기술이 있다.

1. File\
   대부분의 운영체제에서 사용가능 하고 가장 익숙한 방식이다.\
   다만 실시간으로 프로세스끼리 데이터를 주고 받는데는 어려움이 있다.\
   파일을 읽고, 쓰고, 변경하고, 하는 등 파일 처리에 여러 과정이 있기 때문이다.
2. Pipe\
   단방향 통신에 주로 사용된다.\
   부모 프로세스에서 자식 프로세스로 일방적으로 데이터를 보내는 경우 사용하는 기법이다.
3. Message Queue 메시지 큐 라는 메모리 공간을 활용하여 데이터를 주고받는 방식
4. Shared Memory 데이터를 주고 받는 것이 아니라 아예 메모리를 공유하는 방식
5. Socket 이전에 한번 정리했던 소켓이다.\
   원래는 네트워크 통신을 위한 기술로, 클라이언트-서버 사이 통신을 위한 방식이지만, 서로다른 프로세스에서도 적용 할 수 있다.

이 밖에도 Semaphore, Named Pipe 등 더 많은 방법이 존재한다.

## RPC

Remote Procedure Call. IPC의 한 종류로 RPC 라는것이 있다.\
위키피디아 정의에 따르면

> 원격 프로시저 호출(영어: remote procedure call, 리모트 프로시저 콜, RPC)은\
> 별도의 원격 제어를 위한 코딩 없이 다른 주소 공간에서 함수나 프로시저를 실행할 수 있게하는 프로세스 간 통신 기술이다.\
> 다시 말해, 원격 프로시저 호출을 이용하면 프로그래머는 함수가 실행 프로그램에 로컬 위치에 있든 원격 위치에 있든 동일한 코드를 이용할 수 있다.

원격에 있는 프로세스에 접근하여 프로시저 또는 함수를 호출하여 사용하는 것을 의미한다.

일반적으로 프로세스는 자기 주소공간의 함수만 호출하여 사용할 수 있으나, RPC를 통하면\
다른 주소공간에서 동작하고 있는 프로세스의 함수를 실핼 할 수 있다.

### 프로시저 vs 함수

개인적으로 위의 설명을 읽어보면서 프로시저 라는 단어가 이해가 잘 안되었다.

#### 프로시저란

일연의쿼리를 마치 하나의 함수처럼 실행하기 위한 쿼리의 집합, 일련의 작업을 정리한 절차\
리턴값이 없다.\
수행하는 절차 그 자체를 목적으로 한다.

#### 함수란

하나의 특별한 목적의 작업을 수행하기 위한 코드 집합.\
리턴값이 있다.\
어떠한 결과를 만들기 위함을 목적으로 한다.

즉, 함수가 어떤 작업을 위한 기능, 프로시저는 작업을 정리한 절차 를 의미한다.

## RMI

RMI; Remote Method Invocation이란 RPC를 위해서 자바에서 제공하는 기능이다.\
서로 다른 JVM간의 메소드 호출을 지원한다.\
내부적으로는 소켓통신을 하는 중이다.

## JavaBean란?

DTO에 관해 검색을 하면 같이 많이 나오는 개념이 바로 자바빈이다. 자바빈이란 데이터를 표현하는 것을 목적으로 하는 자바 클래스이다.\
컴포넌트와 비슷한 의미로도 사용된다. JavaBean 규격서에 따라 작성된 자바 클래스를 의미한다.

### 자바빈 규격서

여러가지 지켜야할? 규격이 있으나 대표적인 것 몇가지만 꼽아보면 아래와 같다.

* 직렬화 되어야 한다.
* 내부의 속성들은 setter, getter 혹은 다른 메서드로 접근 가능해야 한다.
* 기본 생성자를 가져야 한다.
* 필요한 이벤트 처리 메서드를 포함해야한다.

DTO는 제대로된 객체는 아니고 그냥 무기력한 데이터 덩어리 이다. C/C++ 등에서는 구조체로 구분할 수는 있으나, 자바는 못함 (최신 Java에서는 record를 활용가능, 오래된 Bean관련 라이브러리는 지원x)

> 참고자료\
> [https://velog.io/@redgem92/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-IPC-%EA%B8%B0%EB%B2%95](https://velog.io/@redgem92/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-IPC-%EA%B8%B0%EB%B2%95)\
> [https://velog.io/@moonheekim0118/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-IPC-%ED%86%B5%EC%8B%A0-%EB%B0%A9%EB%B2%95-Socket%EA%B3%BC-RPC](https://velog.io/@moonheekim0118/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-IPC-%ED%86%B5%EC%8B%A0-%EB%B0%A9%EB%B2%95-Socket%EA%B3%BC-RPC)\
> [https://fomaios.tistory.com/entry/Oracle-%ED%95%A8%EC%88%98Function%EC%99%80-%ED%94%84%EB%A1%9C%EC%8B%9C%EC%A0%80Procedure-%EC%B0%A8%EC%9D%B4](https://fomaios.tistory.com/entry/Oracle-%ED%95%A8%EC%88%98Function%EC%99%80-%ED%94%84%EB%A1%9C%EC%8B%9C%EC%A0%80Procedure-%EC%B0%A8%EC%9D%B4) >\
> [https://lifearchitect.tistory.com/14](https://lifearchitect.tistory.com/14)\
> [https://int-i.github.io/java/2022-09-12/java-record/](https://int-i.github.io/java/2022-09-12/java-record/)\
> [https://velog.io/@power0080/java%EC%9E%90%EB%B0%94-record%EB%A5%BC-entity%EB%A1%9C#1-1-record-%EB%93%B1%EC%9E%A5-%EB%B0%B0%EA%B2%BD](https://velog.io/@power0080/java%EC%9E%90%EB%B0%94-record%EB%A5%BC-entity%EB%A1%9C#1-1-record-%EB%93%B1%EC%9E%A5-%EB%B0%B0%EA%B2%BD)
