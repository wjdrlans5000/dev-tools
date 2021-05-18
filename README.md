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
     - 
