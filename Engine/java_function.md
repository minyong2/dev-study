## 자바 관련 함수
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