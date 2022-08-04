## 카프카

### 메시지큐란?
- 참고 URL  
  - https://blog.naver.com/PostView.nhn?blogId=dktmrorl&logNo=222117711303&categoryNo=0&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postView

### Kafka 설치 및 개념 (producer-consumer)
- 참고 URL
  - https://cobajiyoung.tistory.com/68
  - https://jjjwodls.github.io/etc/2020/01/07/01-Kafka-Setup.html
  - https://jessyt.tistory.com/131?category=966697

1) zookeeper 수행
D:\Kafka\kafka_2.12-2.6.0\bin\windows\zookeeper-server-start.bat D:\Kafka\kafka_2.12-2.6.0\config\zookeeper.properties

2) kafka 수행
D:\Kafka\kafka_2.12-2.6.0\bin\windows\kafka-server-start.bat D:\Kafka\kafka_2.12-2.6.0\config\server.properties

3) topic 생성
D:\Kafka\kafka_2.12-2.6.0\bin\windows\kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic ClickViewData

4) 생성된 토픽 list 확인
D:\Kafka\kafka_2.12-2.6.0\bin\windows\kafka-topics.bat --list --zookeeper localhost:2181

5) Consumer 생성 - 생략
kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic ClickViewData

6) Producer 생성
kafka-console-producer.bat --broker-list localhost:9092 --topic ClickViewData

7) Consumer 프로젝트 실행
spring.kafka.bootstrap-servers=localhost:9092 ( 설치된 카프카 IP:PORT )

8) Topic 메시지 전송

### 카프카 컨슈머 관련 정보
- @KafkaListener는 KafkaListenerEndpointRegistry에서 Bean으로 등록되고 등록된 Bean은 framework에 의해 자동으로 선언되고 Container의 Lifecycle을 관리한다.
- @KafkaListner를 이용한 Consumer 구현
  - 참고 URL : https://jessyt.tistory.com/146
- 스프링 카프카 라이프사이클 참고 URL : https://jessyt.tistory.com/151?category=966697
- 컨슈머가 동일한 데이터를 중복해서 가져오는것을 방지하기 위하여 OFFSET을 커밋하여 이를 방지한다.
- OFFSET COMMIT을 오토로 설정하지 않아 컨슈머가 로직처리중 EXCEPTION이 발생하더라도 Consumer 내부에서 Partition Offset과는 별개로 Consumer만을 위한 Offset을 관리하고 있기 때문에 무한루프에 빠지지 않는다 
- 카프카 OFFFSET 관련 참고 URL : https://jessyt.tistory.com/167
