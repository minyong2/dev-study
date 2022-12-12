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
☝ DB에 접속하여 저장된 경로 데이터 건수와 RAW 데이터 건수가 일치함을 확인
<niadb ip_sdn tb_link>
select count(id) from ip_sdn.tb_link tl

<UTC2 optroute link>
select COUNT(l.Id)  from link l 
```

## 3.1.1.4 KOREN IP SDN 운용 데이터 수집기능 고도화(실시간)
```
tb_syslog_collect
tb_sflow_collect                

☝수집서버의 로그를 통해서 syslog,sflow 데이터가 정상적으로 수집되고 있는지를 확인
 >> ipSdnSyslogLinkage
 >> ipSdnSflowLinkage  =====> log가 안나온당.
✌ 실시간으로 수집된 데이터가 DB에 저장되는지 확인한다.
 >> gather.tb_syslog_collect
```

## 3.2.1.1 장애 사전 인지를 위한 IP SDN 스위치 장비 장애 감시 기능(AP)
```
☝ Syslog 메시지별 경보 코드 매핑 데이터 구축 내용 확인
- AI-NMS DB에 접속하여 경보 코드 매핑 데이터를 조회한다.
>> gather.tb_syslog_rule_mst

✌ rca.tb_syslog_alarm_mst
- syslog 경보 생성 모듈 log를 통해 경보가 정상 출력됨을 확인한다
/server/logs$ tail -f syslogPreprocessor.날짜.log

```

## 3.3.1.1 경로 최적화에 필요한 데이터 수집(IP SDN 망에서 정보 수집을 위한 Prob 기능)
```
☝ DB에 접속하여 트래픽이 1분단위로 쌓이는지 확인
 - 1분단위로 링크/노드/트래픽 데이터 출력 확인
>> ip_sdn.tb_link , ip_sdn.node, ip_sdn.tb_link_traffic 
tail -f ipSdnLinkage_application_날짜.log
DB에서 데이터 보여주면 됨

👓링크랑 노드는 어떻게 보여주나? xshell에서 로그를 보여주면 되나

```

## 3.9.1.1 데이터 In/Out 덤프를 위한 API 기능
```
프레스토
☝ 데이터 다운로드 기능 => UI에서 다운로드 하는 기능이 있음
✌ 데이터 다운로드 기능 사용시의 프레스토 API 로그를 확인
 - 데이터 다운로드 기능 사용시의 프레스토 API 전처리 로그를 확인 => 어떤 로그?
하둡
☝ 데이터 다운로드 기능 => UI에서 다운로드 하는 기능이 있음
✌ 데이터 다운로드 기능 사용시의 하둡 API 로그를 확인
 - 데이터 다운로드 기능 사용시의 하둡 API 전처리 로그를 확인 => 어떤 로그?

```

## 3.12.1.3 외부 제공용 덤프 데이터 정확성
```
☝ 데이터 스냅샷 이력에 대한 덤프 데이터 일치여부 확인
☝-1 기 등록된 스냅샷 이력에서 임의로 5개를 선정하여 데이터를 덤프 받고, 해당 덤프 데이터가 원본 데이터와 일치함을 확인

👓 덤프 데이터?
👓 기 등록된 스냅샷 이력?
👓 어떻게 보여주나

```