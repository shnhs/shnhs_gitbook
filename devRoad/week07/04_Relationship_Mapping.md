# Relationship Mapping

## N + 1 Problem

연관 관계를 설정된 엔티티에 대해 DB에서 조회하고  
연관된 다른 엔티티에 접근하려 할 때 추가적인 쿼리가 발생하는 이슈이다.

처음 쿼리로 데이터를 가져올 때 하위 엔티티는 가져오지 않고,  
Lazy Loading으로 접근 할 때 실제 쿼리가 한번 더 실행된다.

데이터가 몇가지 없는 경우엔 큰 문제가 되지는 않지만  
만약 필요한 조회 결과가 많아지면 많아질수록 DB 접근이 기하급수적으로 늘어날 것이다.

N + 1 문제를 해결하기 위한 방법에는  
Fetch Join, 혹은 직접 JOIN을 사용한 쿼리 실행 등의 방법이 있다.

## JPA Cascade Types - CascadeType.ALL

상위 엔티티에서의 작업을 하위 엔티티로 모두 전파한다.  
예를들어 상위 엔티티를 삭제하면 연관된 하위 엔티티까지 모두 삭제한다.

## orphanRemoval

보통 1:N 관계를 가지는 엔티티 설정에 추가하는 옵션이다.  
상위 엔티티가 삭제되어서 하위 엔티티가 바라보는 연관관계가 사라졌을 경우  
해당 하위 엔티티를 자동으로 삭제해준다.

## JPA 어노테이션

### @OneToMany

단방향의 1:N 엔티티 관계를 설정하기 위해 사용하는 어노테이션이다.  
방향이라는 의미는 어떠한 엔티티가 다른 엔티티를 가질 수 있는가라는 의미이다.

예를 들어 '작가' 엔티티가 있고, '책' 엔티티가 있다고 하자,  
작가 한명은 여러 책을 가질 수 있으나, 한 책은 오직 하나의 작가와 관계된다.  
이때 (작가 -> 책)의 엔티티 방향을 가진다 고 설명할 수 있다.

이러한 단방향 1:N 매핑을 JPA 어노테이션으로 @OneToMany로 표현할 수 있다.

### @JoinColumn

위에서 설명한 1:N 관계 매핑에서 컬럼의 이름을 지정할 수 있는 어노테이션 이다.  
@JoinColumn은 1:N 관계중 N 쪽 엔티티에 지정한다.

몇가지 속성을 설정할 수 있다.  
먼저 name 속성에는 매핑할 외래 키의 컬럼명을 지정한다.  
referencedColumnName 속성은 외래 키가 참조할 대상 엔티티의 컬럼 이름을 지정한다.  
이때 referencedColumnName를 지정하지 않으면 대상 테이블의 PK로 자동으로 지정된다.

실제 코드를 예시로 보면 아래와 같다.

```java
@Entity
public class Author {

    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @OneToMany(cascade = CascadeType.ALL)
    @JoinColumn(name = "author_id")
    private List<Book> books;
}
```

Author 엔티티에서 @OneToMany로 Book 엔티티에 대한 1:N 관계를 표현하였고,  
JoinColumn으로 책 엔티티 쪽에 Author 테이블의 Id를 외래키로 잡으며,  
DB에서 그 컬럼 이름은 "author_id" 가 될 것이다.
