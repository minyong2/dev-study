# JPA  
```
@Entity
>>해당 클래스가 엔티티 클래스임을 나타낸다.
JPA에서는 기존 JDBC 방식처럼 쿼리를 사용하지 않고, 엔티티 클래스를 활용하여 데이터를 관리한다.
어플리케이션이 실행될 때, JPA는 이 클래스들을 엔티티 빈에 등록한다.

데이터베이스에 저장하기 위해 유저가 정의한 클래스가 필요한데 그런 클래스를 Entity라고 한다.
```
```
@GeneratedValue
>>PK의 생성 규칙을 나타낸다. 기본값으로는 데이터 추가시, 값이 자동으로 증가하는 정수형
```
```
@JsonProperty
>>JSON데이터와 자바 엔티티를 매핑시켜줄때 Key가 일치하지 않아 값을 제대로 받아올 수 없을때
사용한다.
```
```java
### @JsonProperty예시 ###

@Data
public class myInfo{
    @JsonProperty("my_name")
    private String myName;

    @JsonProperty("my_age")
    private String myAge;

    @JsonProperty("my_country")
    private String myCountry;
}
```
```
@JsonNaming
>> @JsonProperty를 사용할때 필드가 많아지게 되면 코드가 엄청나게 길어질 수 있다.
이 때 JsonNaming을 사용한다.
```
```java
### @JsonNaming 예시 ###

@Data
@JsonNaming(value = propertyNamingStrategy.SnakeCaseStrategy.class)
public class Student{
    private String myName;
    private String myAge;
    private String myCountry;
}

```


### @Id
    primary key를 가지는 변수를 선언하는 것을 뜻한다.
    @GeneratedValue은 해당 Id값을 어떻게 자동으로 생성할지 전략을 선택할 수 있다.
    @GeneratedValue(strategy = GenerationType.AUTO)
    -> 일반적으로 이렇게 사용. DB에 맞게 자동으로 생성해주는 역할이다.

### @Table
```
별도의 이름을 가진 데이터베이스 테이블과 매핑한다.
기본적으로 @Entity로 선언된 클래스의 이름은 실제 데이터베이스의 테이블 명과 일치하는 것을 매핑한다.
따라서 @Entity의 클래스명과 데이터베이스의 테이블명이 다를 경우 @Table(name="")과 같은 형식을 사용해서 매핑이 가능하다.
```

### @Column
```
@Column 선언이 꼭 필요한 것은 아니지만, @Column에서 지정한 변수명과 데이터베이스의 컬럼명을 서로 다르게 주고 싶다면 @Column(name="")과 같은 형식으로 작성하면 된다.
그렇지 않은 경우에는 기본적으로 멤버 변수명과 일치하는 DB 컬럼을 매칭한다.
컬럼에 언더바가 있을 경우에는 꼭 Column 어노테이션을 사용해야 한다.

해당 필드를 테이블의 컬럼과 매핑시켜준다.
생략하게 되면, 해당 필드명이 테이블의 컬럼명으로 자동 매핑된다.
컬럼의 규칙 등을 명시할 수 있다.
```
```
findBy~~ : 쿼리를 요청하는 메소드임을 알림.
```

## JpaRepository
```java
public interface MarketCodeRepository extends JpaRepository<MarketCode, String> {
}
```
- Spring Data JPA에서 제공하는 JpaRepository 인터페이스를 상속하기만 해도 되며, 인터페이스에 따로 @Repository등의 어노테이션을 추가할 필요가 없다.

이름|기능
---|---|
save()|레코드저장
findOne()|primary key로 레코드 한 건 찾기
findAll()|전체 레코드 불러오기,정렬(sort),페이징(pageable)가능
count()|레코드 갯수
delete()|레코드 삭제

```java
public interface MarketCodeRepository extends JpaRepository<MarketCode, String> {
	List<MarketCode> findBykoreanName(String koreanName);
}
```
!주의사항 변수명을 korean_name 처럼 언더바로 사용할시 findBy가 적용안됨!
- 변수명을 카멜표기법으로 맞추자~

### Query Method에 포함할 수 있는 키워드
메소드 이름 키워드|샘플|설명
---|---|---
And| findByEmailAndUserId(String email, String userId)|여러필드를 and로 검색
Or|findByEmailOrUserId(String email, String userId)|여러필드를 or로 검색
Between| findByCreatedAtBetween(Date fromDate, Date toDate)|필드의 두 값 사이에 있는 항목 검색
LessThan|findByAgeGraterThanEqual(int age)|작은 항목 검색
---
## lombok 어노테이션
```
@NoArgsConstructor
>>기본 생성자를 자동으로 생성한다.

@Getter @Setter
>>클래스 내에 있는 필드들에 대해서 getter,setter 메소드를 자동 생성

@Builder
>>해당 클래스의 빌더 패턴 클래스를 생성
클래스가 아닌 생성자에 명시했을 경우, 해당 생성자에 명시된 필드에 대해서만 빌더를 생성
```


## 코드예시
JPA를 사용하려면 @Entity 클래스를 먼저 생성해야 한다.
```java
@Entity
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@Table(name="coin.market_code")
public class MarketCode {

    @Id
    @Column(name = "market_code")
    private String marketCode;
    @Column(name = "korean_name")
    private String koreanName;
    @Column(name = "english_name")
    private String englishName;

    @Builder
    public MarketCode(String marketCode, String koreanName, String englishName) {
        this.marketCode = marketCode;
        this.koreanName = koreanName;
        this.englishName = englishName;
    }
}
```
---
```java
public interface MemberRepository extends JpaRepository<Member, Long> {

    Member findByName(String name);

    Page<Member> findByName(String name, Pageable pageable);
```
위와 같이 query 메소드를 추가하여 스프링에게 알릴 수 있다.
그러기 위해서는 규칙에 맞는 메소드를 작성해야 한다.
method|설명
---|---
findBy로시작|쿼리를 요청하는 메소드임을 알림
countBy로시작|쿼리 결과 레코드 수를 요청하는 메소드임을 알림

---

```java
 @Override
    public boolean isNew() {
        return false;
    }

    true로 할 시 무조건 새로 생성! (삭제실행안됨)
    새로 생성하는거라 테이블에 없는 데이터라고 생각함.
    키가 null이면 true.
```

```
복합키로 entity 생성했을 시 이름 findBy키이름_컬럼이름 으로 해줘야함
ex) List<AlarmMstEntity> findByAlarmMstKey_Alarmno(String alarmno);
```
### 참고링크
- [JPA사용법[JpaRepository]](https://jobc.tistory.com/120)



