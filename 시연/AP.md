```
마리아db
host : incodej-lab.iptime.org port : 3307
db : optroute?characterEncoding=UTF-8&serverTimezone=UTC
user : optroute
pw : optroutepw1004

optroute DB에 있는 데이터들과 비교하여 건수가 맞아야 함
쿼리 미리 준비하기 :)

시설 -> 건 수
수집 -> 한 타임

https://jsonformatter.org/#
👌🖖🖐
```

## 3.1.1.2 IP SDN 망 경로 데이터 구축
```
ANM-002
☝ DB에 접속하여 저장된 경로 데이터 건수와 RAW 데이터 건수가 일치함을 확인
<링크>
<niadb ip_sdn tb_link>
select count(id) from ip_sdn.tb_link tl

<UTC2 optroute link>
select COUNT(l.Id)  from link l 

```
![image](https://user-images.githubusercontent.com/97263974/207226736-1bd666d9-17bb-4768-bc8b-87160f7a43de.png)
## 3.1.1.3 KOREN IP SDN 운용 데이터 수집기능 고도화(1분주기)
```
ANM-003
<트래픽, 노드팩터>
1. 백본 및 중계 구간 인터페이스별 트래픽 수집 기능 확인
1.1 연동 대상 트래픽 원시 데이터와 연동 후 DB에 저장된 트래픽 데이터가 일치함을 확인
2. 장비 리소스 사용량 (CPU, Memory) 수집 기능 확인
2.1 연동 대상 원시 데이터와 연동 후 DB에 저장된 데이터가 일치함을 확인.

select count(*) from ip_sdn.tb_nodefactor tn 
where measured_datetime > '2022-12-14 15:35:54.000'

select count(*) from optroute.nodefactor n 
where MeasuredDateTime  > '2022-12-14 15:35:54.000'
```
![nodefactor](https://user-images.githubusercontent.com/97263974/207226967-d13d0a51-782a-46dd-84a0-a9e93156ceb7.png)
![traffic](https://user-images.githubusercontent.com/97263974/207227147-f3d27fb2-1543-4b3b-ae1c-08bb96b4bf47.png)


## 3.1.1.4 KOREN IP SDN 운용 데이터 수집기능 고도화(실시간)
```
ANM-004
tb_syslog_collect
tb_sflow_collect    

☝수집서버의 로그를 통해서 syslog,sflow 데이터가 정상적으로 수집되고 있는지를 확인
 >> ipSdnSyslogLinkage
 >> ipSdnSflowLinkage 
✌ 실시간으로 수집된 데이터가 DB에 저장되는지 확인한다.
 >> gather.tb_syslog_collect
 >> gather.tb_sflow_collect
```


![syslogDB](https://user-images.githubusercontent.com/97263974/207231618-dda624b4-93c9-4290-8e59-b8bbc30d1968.png)
![sflowDB](https://user-images.githubusercontent.com/97263974/207231406-f2f8de64-1a20-42b8-a480-798c7687c46e.png)


## 3.2.1.1 장애 사전 인지를 위한 IP SDN 스위치 장비 장애 감시 기능(AP)
```
ANM-005
☝ Syslog 메시지별 경보 코드 매핑 데이터 구축 내용 확인
- AI-NMS DB에 접속하여 경보 코드 매핑 데이터를 조회한다.
>> gather.tb_syslog_rule_mst

✌ rca.tb_syslog_alarm_mst
- syslog 경보 생성 모듈 log를 통해 경보가 정상 출력됨을 확인한다
/server/logs$ tail -f syslogPreprocessor.날짜.log

```
![syslogRule](https://user-images.githubusercontent.com/97263974/207227557-8da990b4-cc69-4ee5-b530-9ff1235e360b.png)
![syslogAlarmLog](https://user-images.githubusercontent.com/97263974/207227482-c8e1e96f-cb72-4925-bb29-3c450753597d.png)
## 3.12.1.1 IP SDN과의 연계 인터페이스 안정성
```
ANP-001
1. 수집 주기(1분) 간격으로 데이터 수집 정상 여부 확인(4종 수집: 트래픽, CPU, Memory)
1.1  데이터 건수: 1분 주기로 3종 데이터를 수집하므로, 60분간 180건을 수집함.
1.2 1시간동안 누락없이 데이터를 정상적으로 연계한 성공 건수 확인
1.3  성능 목표: 총 수집 180건 중, 100%인 180건을 정상적으로 연계함.

2. 실시간성 데이터 수집 정상 여부 확인 (Sflow)
2.1 실시간으로 연동된 수집서버의 로그와 DB에 저장된 데이터 건수를 확인하여 100% 데이터 수집을 하고 있는지 확인함.

select measured_datetime from ip_sdn.tb_nodefactor tn
where measured_datetime >= '2022-12-14 15:00:00.000' and measured_datetime < '2022-12-14 16:00:00.000'
group by measured_datetime 
```

## 3.12.1.3 외부 제공용 덤프 데이터 정확성
```
ANP-003
☝ 데이터 스냅샷 이력에 대한 덤프 데이터 일치여부 확인
☝-1 기 등록된 스냅샷 이력에서 임의로 5개를 선정하여 데이터를 덤프 받고, 해당 덤프 데이터가 원본 데이터와 일치함을 확인

👓 덤프 데이터?
👓 기 등록된 스냅샷 이력?
👓 어떻게 보여주나

```