# Thread Pool
![thread-pool](https://user-images.githubusercontent.com/97263974/185004981-c0e9d37b-3d24-4ec0-9a16-10d830c34859.jpg)

- submitter : 제안자, 제출자
- executor Service: 집행 서비스(실행?서비스)
---
```
프로세스 내에서 스레드의 생성 및 수거가 빈번하게 발생한다면 메모리 할당에 소모되는 비용이 많이 들지 않을까? 
이런 문제의 방안으로 스레드풀을 사용한다!
스레드 풀은 매번 생성 및 수거 요청이 올 때 스레드를 생성하고 수거하는 것이 아닌, 스레드 사용자가 설정해둔 개수만큼 미리 생성해두는 것이다.
```

## 다시 알아보는 Thread의 개념
- 어떤 프로그램 내에서 실행되는 흐름의 단위
- 특히 프로세스 내에서 실행되는 흐름의 단위

## 풀(Pool)
- 필요할 때마다 개체를 할당하고 파괴하는 대신, 사용 준비된 상태로 초기화된 개체 집합

```
스레드를 생성,수거 할 때 커널 오브젝트를 동반하는 리소스이므로 생성 비용이 커 스레드를 무차별적으로 생성 할 시 리소스가 빠르게 소진될 수 있음 이를 방지하기 위해 thread pool을 사용한다.
```
1. 병렬 작업의 형태로 동시 코드를 작성한다.
2. 실행을 위해 스레드 풀의 인스턴스에 제출한다.
3. 제출한 인스턴스에서 실행하기 위해 재사용되는 여러 스레드를 제어한다.

### 장점
    비용적인 측면이나 컨텍스트 스위치가 발생하는 상황에서 딜레이를 줄일 수 있다.

### 단점
    스레드 풀에 너무 많은 양의 스레드를 만들어둔다면 메모리 낭비가 심해질 수 있다는 점
    그 때문에 얼만큼의 스레드가 필요할지 예측하고 할당해서 사용하는 것이 스레드 풀을 현명하게 사용하는 것이라고 할 수 있음.


### 컨텍스트 스위칭
    컨텍스트란?
    => CPU가 해당 프로세스를 실행하기 위한 해당 프로세스의 정보들.

### 커널 오브젝트



### java에서 thread pool 사용하기
```java
executorService = Executors.newWorkStealingPool(5);
                    executorService.invokeAll(perfFileCallables)
                            .stream()
                            .map(future -> {
                                try {
                                    return future.get();
                                }
                                catch (Exception e) {
                                    throw new IllegalStateException(e);
                                }
                            })
                            .filter(data -> data != null)
                            .forEach(list -> roadmOptPmList.addAll(list));

                    executorService.shutdown();
```
```java
코드 분석
executorService = Executors.newWorkStealingPool(5);
// thread pool 5개 생성
executorService.invokeAll(perfFileCallables)
//
.map(future -> {
                                try {
                                    return future.get();
                                }
                                catch (Exception e) {
                                    throw new IllegalStateException(e);
                                }
                            })
//

.filter(data -> data != null)
// stream 내 요소에 대해 필터링
// data가 null이 아니면
.stream()
//list를 돌림(람다표현식)
.forEach(list -> roadmOptPmList.addAll(list));
//list에 추가
xecutorService.shutdown();
// 모든 task 강제 종료
```

### Stream
```
stream은 데이터의 흐름.
배열 또는 컬렉션 인스턴스에 함수 여러 개를 조합해서 원하는 결과를 필터링하고 가공된 결과를 얻을 수 있음

병렬처리 가능
하나의 작업을 둘 이상의 작업으로 잘게 나눠서 동시에 진행하는 것을 병렬 처리라고 합니다.
즉 thread를 이용해 빠르게 처리할 수 있음.

.Stream()의 의미
=> List를 사용할 때 사용

```
---
### Future
```
Future는 비동기적인 연산의 결과를 표현하는 클래스
Future를 이용하면 멀티쓰레드 환경에서 처리된 어떤 데이터를 다른 쓰레드에 전달할 수 있습니다.
Futre 내부적으로 Thread-safe 하도록 구현되었기 때문에 `synchronized block(동기화 블록)`을 사용하지 않아도 된다.

Future 객체는 작업이 완료될 때까지 기다렸다가 최종 결과를 얻는 데 사용하며, 때문에 지연 완료(pending completion) 객체라고도 한다.
```
### Synchronized (동기화)
```
두 개 이상의 멀티쓰레드 환경에서 하나의 변수에 동시 접근을 할 때 경쟁상태(Race Condition)이 발생하지 않도록 한다.
만약 경쟁상태가 발생할 수 있는 코드 블럭을 synchronized 키워드로 감싸면, 하나의 쓰레드만 이 코드 블럭에 진입할 수 있다.
그 외에 다른 쓰레드는 먼저 진입한 쓰레드가 이 코드 블럭을 나갈 때 까지 기다리도록 하여 경쟁상태가 발생하지 않도록 합니다.

즉, 공유자원의 손상을 막기 위해 상호배제를 해야한다.
 어떤 스레드가 임계 영역을 실행하고 있는 도중 다른 스레드가 동일한 임계 영역을 실행하게 해서는 안된다.

상호배제는 공유 자원에 접근하는 임계 영역을 lock 하고 다 사용한 다음 다른 스레드가 접근할 수 있도록 unlock하는 것.
java에서 lock, unlock을 Synchronized 키워드를 사용해서 처리할 수 있다.
```

### invokeAll()
```
invokeAll() : 여러 작업을 동시에 실행한뒤 결과를 가져오는 메서드. 동시에 실행한 작업 중에 제일 오래 걸리는 작업만큼 시간이 걸린다.

invokeAny() : 여러 작업 중에 하나라도 먼저 응답이 오면 끝내는 메서드. 동시에 실행한 작업 중에 제일 짧게 걸리는 작업만큼 시간이 걸린다.
```


### Thread 간 메모리 공유
    스레드는 전역변수, heap 등의 프로세스 자원은 하나씩만 두고 서로 공유한다.
    그래서 공유자원에 대해 여러 스레드가 동시에 접근하는 것을 조심해야 한다.
    멀티스레딩 환경에서 나타낼 수 있는 문제로는 경쟁상태(race condition)가 있다.
    경쟁상태는 스레드의 실행 순서에 따라 결과가 달라지게 한다.


---
---
---
참고 코드 - dr.lauren infineraPerfHdlService.java

