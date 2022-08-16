# Iterator란?
    Iterator란 자바의 컬렉션 프레임워크에서 컬렉션에 저장되어 있는 요소들을 읽어오는 방법을 표준화 하였는데 그 중 하나이다.

    Iterator는 인터페이스!
    구성)
    public interface Iterator{
        boolean hasNext();
        Object next();
        void remove();
    }

    해석)
    boolean hasNext() 메소드는 읽어 올 요소가 남아있는지 확인하는 메소드 //있으면 true 없으면 false
    Object next() 메소드는 읽어 올 요소가 남아있는지 확인하는 메소드 // 있으면 true 없으면 false
    void remove() 메소드는 next() 로 읽어 온 요소를 삭제한다.
    next()를 호출한 다음에 remove()를 호출해야 한다.

    List 혹은 Set 인터페이스를 구현하는 컬렉션은 iterator()가 컬렉션의 특징에 맞게 설계가 되어있다.