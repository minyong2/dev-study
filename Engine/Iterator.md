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

---
### (루프 도중에 아이템을 오류없이 삭제하는 방법)
```
iterate : (계산,컴퓨터 처리 절차를) 반복하다
iterator : 반복자

프로그래밍에서 반복기는 개발자가 컨테이너, 특히 리스트를 순회할 수 있게 해주는 객체다.
반복자는 객체 지향적 프로그래밍에서 배열이나 그와 유사한 자료구조의 내부요소를 순회하는 객체이다.
iterator는 ArrayList, HashSet과 같은 컬렉션을 반복하는 데 사용할 수 있는 객체다.
```
### 장점
    1. 컬렉션에서 요소를 제어하는 기능
    2. next() 및 previous()를 써서 앞 뒤로 이동하는 기능
    3. hasNext()를 써서 더 많은 요소가 있는지 확인하는 기능

### 사용 이유
```java
public void removeCluster(String tmpClusterNo){
        try{
            LOGGER.info("=====> [ClusterService] removeCluster tmpClusterObject("+tmpClusterNo+") <=====");
            Iterator<TmpClusterObject> tmpItr;
            Iterator<AlarmClusterService> actItr;
            TmpClusterObject tmpClusterObject;
            if(tmpClusterList.size() > 0){
                tmpItr = tmpClusterList.iterator();
                while( tmpItr.hasNext() ) {
                    tmpClusterObject = tmpItr.next();
                    if(tmpClusterObject.getTmpClusterSeq().equals(tmpClusterNo)){
                        tmpItr.remove();
                        //tmpClusterList.remove(tmpItr);
                    }
                }
            }
```
```
데이터를 List에 담아서 조건에 맞지 않는 데이터는 삭제하려고 할 때,
루프(loop)를 돌면서 체크해서 삭제를 하는데, 일반적인 for 루프를 사용하면 예외 발생

*어떤 예외?
==> list의 크기와 처리해야할 항목을 가리키는 index의 불일치 때문에 에러 , 혹은 논리적인 오류 발생

이런 에러는 index를 사용하는 for 루프를 사용해서 없어진 만큼 사이즈와 인덱스를 직접 조정 할 수 있지만 Iterator를 사용하면 더 편리하게 코딩할 수 있음
```

```java
<📆221212 추가>

public void createSdnTrafficTicket(SdnTrafficListVo sdnTrafficListVo) {
 Iterator<SdnTrafficVo> itr;

try{
            if(sdnTrafficListVo != null && sdnTrafficListVo.getData().size() > 0) {
                itr = sdnTrafficListVo.getData().iterator();

                while (itr.hasNext()) {
                    sdnTrafficVo = itr.next();
        // iterator를 사용하여 list가 변경되어도 오류가 나지 않도록 함

                    parameterMap = new HashMap<String, String>();
                    parameterMap.put("strifid", sdnTrafficVo.getStrifid());
                    parameterMap.put("measured_datetime", sdnTrafficVo.getMeasured_datetime() + "");


                    sdnTrafficInfoVo = sdnTrafficMapper.selectSdnTrafficAlarm(parameterMap);

                    if (sdnTrafficInfoVo != null) {
                        sdnTrafficVo.setStrifid(sdnTrafficInfoVo.getStrifid());
                        sdnTrafficVo.setStrresid(sdnTrafficInfoVo.getStrresid());
                    }
                ...}
            ...}
        ...}
    ...}

```