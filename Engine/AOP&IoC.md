# 스프링 `AOP` (Aspect Oriented Programming)
```
관점 지향 프로그래밍이라고 불린다.

어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어서 보고 
그 관점을 기준으로 각각 모듈화하겠다는 것이다.

예로들어 핵심적인 관점은 결국 우리가 적용하고자 하는 핵심 비즈니스 로직이 된다. 또한 부가적인 관점은 핵심 로직을 실행하기 위해서 행해지는 데이터베이스 연결, 로깅, 파일 입출력 등을 예로 들 수 있다.
```
![image](https://user-images.githubusercontent.com/97263974/210187326-03464dcb-f366-4235-9940-347dd0fa7413.png)
```
위와 같이 관점으로 모듈화하고 핵심적인 비즈니스 로직에서 분리하여 재사용 하는 것이 AOP의 취지

==> 코드를 수정하지 않으면서 공통 기능의 구현을 추가하는 것
```

### 스프링 AOP 특징
- 스프링 빈에만 AOP 적용 가능
- 모든 AOP 기능을 제공하는 것이 아닌 스프링 IoC와 연동하여 엔터프라이즈 애플리케이션에서 가장 흔한 문제(중복코드, 프록시 클래스 작성의 번거로움, 객체들 간 관계 복잡도 증가 ...)에 대한 해결책을 지원하는 것이 목적)

### 스프링 AOP : @AOP
```xml
아래의 의존성을 추가해줘야 한다.
maven
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>

gradle
implementation 'org.springframework.boot:spring-boot-starter-aop'
```

# 스프링 `DI/IoC`란?
![image](https://user-images.githubusercontent.com/97263974/210188194-dc820007-7273-4d46-ae7f-dbf0885ed12f.png)
## `DI`
    의존관계 주입
    의존관계란 UML 모델에서는 두 클래스를 점선으로 된 화살표로 표현한다.
    존하고 있다는 건 무슨 의미일까? 의존한다는 건 의존대상, 여기서는 B가 변하면 그것이 A에 영향을 미친다는 뜻

의존관계 주입 전
```java
public class UserDao {
  DConnectionMaker connectionMaker = new DConnectionMaker();
}
```
UserDao는 이미 설계 시점에서 DConnectionMaker클래스의 존재를 알고 있다.

따라서 런타임의 의존관계 즉 , DConnectionMaker 오브젝트를 사용했다는 것 까지 UserDao가 결정하는 셈이다.

그래서 IoC 방식을 사용해서 UserDao로 부터 런타임 의존관계를 드러내는 코드를 제거하고, 제 3의 존재에 런타임 의존관계 결정 권한을 위임한다.


의존관계 주입 후 
```java
public class UserDao {

  private DConnectionMaker dConnectionMaker;

  public UserDao(DConnectionMaker dConnectionMaker) {
    this.dConnectionMaker = dConnectionMaker;
  }
}
```
#### DI 컨테이너는 UserDao를 만드는 시점에 생성자의 파라미터로 이미 만들어진 DConnectionMaker의 오브젝트를 전달합니다. 
#### 이렇게 해서 해서 두 개의 오브젝트 간에 런타임 의존관계가 만들어졌다.
#### UserDao 오브젝트는 이제 생성자를 통해 주입받은 DConnectionMaker오브젝트를 언제든 사용하면 된다.
#### 이렇게 DI 컨테이너에 의해 런타임시 의존 오브젝트를 사용할 수 있도록 그 레퍼런스를 전달받는 과정이 마치 생성자를 통해 DI 컨테이너가 UserDao에게 주입해주는 것과 같다고 해서 이를 의존관계 주입이라고 합니다.
--

--
## `IoC`(Inversion of Control)
    
IoC(inversion of Control) 적용 전
```java
public class UserDao {

  private DConnectionMaker dConnectionMaker;

  public UserDao(DConnectionMaker dConnectionMaker) {
    this.dConnectionMaker = dConnectionMaker;
  }
}
```

IoC(inversion of Control) 적용 후
```java
public class UserDao {

  private DConnectionMaker dConnectionMaker;

  public UserDao(DConnectionMaker dConnectionMaker) {
    this.dConnectionMaker = dConnectionMaker;
  }
}

@Configuration
public class DaoConfig {

  @Bean
  public DConnectionMaker dConnectionMaker() {
    return new DConnectionMaker();
  }

  @Bean
  public UserDao userDao() {
    return new UserDao(dConnectionMaker());
  }
}
```
#### @Configuration 이 붙은 DaoConfig 는 이 `ApplicationContext` 가 활용하는 IoC 설정 정보다.
####  `ApplicationContext` 는 DaoConfig 클래스를 설정정보로 등록해두고 @Bean 이 붙은 메소드의 이름을 가져와 빈 목록을 만들어둔다.

### 스프링 컨테이너
```
ApplicationContext 를 스프링 컨테이너라고 부른다.
기존에는 개발자가 직접 객체를 생성하고 DI 했지만, 이제부터는 스프링 컨테이너가 한다.
스프링 컨테이너는 @Configuration이 붙은 클래스를 설정 정보로 사용한다.

여기서 @Bean이라 적힌 메소드를 모두 호출해서 반환된 객체를 스프링 컨테이너에 등록한다.
이렇게 스프링 컨테이너에 등록된 객체를 스프링 빈이라고 한다.

이제는 스프링 컨테이너에서 객체를 스프링 빈으로 등록하고, 스프링 컨테이너에서 스프링 빈을 찾아서 사용할 수 있다.
```

## 결론
```
스프링이란 결국 어떻게 오브젝트가 설계되고, 만들어지고, 어떻게 관계를 맺고 사용되는지에 관심을 두는 프레임워크



스프링은 단지 원칙을 잘 따르는 설계를 적용하려고 할 때 필연적으로 등장하는 번거로운 작업을 편하게 할 수 있도록 도와주는 도구일 뿐임을 잊으면 안 됩니다.
```
