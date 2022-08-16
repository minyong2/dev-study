# `Thread`
    메모리 할당받아 실행 중인 프로그램
    - 프로세스 내의 명령어 블록으로 시작점과 종료점을 가진다.
    - 실행중에 멈출 수 있으며 동시에 수행 가능
    
## Thread 클래스
    Thread()
    Thread(String s) // 스레드 이름
    Thread(Runnable r) //인터페이스 객체
    Thread(Tunnable r , String s) //인터페이스와 스레드 이름

## Thread 메소드
    static void sleep(long msec) throws Interrupted Exception //msec에 지정된 밀리초 동안 대기
    String getName()
    void setName(String s) // 스레드의 이름을 s로 설정
    void start() // 스레드를 시작 ,run()메소드 호출
    int getPriority() // 스레드의 우선 순위를 반환
    void setpriority(int p) // 스레드의 우선순위를 p값으로
    boolean isAlive() // 스레드가 시작되었고 아직 끝나지 않았으면 true, 끝났으면 false 반환
    void join() throws InterriptedException // 스레드가 끝날 때 까지 대기
    void run() // 스레드가 실행할 부분 기술 (오버라이딩 사용)
    void suspend() // 스레드가 일시정지 resume()에 의해 다시 시작 할 수 있다.
    void resume() // 일시 정지된 스레드를 다시 시작
    void yield() // 다른 스레드에게 실행 상태를 양보하고 자신은 준비 상태로

    