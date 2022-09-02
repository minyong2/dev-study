# Annotation
---
### @Scheduled
    주기적인 작업이 있을 때 사용하면 쉽게 적용할 수 있다.
속성
- cron : "초 분 시 일 월 주 (년)"으로 표현한다. 특수문자도 사용 가넝
- fixedDelay : milliseconds단위, 이전 작업이 끝난 시점으로 부터 고정된 시간을 설정한다. ex) fixedDelay = "5000"
- fixedDelayString : ficedDelay와 같은데 property의 value만 문자열로 넣는 것이다.
- fixedRate : milliseconds 단위, 이전 작업이 수행되기 시작한 시점으로 부터 고정된 시간을 설정한다. ex) ficedRate = 3000
- fixedRateString : fixedDelay와 같은데 property의 value만 문자열로 넣는 것이다. ex) fixedRate = "3000"
- initialDelay : 스케줄러에서 메소드가 등록되자마자 수행하는 것이 아닌 초기 지연시간을 설정하는 것이다.
- initialDelayString : 위와 마찬가지로 문자열로 값을 표현하겠다는 의미다.
- zone : cron 표현식을 사용했을 때 사용할 time zone으로 따로 설정하지 않으면 기본적으로 서버의 time zone이다.

![image](https://user-images.githubusercontent.com/97263974/187860253-9e7848db-63ca-410d-85be-16a6f952d65f.png)

- Rate는 작업 수행시간과 상관없이 일정 주기마다 메서드 호출을 시켜주는 것이고,
Delay는 (작업 수행 시간을 포함하여) 작업을 마친 후부터 주기 타이머가 돌아 메서드를 호출해주는 것이다.


## AOP 관련

### @Aspect
    @Aspect의 경우 해당 Class가 횡단관심사(부가기능) Class임을 알려주는 어노테이션
    @Aspect를 했다고 자동으로 Bean에 등록되는것은 아니다. 따로 등록을 해주는 작업 필요

### @Around("PointCut")
    @Around는 `Advice`의 한 종류로 `핵심관심사`의 실패여부와 상관없이 전 후로 실행되도록 하는 `Advice`이다.
    속성값으로 `PointCut`을 전달해줘야한다.
```java
@Aspect
public class LogAop{
    @Around("execution(* com.java.ex.Car.accelerate(..))")
    public Object logging(ProceedingJoinPoint joinPoint) throws Throwable{
        String methodName = joinPoint.getSignature().toShortString();
        try{
            System.out.println(methodName + "is Start.");
            Object obj = joinPoint.proceed();
            return obj;
        }finally{
            System.out.println(methodName + "is finish.");
        }
    }
}
```

### @Pointcut
    @Pointcut 횡단관심사(부가기능)이 적용될 JoinPoint들을 정의한 것이다.
    복잡한 표현식을 가지고 있다.

#### - execution(접근제어자,반환형 패키지를 포함한 클래스 경로, 메소드파라미터)
```java
@Pointcut("execution(public void get*())")
```
- 퍼블릭형의 반환형이 없는(public void)형태의 메소드 중 get으로 시작하는(get*)모든 메소드 중 파라미터가 존재하지 않는 (`()`)메소드들에게 적용

```java
@Pointcut("execution(* *(..))")
```
- 첫번째 `*` 기호는 접근제어자와 반환형 모두를 상관하지 않고 적용하겠다는 의미.
- 두번째 `*` 기호는 어떠한 경로에 존재하는 클래스도 상관하지 않고 적용하겠다는 의미.
- 마지막 `(..)`은 메소드의 파라미터가 몇개가 존재하던지 상관없이 실행하는 경우
```java
@Pointcut("execution(* com.java.ex.Car.accekerate())")
```
- 첫번째 `*`기호는 역시 `접근제어자, 반환형`을 상관하지 않는다는 의미
- `com.java.ex.Car.accekerate()`는 해당 Class의 `accelerate()`(파라미터가 없는) 메소드가 호출될 때 실행하는 경우를 의미한다.
```java
@Pointcut("execution(* com.java..*.*())")
```
- `..`의 경우 해당 패키지를 포함한 모든 하위패키지에 적용한다는 의미.
- 해석한다면 `접근제어자,반환형`을 상관하지 않고 `com.java`패키지를 포함한 모든 하위디렉토리의 모든 Class의 모든 파라미터가 존재하지 않는 메소드에 적용한다는 의미


#### - within(`Class의경로`)
* 패키지 내의 모든 메소드에 적용할 때 사용
```java
@Pointcut("within(com.java.ex.*)")
```
- `com.java.ex.`하위의 모든 클래스의 모든 메소드에 적용하겠다는 의미
```java
@Pointcut("within(com.java.ex..*)")
```
- `com.java.ex.`패키지의 하위 패키지를 포함한 모든 클래스의 모든 메소드에 적용하겠다는 의미
```java
@Pointcut("within(com.java.ex.Car)")
```
- `com.java.ex.Car`클래스 안의 모든 메소드에 적용하겠다는 의미

>EX
```java
@Aspect
public class LogAop{
    @Around("within(com.java.ex.*)")
    public Object logging(ProceedingJoinPoint joinPoint) thorws Throwable {
        String methodName = joinPoint.getSignature().toShortString();
        try{
            System.out.println(methodName + "is Start.");
            Object obj = joinPoint.proceed();
            return obj;
        }finally {
            System.out.println(methodName + "is Finish.");
        }
    }
}    
```

#### Bean(bean id)
