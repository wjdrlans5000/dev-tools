## Nginx

### WEB Server 란?
- 클라이언트 요청에 따라 HTML,CSS,JS, 이미지 파일 같은 정적 파일을 응답하여 제공하는 소프트웨어. HTTP 프로토콜 사용하여 통신
- 대표 웹 서버 : Nginx, Apache, IIS 등

### Nginx
- Nginx란 트래픽이 많은 웹사이트의 서버(WAS)를 도와주는 비동기 이벤트 드라이븐 구조의 경량화 웹 서버 프로그램
- 목적 또는 역할 )
  - 정적 파일 응답 HTTP WEB Server
  - Reverse Proxy Server(프록시 서버가 애플리케이션 서버 앞에 위치하여 클라이언트의 요청을 중개) 로써 WAS 로드밸런서 역할

### Nginx의 구조
- Nginx는 설정파일을 읽고, 설정에 맞게 worker process를 생성하는 master process가 있음
- worker process는 실제로 일을 하는 프로세스이고, worker process가 만들어질 때 지정된 listen 소켓을 배정
- 그 소켓에 새로운 클라이언트의 요청이 들어오면 connection을 형성하고 처리
- connection은 정해진 Keep-Alive 시간만큼 유지 but, connection이 형성되었다고 해서 worker process가 해당 connection 하나만 담당하지는 않는다.
- 형성된 connection으로부터 아무런 요청이 없다면 새로운 connection을 형성하거나 이미 만들어진 다른 connection으로부터 들어온 요청을 처리
- Nginx에서는 이러한 connection 형성과 제거, 그리고 새로운 요청을 처리하는 것을 이벤트(event)라고 한다.
- 이벤트들은 os커널이 queue형식으로 worker process에게 전달, 이벤트들은 queue에 담긴 상태에서 비동기 상태로 대기
- worker process는 하나의 스레드로 이벤트를 꺼내서 처리해 나가고, 이런 방식은 worker process가 쉬지 않고 일을 하기에, 요청이 없을 때 프로세스를 방치시키는 Apache Server보다 훨씬 효율적으로 자원을 사용할 수 있다.
  ![image](https://github.com/wjdrlans5000/dev-tools/assets/62735399/fce18c3f-9dbe-4134-9973-86bce9ec6d41)
