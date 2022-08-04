### 카프카
1) Kafka 설치 및 개념 (producer-consumer)
https://jjjwodls.github.io/etc/2020/01/07/01-Kafka-Setup.html

2) zookeeper 수행
D:\Kafka\kafka_2.12-2.6.0\bin\windows\zookeeper-server-start.bat D:\Kafka\kafka_2.12-2.6.0\config\zookeeper.properties

3) kafka 수행
D:\Kafka\kafka_2.12-2.6.0\bin\windows\kafka-server-start.bat D:\Kafka\kafka_2.12-2.6.0\config\server.properties

4) topic 생성
D:\Kafka\kafka_2.12-2.6.0\bin\windows\kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic ClickViewData

5) 생성된 토픽 list 확인
D:\Kafka\kafka_2.12-2.6.0\bin\windows\kafka-topics.bat --list --zookeeper localhost:2181

6) Consumer 생성 - 생략
kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic ClickViewData

7) Producer 생성
kafka-console-producer.bat --broker-list localhost:9092 --topic ClickViewData

8) Consumer 프로젝트 실행
spring.kafka.bootstrap-servers=localhost:9092 ( 설치된 카프카 IP:PORT )

9) Topic 메시지 전송
