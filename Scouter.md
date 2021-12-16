# scouter
스카우트 설치 및 설정

- 스카우트 퀵스타트
  - https://github.com/scouter-project/scouter/blob/master/scouter.document/main/Quick-Start_kr.md

- 스카우트?
  - Open Source APM(Application Performance Management)으로 서버의 성능을 모니터링 하기위한 도구이다.
- 설치
  - Github(https://github.com/scouter-project/scouter/releases/) 에서 다운받을 수 있다.
  - 두개의 파일을 받는데 하나는 서버용이고, 하나는 클라이언트용이다.
    - 서버용 : scouter-all-[VERSION].tar.gz
    - 클라이언트 : scouter.client.product-[Client OS]
- 구성
  - Scouter(Collector) Server
    - 스카우터 수집서버. 모니터링할 정보들을 수집하는 서버이다.
    - 데이터는 자체 제작된 내부파일 DB에 저장된다.
    - 최근에 Collector API가 제공되고 있으며, Scouter Paper(웹 브라우저 클라이언트로 보임).
  - Scouter Agent
    - Agent는 HostAgent와 JavaAgent가 있고, 모니터링대상(WAS)에 설치한다.
    - 데이터는 UDP와 TCP로 전송된다.
    - Host Agent: CPU, MEMORY, DISK 정보를 전달한다.
    - Java Agent: Heap, TPS, Response Time, Service Profile 정보를 전달한다.
  - Scouter Client
     - 스카우터 클라이언트로 스카우터 서버에 접속하여 수집된 정보를 모니터링한다.
     -  모니터링 화면은 Eclipse기반의 UI로 구성되어 있다.

- tomcat 메뉴
  - **Elapsed 90%** : 전체 사용자의 90%가 받는 응답속도. 대부분의 사용자의 응답속도. 이게 좀 더 중요하다.
  - Elapsed Time(평균속도) : 크게 의미없다
  - Error Rate : 이건없어야 한다.
  - GC Count : 1초 간격으로 gc가 몇번 일어나는지(이게 올라가면..)
  - GC Time : GC 수행시간 (300ms-1s)
  - Heap used : 톱니모양이 좋은것.. 가장 최저점들의 노드를 연결해보면 우상향그래프가 될텐데 그럼 Full GC가 발생한다. 그리고 다시 아래부터 올라간다. 올라가면 안좋은거..
  - Heap Memory : Heap used 와 함께 Max 사용가능한 수치도 같이 볼수 있다.
  - ProcessCpu : JVM CPU 사용
  - Recent User : 최근(새로운)사용자. 믿기 어렵다. 구글 애널리틱스를 쓰자. 스카우터 쿠키를 구워서 다음에 왔을땐 새로운게 아니니까.. 믿지말자..
  - **TPS** : 트랜잭션 퍼 세컨드 (초당트랜잭션) CPU사용량과 밀접한 관계가 있다.
  - User Transaction : 하나의 행위(메일을쓰기, 하나의 페이지를 로딩)
  - Server Transaction : 하나의 행위를 위해 여러번 요청을 할때 각각 트랜잭션으로..
  - Active Service EQ : 쓰레드 정보. 빨간색 8초 응답, 노란색 4초 응답, 파란색은 OK
  - 문제가 생기면 더블클릭을 해서 상세내용을 보고 판단한다.
  - Active Service List : 톰캣에 돌고 있는 쓰레드(CPU잡아먹으니 장애났을떄만 본다)
  - Active Service Vertical EQ : 세로로 보여준거.
  - Active Speed : 이건 띄워놓는게 좋다. 장애가 나면 빨개진다. 장애여부를 바로볼수있다.
  - XLog : 스카우터에서 가장 메인으로 보게될 뷰로 요청과 프로파일정보들을 제공한다.
  - 24H Service Count : 일별 문제 카운트
  - Throughput : Thoughput을 임의로 지정해서 볼수 있다. url 기능마다 그룹지어놓으면 분석할 수 있다. 많은 수작업이 들어간다. 설정에..
  - Today visitor : 오늘 온사람 확인용도
  - Summary : 통계기능. 특정시간에 요청된 내용을 볼수 있음
- 로그파일
  - 저장경로: ${SCOUTER_HOME}/server/database
  - 한달이 넘거나 서버 디스크의 70%이상이면 가장 오래된로그부터 지운다.

- 스카우터 실행
  - /scouter/server/start.bat 실행하여 스카우트 서버 실행
  - /scouter/agent.host/host.bat 실행하여 host agent 실행
  - intellij vm optrions또는 tomcat server.xml에 -javaagent:"C:\scouter\agent.java\scouter.agent.jar" -Dscouter.config="C:\scouter\agent.java\conf\scouter.conf" -Dobj_name=scouterapptestWeb 추가
  - 스카우터 클라이언트 실행 admin/admin
