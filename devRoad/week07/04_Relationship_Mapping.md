# Relationship Mapping

데이터 모델의 Entity의 관계를 객체 참조로 간단하게 활용할 수 있도록 함

훌륭한 방법이지만 잘못쓰는 상황이 많다.

Join 과 Lazy fetch

일반적으로 aggregate를 구현하기 위해 cascade-type ALL 지정과
orphanRemoval=true를 지정하는게 강력히 권고

cascade-type ALL ⇒ 업데이트나 삭제같은거 전파
orphanRemoval=true ⇒ 안쓰면 삭제할거다.

일정 이상의 규모에서는 Command와 query를 구분??

Person이 Item을 소유하고 Item을 개별적으로 접근하는건 절대로 없도록
DDD에선 이런것을 Aggregate라는 개념으로 다룸

Aggregate를 잘 구성하면 적절한 개수의 DAO로 구성할 수 있다.

지금까지 사용한 과정에서의 불편함

1. persistence.xml에 계속 Entity 추가필요
2. EntityManager를 얻는 과정이 너무 불편
3. Transaction 관리 불편

위의 모든 것은 Spring이 해결해준다.

- N + 1 problem
- DDD의 Aggregate
- `CascaseType.All`
- `orpahanRemoval`
- Event Sourcing
- JPA 어노테이션
  - @OneToMany
  - @JoinColumn
