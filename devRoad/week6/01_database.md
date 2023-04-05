# DataBase

## DB

데이터의 모음  
그냥 막 모아놓은것이 아니라, 어떤 목적을 위해 정리정돈 하여 저장한 데이터

## DBMS

데이터 베이스 관리 시스템

DB에 접근하여 DB의 데이터를 CRUD할 수 있는 프로그램  
CRUD와 더불어 보안 및 권한 관리도 제공한다.

> 보통 개발 과정중에 DB라고 지칭하는 것은 대부분 DBMS를 말한다

DBMS의 유형으로는 계층형, 망형, 관계형, 객체지향형 등등 여러 종류가 있으나  
현재 가장 보편적으로 사용하는 DBMS는 `관계형(RDBMS)`이다.

## RDBMS

관계형 데이터 베이스 관리 시스템  
주로 행과 열로 이루어진 테이블의 형태로 데이터를 저장하는 방식이다.

우리가 주로 사용하는 MySql, MariaDb, PostgreSQL 등이 RDBMS에 해당된다.

## SQL

Structured Query Language

관계형 데이터 베이스에서 사용되는 언어이다.  
주로 데이터베이스를 조작하는 데 사용된다.

SQL은 국제 표준화기구에서 관리되는 표준 언어 규칙이 존재한다.  
그러나 모든 DBMS 가 표준 SQL을 만족하는 것은 아니고,  
최대한 표준을 지키고 각 DB의 특성을 반영한 SQL을 사용한다.

SQL문장들의 종류를 크게 아래의 4가지로 구분 할 수 있다.

### DDL

Data Definition Language. 데이터 정의어

DB 혹은 테이블 생성 및 테이블 구조 변경 및 삭제, 이름 변경 등  
DB 스키마(뼈대 혹은 구조?)를 조작하거나 정의하기 위한 언어이다.

### DML

Data Manipulation Language. 데이터 조작어

DB의 데이터를 직접 조작하기 위한 언어이다.  
실제 데이터의 CRUD를 하는 언어라고 생각하면 될 것 같다.

### DCL

Data Control Language. 데이터 제어어

DB에 대한 접근 권한 및 보안 관련 언어이다.  
GRANT(권한 부여), REVOKE(권환회수) 가 대표적이다.

### TCL

Transaction Control Language. 트랜잭션 제어어

DB에서 어떤 명령어(쿼리)를 실행할지 말지 여부를 결정하는 언어이다.  
COMMIT으로 쿼리를 실행, ROLLBACK으로 실행 취소가 대표적이다.

> TCL을 DCL과 같은 범주로 묶기도 한다.

## Data Model

데이터 모델이란 현실에서의 무언가를 데이터로 표현하기 위해  
데이터의 속성 혹은 데이터사이의 관계등을 단순화, 추상화 한 개념을 의미한다.

> 어떤 대상에 대한 DB를 구성할 때 어떤 구조(테이블?), 속성(컬럼?)으로  
> 구성할지에 대한 개념으로 일단 일단 이해하고 넘어가자

데이터 모델은 어떤 데이터를 저장할 DB를 이해하고, SQL을 효율적으로 구성하는데 도움을 준다.

데이터 모델을 구성하기 위해 일반적으로 3단계로 진행한다.

1. Conceptual Data Model - 개념적 구분  
   데이터의 전반적인 구조와 콘텐츠를 나타낸다.
2. Logical Data Model -논리적 구분  
   DB로 구성하고자 하는 대상에 대한 구체적 속성과 관계를 구성한다.
3. Physical Data Model - 물리적 구분  
   실제 DB가 위치할 DBMS 혹은 하드웨어적 관점을 구성한다.

보통 데이터 모델의 종류 등을 얘기할 때 ‘논리적 구분’ 단계를 더 세분화 하여 다룬다.

논리적 구분의 종류에는 대표적으로 계층형, 네트워크형, 관계형, 객체지향형 등이 있으나  
역시 현재 가장 보편적으로 인기있게 사용하는 모델은 관계형 데이터 모델이다.