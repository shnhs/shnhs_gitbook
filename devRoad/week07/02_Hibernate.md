# Hibernate

## Hibernate란

이전에 ORM 포스트에서 언급한 대로 대표적인 JPA 구현체이다.  
가장 일반적으로 사용한다.

내부적으로는 JDBC API를 사용한다.

### 장점

- 직접 SQL을 작성하는 것이 아닌 메서드 호출로 DB를 관리할 수 있다.
- 메서드와 객체로 DB를 관리하기 때문에 객체와 비즈니스 로직에 더욱 집중할 수 있다.

### 단점

- 직접 SQL을 사용하는 것보다는 성능상 약간 떨어짐
- (매핑이 잘못될 경우) 데이터 손실이 발생할 수 있다.

## Entity

거칠게 말해서 DB 테이블과 매칭되는 객체를 의미한다.  
Java 클래스에 `@Entity` 어노테이션으로 JPA에서 관리할 엔티티임을 명시한다.

## Entity Manager

이름에서 알 수 있듯이 Entity Manager는 Entity를 관리하는 객체이다.  
내부에 영속성 컨텍스트(Persistence Context)를 두어 엔티티 객체를 관리한다.

Entity Manager는 또 Entity ManagerFactory를 통해서 얻을 수 있다.

## 트랜잭션

DB등의 시스템에서 최소한의 작업 단위를 의미한다.  
다르게 말하면 한번에 처리해야하는 작업의 묶음 이라고 표현 할 수도 있다.

하나의 트랜잭션이 DB에 반영되는 것을 `Commit`.  
해당 트랜잭션에서 일어났던 모든 변화를 다시 이전 상태로 되돌리는 것을 `Rollback` 이라고 한다.

## JPQL

JPA에서 메서드를 사용하여 DB를 관리하는 기능을 제공하나  
모든 DB관련 작업을 하는데는 한계가 있다.

이런 한계를 채우기 위해 JPQL(Java Persistence Query Language)를 사용한다.  
메서드 호출로 다루기 힘든 자세한 쿼리를 작성하는데 사용한다.

일반 SQL쿼리와는 다르게 DB데이터를 대상으로 검색하는 것이 아닌  
객체를 기반으로 검색하는 쿼리 형태로 작성한다.

JPQL 구문을 예시로 들면 아래와 같다.

> String jpql = "select m from Member as m where m.name = 'gildong'";
