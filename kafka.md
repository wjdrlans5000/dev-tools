## 카프카

### 메시지큐란?
- 참고 URL  
  - https://blog.naver.com/PostView.nhn?blogId=dktmrorl&logNo=222117711303&categoryNo=0&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postView

### Kafka 설치 및 개념 (producer-consumer)
- 참고 URL
  - https://cobajiyoung.tistory.com/68
  - https://jjjwodls.github.io/etc/2020/01/07/01-Kafka-Setup.html
  - https://jessyt.tistory.com/131?category=966697
  - https://jaimemin.tistory.com/1901 : 관련 개념 및 모니터링 툴

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

### Flink
- 스트림 데이터를 받아 데이터를 가공하고 처리(저장)하는 스트리밍 & 배치 프로세싱 플랫폼
- 참고 URL : https://gyrfalcon.tistory.com/entry/Flink-1-%EC%86%8C%EA%B0%9C-Basic-Concept

### 컨슈머 랙(LAG)
- 운영 모니터링 지표 중 하나로 파티션에 데이터가 하나씩 들어가면 각 데이터에 오프셋이라는 숫자가 붙는다 (0부터)
- 그런데 프로듀서가 데이터를 넣어주는 속도보다 컨슈머가 데이터를 가져가는 속도가 더 빠르다면 프로듀서가 넣은 데이터의 오프셋과 컨슈머가 가져간 데이터의 오프셋 간의 차이가 발생하는데 이를 `컨슈머 랙` 이라고 한다.
- 이 렉의 숫자를 통해 해당 토픽에 대한 프로듀서와 컨슈머의 상태유추가 가능하다
- 토픽에 파티션이 여러 존재할 경우 LAG도 여러개 존재할수 있다.
- 즉 LAG가 마이너스 일 경우 프로듀서가 넣은 데이터보다 컨슈머가 더 많은 데이터를 가져간 상태 (비정상)
- 오프셋이 변하지 않는데 LAG이 고정되거나 증가하고 있으면 컨슈머는 ERROR 상태가 된다. 그리고 파티션은 STALLED라고 표시한다.
- 컨슈머의 오프셋이 증가하고 있지만 LAG이 고정되거나 증가하고 있으면 컨슈머는 WARNING 상태가 된다. WARNING 상태라는 것은 컨슈머가 느려서 점점 뒤쳐지고 있다는 것이다.
