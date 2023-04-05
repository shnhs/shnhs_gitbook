# Data Access

## DAO; Data Access Object

이전에 레이어드 아키텍쳐에 대해 정리하면서  
Business Logic에서 바로 DB에 접근하는것이 아니라  
Persistance Layer를 거쳐서 데이터를 다룬다고 하였다.  
Persistance Layer의 대표적인 예시로 DAO가 있다.  
JPA를 사용한다면 보통 Repository라는 개념을 많이 사용한다.

DAO는 DB에 접근하여 Business Logic에서 필요한 데이터를 CRUD 하는 객체를 의미한다.  
DB에서 데이터를 CRUD 하는 과정도 신경 쓸 부분이 꽤나 많기 때문에  
Business Logic과 분리하여 관심사의 분리를 좀 더 실현하고자 하는 이유가 아닌가 생각해본다.

_이름도 뭔가? 비슷한 탓인지 개념이 유사해서인지 DTO와 함께 많이 묶여서 설명되는 것 같다._  
_DAO는 DB에 접근하는 `객체` 라고 이해했고,_  
_어쨋든 DB Layer로 계층이 분리되기 때문에 Persistance Layer <-> DB Layer 혹은 다른 계층 이동에서_  
_계층간에 데이터를 전달하고 얻어올 때 사용하는 `포맷`이 DTO라고 일단 이해했다._

---

## List

데이터를 다루른 자료 구조 중 하나이다.  
배열과 유사하나 크게 다른점은 인덱스의 유무이다.

배열은 각 요소에 대한 인덱스가 정해져있어 인덱스를 통한 빠른 접근이 가능하다.  
그러나 리스트는 인덱스를 따로 사용하지 않으며, 중간 값이 제거되면 빈공간을 남기지 않고 채우게 된다.

Java에서 List의 종류로는 ArrayList와 LinkedList가 있다.  
둘은 사용할때는 동일하게 사용가능하나, 내부 구현 방법이 다르다.  
구현 방법의 차이에 따라 요소 삽입/삭제, 조회 기능에 따라 성능 차이가 약간 있으므로  
Business Logic에서의 기능에 따라 알맞은 형식을 사용하면 될 것 같다.

## Map

파이썬의 dictionary와 유사한 자료구조 이다.  
메모리에 Value만 차곡차곡 쌓이는 List와 다르게  
`Key-Value` 쌍으로 이루어지는 자료구조 이다.

Map을 생성하고 데이터를 추가할 때 Value의 중복은 허락하나,  
Key의 중복은 허락하지 않는다.