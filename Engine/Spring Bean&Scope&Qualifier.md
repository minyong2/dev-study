# java 

---
## Lombok
### @Scope
    => Spring Scope를 이해하기전에 Spring Bean부터 알아보자.

## Spring Bean

![spring-bean](https://user-images.githubusercontent.com/97263974/183579410-7229900d-7484-40d3-8dc8-bee72e46c11a.png)

    Spring에서 POJO(plain, old java object)를 'Beans'라고 부른다.
    Beans는 애플리케이션의 핵심을 이루는 객체,
    Spring IoC 컨테이너에 의해 인스턴스화, 관리, 생성된다.
    Beans는 우리가 컨테이너에 공급하는 설정 메타 데이터(XML)에 의해 생성된다.
        - 컨테이너는 이 메타 데이터를 통해 Bean의 생성, Bean Life Cycle, Bean Dependency(종속성) 등을 알 수 있다.
    애플리케이션의 객체가 지정되면, 해당 객체는 getBean() 메서드를 통해 가져올 수 있다.

### Spring Bean 정의
- 일반적으로 XML 파일에 정의한다.
- 주요속성 : 
    - class(필수) : 정규화된 자바 클래스 이름
    - id : bean의 고유 식별자
    - scope : 객체의 범위(singlton, prototype)
    - constructor-arg: 생성 시 생성자에 전달할 인수
    - property: 생성 시 bean setter에 전달할 인수
    - init method와 destroy method
```xml
<!-- A simple bean definition -->
<bean id="..." class="..."></bean>


<!-- A bean definition with scope-->
<bean id="..." class="..." scope="singleton"></bean>


<!-- A bean definition with property -->
<bean id="..." class="...">
	<property name="message" value="Hello World!"/>
</bean>

https://gmlwjd9405.github.io/2018/11/10/spring-beans.html
```
--- 
## 이제 @Scope를 알아볼까요?
### Spring Bean Scope
```
- 스프링은 기본적으로는 모든 bean을 singlton으로 생성하여 관리함.
    구체적으로는 애플리케이션 구동 시 JVM안에 스프링이 bean마다 하나의 객체를 생성하는 것을 의미
    그래서 우리는 스프링을 통해 bean을 제공받으면 언제나 주입받은 bean은 동일한 객체라는 가정하에서 개발을 함.
    request, session, global session의 Scope는 일반 Spring 어플리케이션이 아닌, Spring MVC Web Application에서만 사용된다.

```
### Singlton
```
클래스의 인스턴스가 딱 1개만 생성되는 것을 보장하는 디자인 패턴이다.

그래서 객체 인스턴스를 2개 이상 생성하지 못하도록 막아야 한다.

코드에서 private 생성자를 사용해서 외부에서 임의로 new 키워드를 사용하지 못하도록 막아야 한다.

하지만 스프링 컨테이너를 사용하면 컨테이너에 등록되는 빈들을 알아서 싱글톤으로 관리해준다.
```
```
싱글톤 패턴에서 주의점
하나의 인스턴스를 생성해서 공유하는 형식이므로 객체의 상태를 유지하게(stateful) 설계하면 안된다.

특정 클라이언트에 의존적이거나 값을 변경할 수 있는 필드가 있으면 안된다.

가급적 읽기만 가능해야 한다.

필드 대신 공유되지 않는 지역변수,파라미터,ThreadLocal을 쓰자.(예를 들어 필드를 반환하는게 아닌 파라미터를 반환하자)
```

### @Scope
```
Scope의 뜻대로 빈이 존재할 수 있는 범위를 말한다. 스프링 빈은 기본적으로 싱글톤 스코프가 적용된다.

스프링은 다양한 종류의 스코프를 지원하는데 각 스코프를 분석해보자.



1.싱글톤: 기본 스코프, 스프링 컨테이너의 시작과 종료까지 유지되는 가장 넓은 범위의 스코프

2.프로토타입: 스프링 컨테이너는 프로토타입 빈의 생성과 의존관계 주입까지만 관여하고 더는 관리하지 않는 매우 짧은 범위의 스코프. @Scope("prototype")로 선언한다.

(의존관계 까지만 주입하고 보내주기 때문에 @PreDestroy를 볼 수 없음)
```
![spring-bean-scope](https://user-images.githubusercontent.com/97263974/183580838-63116f45-13b4-47a2-9d9e-2897d46b7903.png)


---
### @Qualifier
```
스프링이 빈을 주입할 때 bean의 객체가 두개 이상이라면??
=>스프링이 어떤 빈을 주입해야할지 몰라서 Exception을 발생시킴.

@Autowired의 주입 대상이 한 개여야 하는데 실제로 두 개 이상의 빈이 존재하게 되면 주입할 때 객체를 선택할 수 없게 됨.

단, @Autowired가 적용된 필드나 설정 메소드의 property 이름과 같은 이름을 가진 bean 객체가 존재할 경우 같은 이름의 bean 객체를 주입받는다.
```
- 사용할 의존 객체를 선택할 수 있도록 해준다.
    - 설정에서 bean의 한정자 값을 설정
    - @Autowired 어노테이션이 적용된 주입 대상에 @Qualifier 어노테이션을 설정한다.
        - 이때 @Qualifier의 값으로 앞서 설정한 한정자를 사용

```java
@Service("MinyoungService")
@Scope(value = "prototype")
public class MinyoungServiceImpl implements MinyoungService{
	private final static Logger LOGGER = Logger.getLogger(MinyoungServiceImpl.class);

    @Autowired
    private ClusterPrdAmqp clusterPrdAmqp;

    @Autowired
    @Qualifier("PotnMinyoungService")
	private PotnMinyoungServiceImpl potMinyoungService;

    @Autowired
    @Qualifier("RoadmMinyoungService")
	private RoadmMinyoungServiceImpl roadMinyoungService;
```
        ↪ @Qualifier로 각 생성자가 bean객체를 주입받음... 맞나?
        아무튼 더 많은 코드를 보고 이해해보겠읍니다 .


---
---
---
---
---
---
수정 

220809

