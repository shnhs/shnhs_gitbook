# JDBC

## JDBC란?

Java Database Connectivity  
Java에서 RDBMS를 사용할 수 있도록 도와주는 자바 표준 API이다.  
관계형 DB에 접근하여 SQL 쿼리를 실행하기 위한 라이브러리를 제공해준다.  
사용하고자 하는 RDBMS의 드라이버를 추가로 설치하여 사용하면 된다.

## 사용법

JDBC를 사용하여 DBMS에 접근하는 단계는 아래와 같다.

1. 드라이버 설정
2. Connection 객체 생성
3. 쿼리문 실행 객체 생성 및 쿼리 실행
4. 쿼리 실행 결과 값 사용
5. 연결 객체 종료

### 1. 드라이버 설정

사용하고자 하는 DBMS종류와 버전에 맞게 드라이버를 설정해준다.  
Gradle을 사용한다면, build.gradle 파일에 의존성을 추가해주면 된다.

```text
implementation 'org.postgresql:postgresql:42.5.4'
```

### 2. Connection 객체 생성

데이터 베이스에 접근하기 위해 Connection 객체를 생성하여 사용한다.  
DriverManager 클래스의 getConnection() 메서드를 사용하여 얻을 수 있다.

getConnection()에 할당 할 수 있는 매개변수는 아래와 같다.

```java
Connection connection1 = DriverManager.getConnection(String url)
Connection connection2 = DriverManager.getConnection(String url, String user, String password)
Connection connection3 = DriverManager.getConnection(String url, Properties info)
```

url의 표현 형식은 'jdbc:[dbms이름]://{ip}:{port}/{db식별자}'의 형태로 구성된다.  
Properties는 사용자 계정과 비밀번호를 포함하는 객체이다.

### 3. 쿼리 실행 객체 생성 및 쿼리 실행

Connection 객체로 DB에 접근 한 후 쿼리 실행 객체로 원하는 쿼리를 실행한다.  
쿼리 실행 객체에는 `Statement`, `PreparedStatement` 등이 있다.  
Statement는 정적 쿼리에, PreparedStatement는 동적 쿼리에 사용한다.

`connection.createStatement()` 메서드로 steatement 객체를 생성 할 수 있다.  
생성된 Statement 객체에 `executeQuery(String sql)` 혹은  
`executeUpdate(String sql)` 메서드를 통해서 원하는 쿼리문을 실행한다.

`executeQuery` 메서드는 `ResultSet`이라는 타입의 객체를 리턴한다.  
`executeQuery` 메서드는 SELECT문을 실행 할 때 사용되며,  
리턴받은 ResultSet 객체를 통해 읽어온 결과를 사용할 수 있다.

`executeUpdate` 메서드는 INSERT, UPDATE, DELETE 쿼리문을 통해 데이터 CUD를 할 수 있다.

`PreparedStatement`로 사용할 수 있는 동적 쿼리는 쿼리 문자열에 ? 를 사용할 수 있다.  
? 에는 해당 위치에 들어갈 속성 혹은 릴레이션 등 원하는 값을 설정 할 수 있다.  
`connection.preparedStatement(String sql)` 메서드로 preparedStatement 객체를 생성 할 수 있다.  
생성된 객체에는 set###(index, value) 와 같은 형태로 ? 에 어떤 값을 설정할 것인지 지정 할 수 있다.

### 4. 쿼리 실행 값 사용

SELECT 쿼리 등으로 조회된 데이터를 받아 오는 경우 앞서 말한 것처럼  
`ResultSet` 타입의 객체로 담겨서 리턴받는다.

해당 객체에서 get###()메서드를 통해서 원하는 변수 타입으로 결과를 받아올 수 있다.  
get###() 메서든 매개변수로 컬럼의 인덱스 혹은 컬럼 이름을 지정하여 원하는 값을 명시 할 수도 있다.

### 5. 연결 객체 종료

더 이상 쿼리 실행에 사용하지 않을 connection 객체는 닫아주는 편이 좋다.  
try-catch-finally 등을 사용하여 자동으로 닫아지게 하거나,  
`connection.close()` 메서드를 통해 connection 객체의 DB 연결을 종료한다.
