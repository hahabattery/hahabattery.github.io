---
layout: page
title: Kafka
description: I will provide some tips on using Kafka and share insights on developing programs with it.
permalink: /messaging/kafka
author: wonchang
category: messaging
---

[UI for Apache Kafka](https://github.com/provectus/kafka-ui)

* kafka-ui를 이용하는 내용
  + https://devocean.sk.com/blog/techBoardDetail.do?ID=163980



 * consumer에서 같은 그룹으로 여러 클라이언트가 동작하는 경우에는 , partition을 각 클라이언트가 분배해서 처리하게 된다.

### Start Zookeeper and Kafka
 * config/zookeeper.properties 설정
   * dataDir에 zookeeper 데이터가 들어갈 디렉토리지정
 * zookeeper 기동
   * bin/zookeeper-server-start.sh config/zookeeper.properties


 * config/server.properties 설정
   * log.dirs 에 kafka 데이터가 들어갈 디렉토리 지정

 * kafka 기동
   * kafka-server-start.sh ./config/server.properties

### kafka cli

 * topic 생성
   * kafka-topics.sh --zookeeper 127.0.0.1:2181 --topic first_topic --partitions 3  --replication-factor 1 --create
   * --replication-factor는 broker개수보다 클 수 없다.

 * 리스트 조회
   * kafka-topics.sh --zookeeper 127.0.0.1:2181 --list

 * topic 정보 조회
   * kafka-topics.sh --zookeeper 127.0.0.1:2181 --topic first_topic --describe

 * topic 삭제
   * kafka-topics.sh --zookeeper 127.0.0.1:2181 --topic second_topic --delete


 * console producer
   * kafka-console-producer.sh --broker-list 127.0.0.1:9092 --topic first_topic
   * kafka-console-producer.sh --broker-list 127.0.0.1:9092 --topic first_topic --producer-property acks=all



 * 존재하지 않는 topic에 대해서 produce 하는 경우에는 default로 topic이 만들어진다.
```
wcsong@ubuntu_test:~/kafka/kafka_2.12-2.0.0$ kafka-console-producer.sh --broker-list 127.0.0.1:90 --topic new_topic
another message

[2019-12-26 13:07:31,546] WARN [Producer clientId=console-producer] Error while fetching metadata with correlation id 1 : {new_topic=LEADER_NOT_AVAILABLE} (org.apache.kafka.clients.NetworkClient)

wcsong@ubuntu_test:~/kafka/kafka_2.12-2.0.0$ kafka-topics.sh --zookeeper 127.0.0.1:2181 --list
first_topic
new_topic
```

 * 디폴트 설정
   * config/server.properties의 num.partitions=3
     * **topic이 없는 상황에서 topic에 대해서 produce된 경우에** 적용되는 default 설정


 * kafka-console-consumer를 실행해보지만, 메세지가 전달되지 않았다.
   * kafka-console-consumer.sh  --bootstrap-server 127.0.0.1:9092 --topic first_topic
   * console consumer는 새 메세지만 받는다.


 * --from-beginning 옵션을 추가해야지 처음부터 받는다.
   * kafka-console-consumer.sh  --bootstrap-server 127.0.0.1:9092 --topic first_topic


 * --group <그룹명> 으로 여러 console consumer에서 그룹지정을 하면,
 * 그룹에 속한 console consumer들이 나눠서 수신한다.
 * group 에 참여한 consumer수가 줄어들면 줄어든 consumer들이 알아서 수신한다.
   * kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --topic first_topic --group my-first-application


 * 그룹이릅(my_second_application)을 다르게 해서 --from-beginning 옵션을 주는 경우에 topic 의 기존 메세지들을 수신한다.
   * kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --topic first_topic --group my-second-application --from-beginning
   * offset은 그룹이름별로 관리된다.


 * 그룹이름(my_second_application)을 같이 해서 다시 실행하면, 이번에는 메세지를 수신하지 않게 된다.
   * offset이 commit 된 것.


 * 다시 kafka-console-consumer.sh를 정지시키고, producer에서 메세지를 topic에 넣는다.
 * kafka-console-consumer.sh를 시작시키면, 정진된 상태에서 producer가 topic에 입력한 메세지들을 볼 수 있다.
   * offset부터 메세지를 수신한 것

 * kafka-console-consumer.sh 프로그램은 --group 을 지정하지않으면, 항상 새로운 메세지만 받는 듯
   * --from-beginning을 지정하면, 기존 메세지도 받는다.
   * --from-beginning 옵션을 지정하면, 메세지를 처음부터 다시 받는다.

 * consumer group 리스트를 출력해보면, 생각 보다 많은 consumer그룹이 있는 것을 알 수 있다.
   * kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list
   *
```
/*
 * consumer-groups.sh 명령으로 그룹명(my-first-application)을 지정해서 상태를 볼 수 있다.
 */
@ubuntu_test:~/kafka/kafka_2.12-2.0.0$ kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group my-first-application
Consumer group 'my-first-application' has no active members.

TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID     HOST            CLIENT-ID
first_topic     0          9               10              1               -               -               -
first_topic     2          9               10              1               -               -               -
first_topic     1          10              11              1               -               -               -

/*
 * consumer group(my-first-application)을 다시 시작하면 LAG으로 표시되던 3개의 메세지를 받는것을 확인 할 수 있다.
 */
$ kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --topic first_topic --group my-first-application
22
21
20

/*
 * myconsole consumer
 */
@ubuntu_test:~/kafka/kafka_2.12-2.0.0$ kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group my-first-application

TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID                                     HOST            CLIENT-ID
first_topic     0          10              10              0               consumer-1-0436bf05-3fba-4d70-a058-27e7e31cedb8 /192.168.0.15   consumer-1
first_topic     1          11              11              0               consumer-1-0436bf05-3fba-4d70-a058-27e7e31cedb8 /192.168.0.15   consumer-1
first_topic     2          10              10              0               consumer-1-0436bf05-3fba-4d70-a058-27e7e31cedb8 /192.168.0.15   consumer-1


```


 * kafka-consumer-groups.sh 프로그램으로 offset을 reset시킬수 있다.
   * kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group my-first-application --reset-offsets --to-earliest --execute --topic first_topic
 ```
@ubuntu_test:~/kafka/kafka_2.12-2.0.0$ kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group my-first-application --reset-offsets --to-earliest --execute --topic first_topic

TOPIC                          PARTITION  NEW-OFFSET
first_topic                    2          0
first_topic                    1          0
first_topic                    0          0

@ubuntu_test:~/kafka/kafka_2.12-2.0.0$ kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group my-first-application
Consumer group 'my-first-application' has no active members.

TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID     HOST            CLIENT-ID
first_topic     0          0               10              10              -               -               -
first_topic     2          0               10              10              -               -               -
first_topic     1          0               11              11              -               -               -
```



 * kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group my-first-application --reset-offsets --shift-by -2 --execute --topic first_topic
   * 2씩 이전으로 돌아가기 때문에 6개를 다시 수신하게 됨.
```
@ubuntu_test:~/kafka/kafka_2.12-2.0.0$ kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group my-first-application --reset-offsets --shift-by -2 --execute --topic first_topic

TOPIC                          PARTITION  NEW-OFFSET
first_topic                    2          8
first_topic                    1          9
first_topic                    0          8

@ubuntu_test:~/kafka/kafka_2.12-2.0.0$ kafka-console-consumer.sh --bootstrap-server 127.0.0.1:9092 --topic first_topic --group my-first-application
test3
22
test2
21
test
20
```

### client configuration
 * configure producer:
   * https://kafka.apache.org/documentation/#producerconfigs

 * configure consumers:  
   * https://kafka.apache.org/documentation/#consumerconfigs



### 유용한 기타 명령들

 * console producer (key, value 형식)
```command
kafka-console-producer.sh --broker-list 127.0.0.1:9092 --topic first_topic --property parse.key=true --property key.separator=,
key,value
another key,another value
```

 * console consumer (key, value 형식)
```
kafka-console-consumer --bootstrap-server 127.0.0.1:9092 --topic first_topic --from-beginning --property print.key=true --property key.separator=,
key1, value1
```


---
# Version Compatability

 * version compatibility
   + Because API calls is versioned, broker and client application have bi-directional compatibility.
   + old client(v1.1) can communicate with new broker(v2.0)
   + new client(v2.0) can communicate with old broker(v1.1)
