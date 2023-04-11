# Embeddable

## Aggregate

Aggregate란 JPA나 DDD에서 다루는 개념이다.  
어떤 모델(도메인)에서 가지는 연관 객체의 묶음을 의미한다.

## Embeddable이란

Aggregate를 간단하게 사용하기 위한 어노테이션이 있다.

어떤 도메인이 가지는 속성 중 한 객체로 관리할 속성을 묶어 클래스로 생성한다.  
해당 클래스에 `@Embeddable` 어노테이션을 사용하여 표기한다.

해당 Embeddable 클래스를 사용할 도메인에서는  
`@Embedded` 어노테이션을 표기하여 사용한다.

Embeddable 클래스는 식별자를 가지지 않는 Value Object로 생성하여 사용을 권장한다.

## Embeddable 예시

블로그에서 자주보이는 예시를 사용하여 한번 더 정리해본다.

아래와 같은 Memeber 엔티티를 가정해보자

```java
public Class Memeber{
    @Id
    @Column(name="member_id")
    private long id;

    private String name;

    private String city;
    private String town;
    private String zipCode;
}
```

Member 클래스가 가지는 속성 중 city, town, zipCode는 '주소'라는 개념을 묶을 수 있다.  
따라서 Address 라는 Embeddable 클래스를 생성할 수 있다.

```java
@Embeddable
public Class Address{
    private String city;
    private String town;
    private String zipCode;
}
```

그리고 원래의 Memeber 클래스에 해당 Embeddable 클래스를 적용할 수 있다.

```java
public Class Memeber{
    @Id
    @Column(name="member_id")
    private long id;

    private String name;

    @Embedded
    private Address address;
}
```
