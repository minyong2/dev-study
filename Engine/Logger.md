# JAVA
## Logger
### - 사용방법
    로깅 라이브러리는 slf4j를 사용하여 내가 `로그를 남길 클래스에 선언`을 해주고 메소드 내에서 사용하면 된다.

```java
public class MainController(){
    private final Logger LOGGER = LoggerFactory.getLogger(MainController.class.getName());
    
    public void moveMainPage(){
        LOGGER.info("Hello world!");
    }
}

이렇게 하면 console창에
[~~~~~.MainController] Hello world!
라고 찍히게 된다.
거의 에러메세지들을 많이 찍는듯.
```

### System.out.println이랑은 뭐가 다른가?
```
Sysout으로 출력시 console에 단지 Hello world만 찍히기 때문에
에러발생 시 추적할 수 있는 경로의 정보가 없다.
```
@ 참고로 로그 레벨은 TRACE > DEBUG > INFO > WARN > ERROR


