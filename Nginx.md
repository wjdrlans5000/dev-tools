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

### 설치 및 기본 설정
- centOS 기반
```
# nginx 공식 저장소 추가
sudo vim /etc/yum.repos.d/nginx.repo
# 파일에 아래 내용 추가
  [nginx]
  name=nginx repo
  baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
  gpgcheck=0
  enabled=1

# nginx 설치
sudo yum install nginx
```
- nginx 버전 확인 및 실행
```
# nginx 버전 확인
nginx -v

# nginx 시작
sudo /etc/init.d/nginx start
```
- /etc/nginx/nginx.conf
```
# worker 프로세스를 실행할 사용자 설정
# - 이 사용자에 따라 권한이 달라질 수 있다.
user  nginx;
# 실행할 worker 프로세스 설정
# - 서버에 장착되어 있는 코어 수 만큼 할당하는 것이 보통, 더 높게도 설정 가능
worker_processes  1;

# 오류 로그를 남길 파일 경로 지정
error_log  /var/log/nginx/error.log warn;
# NGINX 마스터 프로세스 ID 를 저장할 파일 경로 지정
pid        /var/run/nginx.pid;


# 접속 처리에 관한 설정을 한다.
events {
    # 워커 프로레스 한 개당 동시 접속 수 지정 (512 혹은 1024 를 기준으로 지정)
    worker_connections  1024;
}

# 웹, 프록시 관련 서버 설정
http {
    # mime.types 파일을 읽어들인다.
    include       /etc/nginx/mime.types;
    # MIME 타입 설정
    default_type  application/octet-stream;

    # 엑세스 로그 형식 지정
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    # 엑세스 로그를 남길 파일 경로 지정
    access_log  /var/log/nginx/access.log  main;

    # sendfile api 를 사용할지 말지 결정
    sendfile        on;
    #tcp_nopush     on;

    # 접속시 커넥션을 몇 초동안 유지할지에 대한 설정
    keepalive_timeout  65;

    # (추가) nginx 버전을 숨길 수 있다. (보통 아래를 사용해서 숨기는게 일반적)
    server_tokens off

    #gzip  on;

    # /etc/nginx/conf.d 디렉토리 아래 있는 .conf 파일을 모두 읽어 들임
    include /etc/nginx/conf.d/*.conf;
}
```
- 설정 파일 반영을 위한 reload
```
# ubuntu
$ service nginx reload;

# centOS
$ systemctl start nginx
```
- 로드밸런싱 설정, /etc/nginx/conf.d 디렉토리 아래 .conf 파일을 생성
- 80 포트로 들어왔을때 upstream의 3개의 서비스로 로드밸런싱
```
server {
  listen  80;
  server_name {nginx server ip 혹은 domain 설정};

  access_log /var/log/nginx/proxy/access.log;
  error_log /var/log/nginx/proxy/error.log;

  location / {
    include /etc/nginx/proxy_params;
    proxy_pass http://express-app;
  }
}

upstream express-app {
  # least-connected 설정 (아무 설정을 하지 않으면 라운드 로빈 방식으로 로드밸런싱)
  least_conn;

  server 127.0.0.1:7000;
  server 127.0.0.1:8000;
  server 127.0.0.1:9000;
}

```

### 로드밸런싱 방식
- round-robin: 라운드 로빈방식으로 서버를 할당
- least-connected: 커넥션이 가장 적은 서버를 할당
- ip-hash: 클라이언트 IP를 해쉬한 값을 기반으로 특정 서버를 할당

### upstream 서버 디렉티브 옵션 설정
- weight=n: 업스트림 서버의 비중을 나타낸다. 이 값을 2로 설정하면 그렇지 않은 서버에 비해 두배 더 자주 선택된다. (server 127.0.0.1:7000 weight=2 )
- max_fails=n: n으로 지정한 횟수만큼 실패가 일어나면 서버가 죽은 것으로 간주한다. (server 127.0.0.1:7000 max_fails=3)
- fail_timeout=n: max_fails가 지정된 상태에서 이 값이 설정만큼 서버가 응답하지 않으면 죽은 것으로 간주한다. (server 127.0.0.1:7000 fail_timeout=30)
- down: 해당 서버를 사용하지 않게 지정한다. ip_hash; 지시어가 설정된 상태에서만 유효하다. (server 127.0.0.8000 down)
- backup: 모든 서버가 동작하지 않을 때 backup으로 표시된 서버가 사용되고 그 전까지는 사용되지 않는다. (server 127.0.0.1:9000 backup)
