
## 배포
### log경로 설정
![logback-spring](https://user-images.githubusercontent.com/97263974/197952540-b02a6689-d751-4ab4-9004-c4a958448a0b.png)
---
## vi 명령어
![vi명령어](https://user-images.githubusercontent.com/97263974/197971971-a28a0b1d-a2b6-4122-ae95-fa0b6f607f13.png)
```
ls : list
cd : 이동
cp : 카피
    cp [복사대상] [복사할이름]
ex) cp ipSdnSflowLinkage.sh ipSdnSyslogLinkage.sh

jps: Java Virtual Machine 목록을 보여줌


```
## vi명령어 모드
```
명령 모드(Command Mode)
입력 모드(Insert Mode)
마지막 행 모드(Last Line Mode)
```
# 🍳🍳🍳
```
jar 파일 배포 후 sh 만들어주고 
(보통 cp 기존.sh 새로운.sh 로 카피해서 만듦)
ㄴ> vi @@@@@.sh 들어가서 새로운 이름으로 전부 수정해주고
:wq! 로 저장 후 나오기
==> sh파일 이미 존재하면 생략


sh 새로운.sh start 후 jps로 돌아가는지 확인
jps 목록에 있어도 없어도
logs에 가서 로그 확인
vi 새로운.application.현재날짜.log  >> shift+g+g (g두번)  으로도 확인 가능
그리고 tail -f 새로운.log 에서 수집 잘 되는지 확인하기 🍳
```