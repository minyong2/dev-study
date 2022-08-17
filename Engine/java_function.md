## 자바 관련 함수
    코드 분석 중 모르는 혹은 더 알고싶은 함수들을 정리
### String 관련 함수
    - boolean startsWith(String prefix)
    - boolean contains(CharSequence s)
    - String[] split(String regex)
```java
if(basicAlarmVo.getAlarmloc().startsWith("S")){
    if(basicAlarmVo.getAlarmloc().contains("-")){
        ptpName = basicAlarmVo.getSysname().split("-")[1] + "-"+ basicAlarmVo.getAlarmloc().split("-")[0];

        basicAlarmVo.setSlot(basicAlarmVo.getAlarmloc().split("-")[0]);
        basicAlarmVo.setPort(basicAlarmVo.getAlarmloc().split("-")[1]);
            else{
                    ptpName = basicAlarmVo.getSysname().split("-")[1] + "-"+ basicAlarmVo.getAlarmloc();
                        basicAlarmVo.setSlot(basicAlarmVo.getAlarmloc());
                    }
                }
```

### isEmpty(),isBlank()
    isEmpty()
    - 문자열의 길이가 0인 경우에, true를 리턴

    isNotEmpty()
    - 문자열의 길이가 0인 경우 false를 리턴
    StringUtils.isNotEmpty(null) //false
    StringUtils.isNotEmpty("") //false
    StringUtils.isNotEmpty("value") //true
    StringUtils.isNotEmpty(" ") //true

    ex)
    String example = "value"
    if(!StringUtils.isEmpty(Service.example)){   
    } //true
    if(StringUtils.isNotEmpty(Service.example)){   
    } //true
    * !isEmpty == isNotEmpty 인거시다.

    isBlank()
    - 문자열이 비어 있거나, 빈 공백으로만 이루어져 있으면 true를 리턴

```java
public class StringEmptyBlank {    
    public static void main(String[] args) {         
        System.out.println("Hello".isEmpty() + "," + "Hello".isBlank()); 
        // false, false        
        System.out.println("  Hello  ".isEmpty() + "," + "  Hello  ".isBlank()); 
        // false, false        
        System.out.println("".isEmpty() + "," + "".isBlank()); 
        // true, true        
        System.out.println("  ".isEmpty() + "," + "  ".isBlank()); 
        // false, true     }}
```
### Iterator

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

