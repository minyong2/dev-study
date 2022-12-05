# Kafka(카프카)
### 큐! Queue 의 일종



### pom.xml
```
        <dependency>
            <groupId>org.springframework.kafka</groupId>
            <artifactId>spring-kafka</artifactId>
            <version>2.7.3</version>
        </dependency>
```
---
### 구성요소
- Event : Kafka에서 Producer와 Consumer가 데이터를 주고받는 단위
- Producer : Kafka에 이벤트를 게시(post)하는 클라이언트 어플리케이션
- Consumer : Topic을 구독하고 이로부터 얻어낸 이벤트를 처리하는 클라이언트 어플리케이션
- Topic : 이벤트가 쓰이는 곳
```
Producer는 Topic에 이벤트 게시
Consumer는 Topic으로 부터 이벤트를 가져와 처리
```

#### partition
    Topic은 여러 브로커에 분산되어 저장되며, 이렇게 분산된 Topic을 `partition`이라고 한다.
    어떤 이벤트가 partition에 저장될지는 이벤트의 Key에 의해 정해지며, 같은 키를 가지는 이벤트는 항상 같은 partition에 저장됩니다.


### 주요 개념
- Producer와 Consumer의 분리
    ```

    Kafka의 Producer와 Consumer는 별개로 동작한다.
    Producer는 Broker의 Topic에 메세지를 게시하기만 하며
    Consumer는 Broker의 특정 Topic에서 메세지를 가져와 처리하기만 하면 된다.

    이 덕에 높은 확장성을 제공한다.

    ```

---
### Commit과 Offset
![image](https://user-images.githubusercontent.com/97263974/204205223-61cc78f2-acc6-486a-b5ce-b6f931dcc44c.png)
```
메세지는 지정된 Topic에 전달된다.
Topic은 여러 파티션으로 구성돼있으며, 파티션의 한칸한칸은 로그다.
메세지는 로그에 순차적으로 append 되며 이 메세지의 상대적인 위치를 offset이라고 칭한다.
```
```
Consumer의 poll()은 이전에 commit한 offset이 존재하면, 해당 offset 이후의 메세지를 읽어오게 된다.
읽어온 뒤 마지막 offset을 commit한다.
이어서 poll()이 실행되면 방금전 commit한 offset 이후의 메세지를 읽어와 처리하게 된다.
```
