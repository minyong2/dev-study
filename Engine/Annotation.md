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


