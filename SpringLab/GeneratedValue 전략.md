> ### 질문
> 각 Entity Id의 GeneratedValue 전략에 대해 자세하게 알고 싶습니다.
DB: MySql, Postgre, Oracle, h2, ...
Startegy: IDENTITY, SEQUENCE, TABLE, AUTO

## GeneratedValue 전략
JPA에서 ID값을 받아오기 위해 제공하는 어노테이션으로 `@GeneratedValue` 어노테이션이 존재합니다. Entity의 Id값을 null로 보내면 자동으로 Id값을 부여하는 어노테이션입니다. 이런 `@GeneratedValue`에는 여러가지 전략이 있습니다.

1. IDENTITY
2. SEQUENCE
3. TABLE
4. AUTO

### 1. IDENTITY
- 관련 DB: MySQL, PostgreSQL, SQL Server DB2 등

IDENTITY 전략은 id값을 null로 보내면 DB가 알아서 **AUTO_INCREMENT**를 시켜주는 전략입니다. IDENTITY 전략은 `entityManager.persist()` 시점에 즉시 INSERT 쿼리를 실행하고 id값을 DB로부터 받아옵니다.

#### 단점
따라서 Transaction 내부적으로 모아서 한 번에 INSERT를 하는 것이 불가능합니다. 하지만 하나의 Transaction 안에서 여러 INSERT 쿼리가 호출된다고 해서 비약적인 차이가 나진 않습니다.

### 2. SEQUENCE
- 관련 DB: Ex) MySQL, PostgreSQL, SQL Server DB2 등

데이터베이스의 **Sequence Object**를 사용하여 DB가 자동으로 숫자를 generate 해줍니다.
> Sequence Object란 유일한 값을 순서대로 생성하는 특별한 데이터베이스 오브젝트 입니다.

#### 단점
매번 Sequence Object라는 테이블에 가서 새로운 값을 받아와야 하기 때문에 네트워크 비용이 들 수 있습니다. 하지만 이를 위한 해결책으로 `allocationSize`라는 속성을 지원합니다. 이는 미리 `allocationSize`만큼의 PK를 받아오고, 메모리에서 꺼내 쓰는 방식을 의미합니다.

### 3. TABLE
TABLE 전략은 키 생성 전용 테이블을 하나 만들어서 위의 **2. SEQUENCE**전략을 흉내내는 방법입니다. 모든 DB에 적용할 수 있다는 장점이 있습니다.

#### 단점
하지만 **2. SEQUENCE** 전략처럼 최적화 된 테이블을 사용하거나 그런 방식을 쓰지 않기 때문에 성능상의 이슈가 있습니다.

### 4.AUTO
기본 설정 값으로서 방언에 따라 세 가지 전략 중 하나를 선택하여 사용합니다.